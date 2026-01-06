# Fundamentos

> Principios fundamentales de código limpio, mantenible y escalable que aplican a cualquier lenguaje o framework.

---

## 📊 Niveles de Criticidad

Ver [Tabla de Niveles de Criticidad](../indice.md#niveles-de-criticidad) en el Índice General.

---

## 🌐 Fundamentos de Red

**¿Por qué?** Comprender cómo se comunican los sistemas es fundamental para desarrollar aplicaciones distribuidas, APIs y servicios en la nube.

**¿Quién?** Todo desarrollador, especialmente aquellos trabajando con aplicaciones web, APIs y microservicios.

**¿Cuánto cuesta?** Conocimiento base esencial, inversión de 1-2 días para conceptos fundamentales.

| Concepto | Qué es | Por qué | Cuándo aplicarlo | Dónde | Cómo | Recurso |
| :--------- | :------- | :-------- | :----------------- | :------ | :----- | :-------- |
| **Modelo Cliente-Servidor** | Arquitectura donde clientes solicitan recursos/servicios y servidores los proveen | Base de la mayoría de aplicaciones web y distribuidas | En toda aplicación que requiera comunicación entre componentes | Web apps, APIs, microservicios | Cliente envía request (HTTP, gRPC), servidor procesa y responde | [MDN - Client-Server](https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Client-Server_overview) |
| **Dirección IP** | Identificador único de un dispositivo en una red (IPv4: `192.168.1.1`, IPv6: `2001:0db8::1`) | Permite enrutar paquetes al destino correcto | Configuración de servidores, debugging de red, seguridad | Networking, deployment, firewall rules | IPv4 (32 bits, 4 octetos), IPv6 (128 bits, 8 grupos hex). Privadas (10.x, 172.31.x, 192.168.x) vs Públicas | [Wikipedia - IP Address](https://en.wikipedia.org/wiki/IP_address) |
| **DNS** | Domain Name System - Traduce nombres de dominio (`example.com`) a direcciones IP | Humanos recuerdan nombres, máquinas usan IPs | Al configurar dominios, troubleshooting de conectividad | Todos los servicios web, email, CDN | Cliente consulta DNS resolver → obtiene IP → conecta. Tipos: A (IPv4), AAAA (IPv6), CNAME (alias), MX (email) | [Cloudflare - What is DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) |
| **Puertos** | Número que identifica un servicio específico en un host (0-65535) | Múltiples servicios en misma IP | Al exponer servicios, configurar firewalls | Servidores, contenedores, load balancers | Well-known: HTTP (80), HTTPS (443), SSH (22), PostgreSQL (5432). Especificar en `IP:Puerto` | [Wikipedia - Port](https://en.wikipedia.org/wiki/Port_(computer_networking)) |
| **Protocolo HTTP/HTTPS** | Protocolo de transferencia de hipertexto (seguro con TLS) | Comunicación estándar web | Toda API REST, aplicación web | Frontend-Backend, APIs públicas | Request (método, headers, body) → Response (status, headers, body). HTTPS cifra con TLS | [MDN - HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) |
| **Load Balancer** | Distribuye tráfico entre múltiples servidores | Escalabilidad horizontal, alta disponibilidad | Aplicaciones con múltiples instancias | Arquitecturas escalables, microservicios | Algoritmos: Round Robin, Least Connections, IP Hash. Ver también [Networking en Cloud](./infraestructura-cloud.md#networking) | [NGINX - Load Balancing](https://www.nginx.com/resources/glossary/load-balancing/) |

### Ejemplo: Flujo Cliente-Servidor con DNS

```mermaid
sequenceDiagram
    participant Cliente
    participant DNS
    participant LoadBalancer
    participant Servidor1
    participant Servidor2
    
    Cliente->>DNS: ¿Cuál es la IP de api.example.com?
    DNS-->>Cliente: 203.0.113.10
    Cliente->>LoadBalancer: GET /users (203.0.113.10:443)
    LoadBalancer->>Servidor1: Forward request
    Servidor1-->>LoadBalancer: Response {users: [...]}
    LoadBalancer-->>Cliente: Response {users: [...]}
    
    Note over Cliente,Servidor2: Siguiente request puede ir a Servidor2
```

---

## 🧠 Reglas Generales de Código

**¿Por qué?** Estos principios son la base de código profesional que resiste el paso del tiempo y facilita el trabajo en equipo.

**¿Quién?** Todo desarrollador del equipo, sin importar su rol o nivel de experiencia.

**¿Cuánto cuesta?** Inversión inicial en aprendizaje (1-2 semanas), ahorro exponencial en mantenimiento.

| Nivel | Principio | Qué es | Por qué | Cuándo aplicarlo | Dónde | Cómo | Recurso |
| :------ | :---------- | :-------------- | :-------------- | :------------------------ | :-------------- | :----------- | :-------- |
| 🔴 | **SOLID** | Conjunto de 5 principios para diseño OOP mantenible: SRP, OCP, LSP, ISP, DIP | Reduce acoplamiento, aumenta cohesión, facilita testing y extensibilidad | En toda clase, servicio o módulo con responsabilidades | Servicios, controladores, clases de dominio | SRP: una clase = una razón para cambiar. OCP: abierto a extensión, cerrado a modificación | [Clean Coder](https://blog.cleancoder.com/uncle-bob/2020/10/18/Solid-Relevance.html) |
| 🔴 | **HC/LC** | High Cohesion, Low Coupling - Objetos enfocados y poco dependientes | Cohesión: código relacionado junto. Acoplamiento: dependencias mínimas. Es la meta de SOLID y GRASP. | Al diseñar cualquier módulo o asignar responsabilidades | Arquitectura, paquetes, clases | Alta Cohesión: Clase hace una sola cosa bien. Bajo Acoplamiento: Cambiar A no rompe B. | [GeeksforGeeks](https://www.geeksforgeeks.org/software-engineering-coupling-and-cohesion/) |
| 🟢 | **KISS** | Keep It Simple, Stupid - Favorecer soluciones simples | Código simple es más fácil de entender, mantener y depurar | Siempre, al diseñar cualquier solución | Toda lógica de negocio, algoritmos | Evitar condicionales anidados, abstracciones innecesarias, over-engineering | [FreeCodeCamp](https://www.freecodecamp.org/news/keep-it-simple-stupid-how-to-use-the-kiss-principle-in-design/) |
| 🟠 | **DRY** | Don't Repeat Yourself - Evitar duplicación de lógica | Cambios se hacen en un solo lugar, reduce bugs por inconsistencias | Al detectar código o lógica duplicada (regla de 3) | Funciones, componentes, servicios | Extraer a función reutilizable, usar composición, herencia con cuidado | [TechTarget](https://www.techtarget.com/whatis/definition/DRY-principle) |
| 🟠 | **YAGNI** | You Aren't Gonna Need It - No implementar funcionalidades futuras | Reduce complejidad, ahorra tiempo, evita código muerto | Ante tentación de agregar "por las dudas" | Todo el código | Implementar solo lo que pide el requisito actual, iterar después | [C2 Wiki](https://wiki.c2.com/?YouArentGonnaNeedIt) |
| 🟢 | **Curly's Law** | Una función = una responsabilidad clara | Facilita testing, comprensión y reutilización | Al escribir cualquier función o método | Funciones, métodos | Nombre describe acción única, intentando que ocupe 30 líneas, sin efectos colaterales ocultos | [Coding Horror](https://blog.codinghorror.com/curlys-law-do-one-thing/) |
| 🟢 | **DMMT** | Don't Make Me Think - Código intuitivo y autoexplicativo | Reduce carga cognitiva, acelera onboarding | Al nombrar variables, estructurar módulos | Nombres, estructuras, APIs | Variables descriptivas (`isUserAuthenticated` vs `flag`), flujos lineales | [Wikipedia](https://en.wikipedia.org/wiki/Don%27t_Make_Me_Think) |
| 🟠 | **PORE** | Premature Optimization is the Root of Evil - Optimizar basado en datos | Evita complejidad sin beneficio real | Solo tras profiling que muestre bottleneck | Código con problemas de rendimiento reales | Medir con profiler, optimizar cuello de botella, re-medir | [C2 Wiki](https://wiki.c2.com/?PrematureOptimization) |
| 🟢 | **BSR** | Boy Scout Rule - Dejar el código mejor que lo encontraste | Mejora incremental continua | En cada commit, cada PR | Cualquier archivo que toques | Renombrar variable confusa, extraer función, agregar test | [Snappify](https://snappify.com/blog/boy-scout-rule) |
| 🟢 | **CFM** | Code for the Maintainer - Escribir pensando en quien mantendrá | Reduce frustración, facilita debugging futuro | Al escribir cualquier código no trivial | Todo el código | Documentar decisiones no obvias, evitar "hacks inteligentes", README claro | [C2 Wiki](https://wiki.c2.com/?CodeForTheMaintainer) |
| 🟠 | **POLA** | Principle of Least Astonishment - Comportamiento esperado | Usuario/developer no se sorprende | Al diseñar APIs, interfaces, funciones | APIs públicas, UIs, funciones | `deleteUser()` debe borrar usuario, no enviarlo por email. Consistencia | [Wikipedia](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) |
| 🔴 | **DP** | Defensive Programming - Proteger ante inputs inesperados | Previene crashes, vulnerabilidades | En límites del sistema (APIs, inputs) | Controladores, funciones públicas | Validar inputs, usar defaults seguros, manejar errores explícitamente | [Wikipedia](https://en.wikipedia.org/wiki/Defensive_programming) |
| 🔴 | **FSD** | Fail-safe Defaults - Configuración segura por defecto | Sistema seguro "out of the box" | Al definir configs, permisos | Variables de entorno, roles, permisos | CORS restrictivo, autenticación ON, logs sin datos sensibles | [Medium](https://medium.com/javarevisited/failure-is-required-understanding-fail-safe-and-fail-fast-strategies-ac9112fe056d) |
| 🟠 | **RP** | Robustness Principle (Postel's Law) - Liberal en inputs, estricto en outputs | Sistema tolera variaciones pero es predecible | En integraciones, APIs | APIs, parsers, validadores | Aceptar `"true"`, `1`, `"1"` como true. Emitir siempre `true` (boolean) | [Wikipedia](https://en.wikipedia.org/wiki/Robustness_principle) |
| 🟠 | **Poka-yoke** | Diseño que previene errores humanos | Reduce errores de uso | En UIs, APIs, configs | Formularios, CLIs, scripts | Validaciones automáticas, confirmaciones para destructivos, tipos fuertes | [Wikipedia](https://en.wikipedia.org/wiki/Poka-yoke) |
| 🔴 | **AFP** | Anti-footgun Patterns - Evitar acciones peligrosas sin protección | Previene desastres (DELETE sin WHERE) | En operaciones destructivas | Comandos de admin, migraciones | `DELETE FROM users` → requiere `--confirm`, dry-run por defecto | [FreeCodeCamp](https://www.freecodecamp.org/news/antipatterns-to-avoid-in-code/) |
| 🟢 | **CoC** | Convention over Configuration - Defaults sensatos | Reduce decisiones, acelera desarrollo | En frameworks, tooling | Estructuras de carpetas, nombres | `User` model → `users` table automáticamente (Rails, Django) | [Wikipedia](https://en.wikipedia.org/wiki/Convention_over_configuration) |
| 🟠 | **CoI** | Composition over Inheritance - Favorecer composición | Evita jerarquías rígidas, aumenta flexibilidad | Al reutilizar comportamiento | Clases, servicios | Inyectar dependencias, usar Strategy/Decorator en vez de herencia profunda | [Wikipedia](https://en.wikipedia.org/wiki/Composition_over_inheritance) |
| 🟠 | **LoD** | Law of Demeter - Comunicarse con colaboradores directos | Reduce acoplamiento | En métodos que usan otros objetos | Métodos, servicios | Evitar `user.account.bank.withdraw()`. Exponer `user.withdraw()` | [Wikipedia](https://en.wikipedia.org/wiki/Law_of_Demeter) |
| 🟠 | **TDA** | Tell, Don't Ask - Indicar acciones, no pedir datos | Encapsula lógica en objetos correctos | En lógica de negocio | Modelos, entidades | `order.complete()` vs `if order.status == 'paid': order.status = 'complete'` | [Martin Fowler](https://martinfowler.com/bliki/TellDontAsk.html) |
| 🔴 | **SoC** | Separation of Concerns - Separar responsabilidades | Facilita testing, escalabilidad, mantenimiento | En toda arquitectura | Capas, módulos | UI ≠ Lógica ≠ Persistencia. MVC, Clean Architecture. Ver también [Principios de Arquitectura](./arquitectura-patrones.md#principios-de-arquitectura) | [Wikipedia](https://en.wikipedia.org/wiki/Separation_of_concerns) |
| 🟠 | **GRASP** | General Responsibility Assignment Software Patterns - Asignar responsabilidades efectivamente | Bajo acoplamiento, alta cohesión | Al diseñar clases/módulos | Clases de dominio | Creator, Controller, Information Expert, Low Coupling, High Cohesion | [Wikipedia](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)) |
| 🟢 | **CUPID** | Composable, Unix-style, Predictable, Idiomatic, Domain-based | Código joyful, productivo | En todo desarrollo moderno | Todo el código | Componentes pequeños, pipeable, sin sorpresas, siguiendo idioms del lenguaje | [Dan North](https://dannorth.net/blog/cupid-for-joyful-coding/) |
| 🟠 | **CQS** | Command-Query Separation - Separar comandos de consultas | Claridad sobre efectos secundarios | En métodos públicos | APIs, métodos | `getUser()` retorna sin modificar. `updateUser()` modifica sin retornar | [Martin Fowler](https://martinfowler.com/bliki/CommandQuerySeparation.html) |
| 🟠 | **DIP** | Dependency Inversion Principle - Depender de abstracciones | Facilita testing, intercambio de implementaciones | En dependencias entre capas | Servicios, repositorios | `UserService` depende de `IUserRepository` (interfaz), no de implementación concreta. Ver también [Principios de Arquitectura](./arquitectura-patrones.md#principios-de-arquitectura) | [Wikipedia](https://en.wikipedia.org/wiki/Dependency_inversion_principle) |
| 🟢 | **Hollywood** | "Don't call us, we'll call you" - Inversión de control | Framework orquesta el flujo | En frameworks modernos | Hooks, callbacks | Framework llama a tu código (React useEffect, Angular lifecycle) | [Martin Fowler](https://martinfowler.com/bliki/InversionOfControl.html) |
| 🔴 | **Arbitrary Precision** | Evitar la eleccion de precision numerica arbitraria | Un error de precisión en un cálculo puede equivaler a un "bug de seguridad o caída" a nivel de negocio | En operaciones donde la precision es crítica | Operaciones financieras, matematicas de alto nivel | Usar `BigDecimal` en Java, `decimal` en Python | [Wikipedia](https://en.wikipedia.org/wiki/Arbitrary_precision) |

---

## 🧱 Reglas por Lenguaje

**¿Por qué?** Cada lenguaje tiene idioms y herramientas específicas que mejoran calidad y productividad.

| Lenguaje | Qué es | Por qué | Cuándo | Cómo | Herramientas de Validación |
| :--------- | :-------------- | :-------------- | :-------------- | :----------- | :--------------------------- |
| [Python](https://www.python.org/) | Lenguaje dinámico con tipado opcional | Productividad alta, ecosistema rico | Backend APIs, scripts, ML/Data Science | Usar `typing`, `pydantic` para validación, `dataclasses` para estructuras, `mypy` para chequeo estático | `mypy`, `ruff`, `black`, `pylint` |
| [Java](https://www.java.com/es/) | Lenguaje fuertemente tipado y OOP | Robustez, performance, ecosistema enterprise | Backend enterprise, Android, sistemas distribuidos | Usar `Optional` evitando `null`, `Streams` para colecciones, `Records` (Java 14+), aplicar SOLID | `Checkstyle`, `SpotBugs`, `SonarQube` |
| [TypeScript](https://www.typescriptlang.org/) | Superset de JS con tipado estático | Detecta errores en compilación, mejor IDE support | Frontend, Backend Node.js | Usar `strict mode`, interfaces, tipos utilitarios (`Record<K,V>`, `Partial<T>`), evitar `any` | `tsc --strict`, `ESLint`, `Prettier` |

---

## 🧩 Reglas por Framework

**¿Por qué?** Los frameworks establecen patrones que, si se siguen, maximizan productividad y maintainability.

| Framework | Qué es | Por qué | Cuándo | Cómo | Herramientas |
| :---------- | :-------------- | :-------------- | :-------------- | :----------- | :------------- |
| [Angular](https://angular.dev/) | Framework frontend con enfoque en componentes y signals | Escalabilidad, arquitectura clara, TypeScript nativo | SPAs empresariales, dashboards complejos | Usar `signals` (v17+), `@for` en templates, zoneless rendering, `RxJS` para async, `NgRx` para estado, estructura modular | Angular CLI, Nx |
| [React](https://es.react.dev/) | Librería para UIs declarativas | Flexibilidad, ecosistema gigante, performance | SPAs, mobile (React Native), SSR (Next.js) | Usar `hooks`, `signals` (experimental), `context` para estado, `Suspense` para async, `Server Components` (Next.js), tipado con TS | Vite, Next.js, ESLint React |
| [Django](https://www.djangoproject.com/) | Framework Python full-stack con ORM y admin | Productividad, "batteries included", comunidad madura | Backends complejos, CMS, dashboards admin | Usar `models`, `forms`, `signals`, `class-based views`, `DRF` para APIs REST. Separar lógica en `views`, `serializers` | Django Debug Toolbar, pytest-django |
| [FastAPI](https://fastapi.tiangolo.com/) | Framework Python moderno para APIs con tipado fuerte | Performance (async), documentación auto (OpenAPI), validación Pydantic | APIs REST/GraphQL, microservicios Python | Usar `Depends` para inyección, `Pydantic` para DTOs, `async/await`, `BackgroundTasks` para jobs | Uvicorn, SQLAlchemy 2.0 |
| [Dash](https://dash.plotly.com/) | Framework Python para dashboards interactivos | Rápido para prototipos analíticos, Python puro | Dashboards internos, BI, visualización científica | Separar callbacks por módulo, usar `dcc.Store` para estado, `layout` modular, evitar callbacks encadenados | Plotly, pandas |
| [Spring Boot](https://spring.io/projects/spring-boot) | Framework Java para backend con enfoque microservicios | Configuración mínima, ecosistema maduro, production-ready | Backend enterprise, microservicios | Usar `@Service`, `@Repository`, `@Entity` (JPA), `@Validated`, DTOs, `OpenAPI` para docs | Spring Actuator, JUnit 5 |
| [Spring MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html) | Framework Java MVC para web | Separación clara de responsabilidades | Apps web tradicionales, APIs REST | Usar `@Controller`, `@RestController`, `ModelAndView`, `Thymeleaf` para templates, validación en DTOs | Spring DevTools |

### Spring Boot - Features Avanzados

**Qué es:** Funcionalidades avanzadas de Spring Boot para acelerar desarrollo y mejorar mantenibilidad.

**Cuándo:** Proyectos empresariales que necesitan auditoría, REST rápido, y tareas batch.

| Feature | Qué es | Cuándo | Cómo |
| :-------- | :----- | :----- | :---- |
| **@EntityListeners** | Callbacks en ciclo de vida de entidad | Auditoría, validaciones pre-persist | `@PrePersist`, `@PreUpdate`, `@PreRemove`, `@PostLoad` |
| **AuditorAware** | Auditoría automática global | Trackear quién modificó qué | Implementar `AuditorAware<String>`, retornar username actual |
| **@StoredProcedure** | Llamar stored procedures | Lógica compleja en DB, performance | `@NamedStoredProcedureQuery` + `@StoredProcedureParameter` |
| **Spring Data REST** | REST APIs automáticas desde repositorios | CRUD rápido sin controllers | `@RepositoryRestResource`, `@RestResource` |
| **@Validated** | Validación declarativa | DTOs, entities | `@NotNull`, `@Size`, `@Email`, custom validators |
| **Projections** | DTOs automáticos para queries | Reducir payload, seguridad | `@Projection`, interfaces con getters |
| **Excerpt Projections** | Proyección por defecto en listas | Personalizar respuestas | `excerptProjection = UserSummary.class` |
| **Repository Populator** | Carga de datos iniciales | Seeding, demos | JSON/XML en `/data`, auto-load al arrancar |
| **Spring HATEOAS** | Hypermedia en APIs | RESTful nivel 3, navegación | `EntityModel`, `Link`, `RepresentationModelAssembler` |
| **Spring Batch** | Jobs batch para tareas pesadas | Reportes, ETL, migraciones | `@EnableBatchProcessing`, `Job`, `Step`, `ItemReader/Writer` |
| **Specifications API** | Queries dinámicas type-safe | Búsquedas complejas | `JpaSpecificationExecutor`, `Specification<T>`, `Criteria`, `Predicate` |
| **@RepositoryEventHandler** | Eventos pre/post en repositorios | Validaciones, notificaciones | `@HandleBeforeCreate`, `@HandleAfterSave` |
| **HAL Explorer** | UI para explorar APIs HATEOAS | Testing, documentación interactiva | Incluir `spring-data-rest-hal-explorer`, navegar a `/` |

**Ejemplo @EntityListeners:**

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class User {
    @CreatedDate
    private LocalDateTime createdAt;
    
    @LastModifiedDate
    private LocalDateTime updatedAt;
    
    @CreatedBy
    private String createdBy;
    
    @LastModifiedBy
    private String lastModifiedBy;
}

@Configuration
@EnableJpaAuditing
public class JpaConfig {
    @Bean
    public AuditorAware<String> auditorProvider() {
        return () -> Optional.of(SecurityContextHolder
            .getContext()
            .getAuthentication()
            .getName());
    }
}
```

**Ejemplo Specifications:**

```java
// Repository
public interface UserRepository extends JpaRepository<User, Long>, 
                                         JpaSpecificationExecutor<User> {}

// Specification
public class UserSpecifications {
    public static Specification<User> hasEmail(String email) {
        return (root, query, cb) -> 
            cb.equal(root.get("email"), email);
    }
    
    public static Specification<User> isActive() {
        return (root, query, cb) -> 
            cb.isTrue(root.get("active"));
    }
}

// Uso: búsqueda dinámica
Specification<User> spec = Specification
    .where(hasEmail("user@example.com"))
    .and(isActive());
List<User> users = userRepository.findAll(spec);
```

**Ejemplo Spring Data REST:**

```java
@RepositoryRestResource(path = "users", excerptProjection = UserSummary.class)
public interface UserRepository extends JpaRepository<User, Long> {
    
    @RestResource(path = "by-email", rel = "by-email")
    User findByEmail(@Param("email") String email);
}

// GET /users → lista todos
// GET /users/1 → usuario específico
// POST /users → crear
// GET /users/search/by-email?email=x → custom query
```

---

## 🎯 Checklist de Código Limpio

Antes de cada commit, verificar:

- [ ] Nombres de variables/funciones son claros y descriptivos
- [ ] No hay código comentado (usar control de versiones)
- [ ] No hay `console.log()`, `print()` de debugging
- [ ] Funciones tienen máximo 30 líneas
- [ ] Sin magic numbers (usar constantes con nombres)
- [ ] Tests unitarios para lógica no trivial
- [ ] Documentación para funciones públicas
- [ ] Principio de Bajo Acoplamiento y Alta Cohesión respetado
- [ ] Sin warnings del linter/compiler
- [ ] Código formateado con herramienta del equipo
