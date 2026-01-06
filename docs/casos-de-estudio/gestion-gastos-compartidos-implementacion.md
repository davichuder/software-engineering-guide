# Guía de Implementación Técnica: Gestión de Gastos Compartidos

> **Contexto**: Documento técnico para desarrolladores. Acompaña al [Diseño Funcional](./gestion-gastos-compartidos.md).
>
> **Stack**: Java 21 (Spring Boot 3.2), Angular 17, PostgreSQL 16, Kafka, Docker.

---

## 1. Estructura de Proyectos (Microservicios)

Adoptamos **Arquitectura Hexagonal (Ports & Adapters)** para desacoplar el dominio de la infraestructura.

### Árbol de Carpetas (`ms-expenses`)

```text
ms-expenses/
├── src/
│   ├── main/
│   │   ├── java/com/fintech/expenses/
│   │   │   ├── domain/                         # -- CORE --
│   │   │   │   ├── model/                      # Entidades anémicas NO, Ricas SI
│   │   │   │   │   ├── Expense.java
│   │   │   │   │   ├── Split.java
│   │   │   │   │   └── Money.java (Value Obj)
│   │   │   │   ├── port/                       # Interfaces (Puertos)
│   │   │   │   │   ├── inbound/                # Casos de Uso (API del Core)
│   │   │   │   │   │   └── ProcessReceiptUseCase.java
│   │   │   │   │   └── outbound/               # Repositorios/Clientes
│   │   │   │   │       ├── ExpenseRepository.java
│   │   │   │   │       └── OcrClient.java
│   │   │   │   ├── service/                    # Lógica de Negocio
│   │   │   │   │   └── ExpenseService.java
│   │   │   ├── application/                    # -- ORCHESTRATION --
│   │   │   │   └── dto/
│   │   │   ├── infrastructure/                 # -- ADAPTERS --
│   │   │   │   ├── persistence/                # Database
│   │   │   │   │   ├── entity/ (JPA Entities)
│   │   │   │   │   └── JpaExpenseAdapter.java
│   │   │   │   ├── messaging/                  # Kafka
│   │   │   │   │   └── KafkaReceiptConsumer.java
│   │   │   │   └── config/                     # Spring Configuration
│   │   │   └── presentation/                   # -- API --
│   │   │       ├── controller/
│   │   │       └── GlobalExceptionHandler.java
```

---

## 2. Definición de Contratos (Parsing)

Para manejar formatos heterogéneos (CSV, Excel), definimos una interfaz canónica interna.

```java
// Contrato interno unificado para cualquier importador
public record BankTransactionDto(
    LocalDate date,
    String description,
    BigDecimal amount,
    String currency,
    String referenceId, // ID único del banco para deduplicación
    String rawCategory  // Categoría original del banco (ej: "MCC 5411")
) {}

// Interface del Parser
public interface BankStatementParser {
    boolean supports(String mimeType, String fileName);
    List<BankTransactionDto> parse(InputStream content);
}
```

---

## 3. Ejemplos de Implementación Backend

### 3.1 Entidad Rica (Domain)

Encapsula lógica de negocio y validaciones.

```java
public class Expense {
    private ExpenseId id;
    private Money amount;
    private List<Split> splits;
    private ExpenseStatus status;

    // Factory method
    public static Expense createShared(Money total, List<Split> splits) {
        validateTotalMatchesSplits(total, splits);
        return new Expense(ExpenseId.newIdentity(), total, splits, ExpenseStatus.PENDING);
    }

    private static void validateTotalMatchesSplits(Money total, List<Split> splits) {
        Money sum = splits.stream().map(Split::amount).reduce(Money.ZERO, Money::add);
        if (!total.equals(sum)) {
            throw new DomainException("La suma de los splits no coincide con el total. Diferencia: " + total.minus(sum));
        }
    }
    
    // Business Logic
    public void approve(UserId approverId) {
        if (this.status != ExpenseStatus.PENDING) {
            throw new DomainException("Solo gastos pendientes pueden aprobarse");
        }
        // Logica adicional de permisos...
        this.status = ExpenseStatus.APPROVED;
    }
}
```

### 3.2 Puerto de Repositorio (Port)

Interfaz definida en el dominio, implementada en infraestructura.

```java
public interface ExpenseRepository {
    Expense save(Expense expense);
    Optional<Expense> findById(ExpenseId id);
    List<Expense> findByGroup(GroupId groupId, DateRange range);
}
```

### 3.3 Adaptador JPA (Adapter)

Implementación concreta usando Spring Data JPA.

```java
@Component
@RequiredArgsConstructor
public class JpaExpenseAdapter implements ExpenseRepository {
    
    private final SpringDataExpenseRepository jpaRepo; // Interface interna de Hibernate
    private final ExpenseMapper mapper; // MapStruct

    @Override
    public Expense save(Expense domainExpense) {
        ExpenseEntity entity = mapper.toEntity(domainExpense);
        ExpenseEntity saved = jpaRepo.save(entity);
        return mapper.toDomain(saved);
    }
}
```

---

## 4. Testing (Pirámide de Pruebas)

### 4.1 Test Unitario (Dominio)

Rápido, sin Spring, prueba lógica pura.

```java
class ExpenseTest {
    @Test
    void should_throw_error_when_splits_dont_sum_up() {
        Money total = Money.of(100);
        Split s1 = new Split(userA, Money.of(50));
        Split s2 = new Split(userB, Money.of(49.99)); // Faltan 0.01

        assertThrows(DomainException.class, () -> 
            Expense.createShared(total, List.of(s1, s2))
        );
    }
}
```

### 4.2 Test de Integración (Testcontainers)

Prueba el adapter real contra una BD Postgres real efímera.

```java
@SpringBootTest
@Testcontainers
class JpaExpenseAdapterIT {
    
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @Autowired JpaExpenseAdapter adapter;

    @DynamicPropertySource
    static void setProps(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Test
    void should_persist_and_retrieve_expense() {
        Expense expense = ExpenseMother.random(); // Object Mother pattern
        adapter.save(expense);

        Optional<Expense> found = adapter.findById(expense.getId());
        assertThat(found).isPresent();
        assertThat(found.get().getAmount()).isEqualTo(expense.getAmount());
    }
}
```

---

## 5. Frontend Angular (Moderno)

### 5.1 Service con Signals

Gestión de estado reactiva y granular.

```typescript
@Injectable({ providedIn: 'root' })
export class ExpenseService {
  private http = inject(HttpClient);
  
  // State: Signal writable
  private expensesSignal = signal<Expense[]>([]);
  
  // Selector: Signal computed (Derivado)
  public expenses = this.expensesSignal.asReadonly();
  public totalAmount = computed(() => 
    this.expenses().reduce((acc, curr) => acc + curr.amount, 0)
  );

  loadByGroup(groupId: string) {
    this.http.get<Expense[]>(`/api/groups/${groupId}/expenses`)
      .subscribe(data => this.expensesSignal.set(data));
  }

  processReceipt(file: File): Observable<ReceiptData> {
    const formData = new FormData();
    formData.append('file', file);
    return this.http.post<ReceiptData>('/api/processor/upload', formData);
  }
}
```

---

## 6. Configuración e Infra

### 6.1 Kafka Configuration (Reliability)

```yaml
spring:
  kafka:
    consumer:
      group-id: expenses-processor
      auto-offset-reset: earliest
      enable-auto-commit: false # Importante para 'At-least-once' processing
      properties:
        isolation.level: read_committed # Solo leer transacciones confirmadas
    listener:
      ack-mode: MANUAL_IMMEDIATE
```
