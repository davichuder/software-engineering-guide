# Disciplinas de Desarrollo

> Metodologías y enfoques sistemáticos para construir software de calidad con feedback temprano y orientación al dominio.

---

## 🎯 ¿Qué son las Disciplinas de Desarrollo?

Enfoques metodológicos que establecen **cómo** escribir código, diseñar sistemas y validar funcionalidad. Van más allá de "escribir tests" hacia culturas de calidad integrada.

**¿Por qué?** Reducen bugs, facilitan refactoring, alinean código con negocio.  
**¿Quién?** Desarrolladores, testers, arquitectos, product owners (especialmente en BDD/ATDD).  
**¿Cuánto cuesta?** Curva de aprendizaje de 2-4 semanas, ROI positivo desde el primer refactor grande.

---

## 🧪 Disciplinas Principales

| Disciplina | Qué es | Por qué usarlo | Cuándo aplicarlo | Dónde | Cómo implementarlo | Esfuerzo | Herramientas |
|:-----------|:--------------|:---------------------|:------------------------|:--------------|:-------------------------|:-------------------|:-------------|
| **TDD** (Test-Driven Development) | Escribir tests **antes** que el código de producción | Diseño emergente, cobertura 100%, refactoring seguro | En lógica de negocio compleja, funciones puras, algoritmos | Backend, utilidades, servicios | Ciclo Red→Green→Refactor: (1) Test falla, (2) Código mínimo que pase, (3) Refactorizar sin cambiar comportamiento | Alto inicial, reduce bugs 80% | [pytest](https://docs.pytest.org/), [JUnit 5](https://junit.org/junit5/), [Jest](https://jestjs.io/), [Vitest](https://vitest.dev/) |
| **BDD** (Behavior-Driven Development) | Especificaciones ejecutables en lenguaje natural (Given-When-Then) | Colaboración negocio-tech, tests legibles para no-devs | Cuando stakeholders deben validar comportamiento | Features críticas, flujos de usuario | Escenarios Gherkin: `Given` (contexto), `When` (acción), `Then` (resultado esperado). Automatizar con step definitions | Medio, requiere disciplina en escritura | [Cucumber](https://cucumber.io/), [Behave](https://behave.readthedocs.io/), [SpecFlow](https://specflow.org/) |
| **ATDD** (Acceptance Test-Driven Development) | Definir criterios de aceptación con stakeholders **antes** de implementar | Alineación negocio-tech, reduce retrabajos | Al inicio de cada feature/story | Features completas (UI + Backend) | Workshop con PO/QA: definir escenarios, escribir tests de aceptación (E2E), implementar hasta que pasen | Medio-Alto, alta claridad de requisitos | [FitNesse](http://fitnesse.org/), [Robot Framework](https://robotframework.org/), [Playwright](https://playwright.dev/) |
| **DDD** (Domain-Driven Design) | Modelar software según el **dominio de negocio**, con lenguaje ubicuo | Código refleja negocio, facilita comunicación con expertos | Dominios complejos (finanzas, salud, logística), sistemas de larga vida | Arquitectura, diseño de servicios | Bounded Contexts, Aggregates, Entities, Value Objects, Domain Events, Event Storming con expertos | Alto, transforma arquitectura | [EventStorming](https://www.eventstorming.com/), [Context Mapper](https://contextmapper.org/) |
| **FDD** (Feature-Driven Development) | Desarrollo iterativo basado en **features** de negocio visibles | Entregas incrementales, seguimiento claro | Proyectos grandes con múltiples equipos | Gestión de proyectos, planificación | (1) Modelar dominio, (2) Construir lista de features, (3) Planificar/diseñar/construir por feature en ciclos de 2 semanas | Medio, estructura procesos | [Jira](https://www.atlassian.com/software/jira), [Azure DevOps](https://azure.microsoft.com/en-us/products/devops) |
| **MDD** (Model-Driven Development) | Generar código ejecutable desde **modelos abstractos** (UML, DSL) | Reduce código boilerplate, documentación visual ejecutable | Dominios con patrones repetitivos (CRUD, ETL) | Generación de código, arquitectura | Crear modelos UML/DSL, transformaciones automáticas con plantillas, validar código generado | Alto en setup, escala bien | [Eclipse Modeling](https://www.eclipse.org/modeling/), [PlantUML](https://plantuml.com/), [Xtext](https://www.eclipse.org/Xtext/) |
| **PBT** (Property-Based Testing) | Validar **propiedades invariantes** con casos aleatorios generados | Descubre edge cases que testing manual no cubre | Algoritmos, parsers, funciones matemáticas, validadores | Lógica compleja, transformaciones de datos | Definir propiedades (`reverse(reverse(x)) == x`), generar 100s de inputs aleatorios, verificar invariantes | Medio, alta cobertura efectiva | [Hypothesis](https://hypothesis.readthedocs.io/) (Python), [fast-check](https://fast-check.dev/) (JS), [QuickCheck](https://hackage.haskell.org/package/QuickCheck) (Haskell) |

---

## 🔄 Ciclo TDD Detallado

```text
1. RED (Test Falla)
   └─> Escribir test mínimo que falle
       Ejemplo: test_user_can_login()
   
2. GREEN (Test Pasa)
   └─> Código mínimo que pase el test
       Puede ser "feo" o hardcoded
   
3. REFACTOR (Mejorar)
   └─> Limpiar sin cambiar comportamiento
       Extraer funciones, eliminar duplicación
   
4. REPEAT
   └─> Siguiente test
```

**Reglas de oro TDD:**

- No escribir código de producción sin test que lo requiera
- No escribir más test del necesario para fallar
- No escribir más código del necesario para pasar el test

---

## 📝 Ejemplo BDD (Gherkin)

```gherkin
Feature: Autenticación de usuario
  Como usuario registrado
  Quiero iniciar sesión con email y contraseña
  Para acceder a mi cuenta

  Scenario: Login exitoso con credenciales válidas
    Given un usuario registrado con email "user@example.com"
    And contraseña "SecurePass123"
    When el usuario intenta iniciar sesión
    Then el sistema debe autenticar al usuario
    And redirigir al dashboard

  Scenario: Login fallido con contraseña incorrecta
    Given un usuario registrado con email "user@example.com"
    When el usuario intenta iniciar sesión con contraseña "WrongPass"
    Then el sistema debe rechazar la autenticación
    And mostrar mensaje "Credenciales inválidas"
```

---

## 🏗️ DDD: Conceptos Clave

| Concepto | Definición | Ejemplo |
|:---------|:-----------|:--------|
| **Ubiquitous Language** | Vocabulario compartido entre negocio y tech | "Orden" en vez de "Pedido" si negocio dice "Orden" |
| **Bounded Context** | Límite explícito donde un modelo aplica | Contexto "Ventas" vs "Inventario": "Producto" significa cosas distintas |
| **Entity** | Objeto con identidad única que persiste | Usuario (ID único, cambia atributos pero sigue siendo el mismo) |
| **Value Object** | Objeto inmutable definido por sus atributos | Dirección, Money (no tiene ID, se reemplaza completo) |
| **Aggregate** | Cluster de entidades/VOs con raíz que garantiza invariantes | Orden (raíz) contiene LineItems, total siempre es suma de items |
| **Domain Event** | Algo que ocurrió en el dominio | `OrderPlaced`, `PaymentReceived`, `ShipmentDispatched` |
| **Repository** | Abstracción para persistir/recuperar aggregates | `OrderRepository.findById()`, no expone SQL |

---

## 🧪 Property-Based Testing: Ejemplo

```python
from hypothesis import given, strategies as st

# Propiedad: ordenar dos veces debe ser idempotente
@given(st.lists(st.integers()))
def test_sort_idempotent(lista):
    sorted_once = sorted(lista)
    sorted_twice = sorted(sorted_once)
    assert sorted_once == sorted_twice

# Propiedad: revertir dos veces retorna al original
@given(st.text())
def test_reverse_involution(texto):
    assert texto == texto[::-1][::-1]
```

**Ventaja:** Hypothesis genera cientos de casos (listas vacías, negativas, duplicados, etc.) automáticamente.

---

## 🎯 ¿Cuándo usar cada disciplina?

| Contexto | Disciplina Recomendada | Razón |
|:---------|:----------------------|:------|
| Startup/MVP | TDD + BDD ligero | Velocidad, calidad mínima |
| Sistema legacy | ATDD para features nuevas, tests de regresión | Proteger contra regresiones |
| Dominio complejo (fintech, health) | DDD + Event Storming | Alinear modelo con negocio |
| Múltiples equipos | FDD | Coordinación, features independientes |
| Algoritmos críticos | TDD + PBT | Máxima cobertura de edge cases |
| APIs/Integraciones | Contract Testing + BDD | Validar contratos entre servicios |

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
|:------------|:---------|:---------|
| Tests después de código | Bajo coverage, tests sesgados | Adoptar TDD, al menos en lógica crítica |
| Gherkin técnico | Usa implementación en `Given/When/Then` | Lenguaje de negocio, sin mencionar clases/DBs |
| Tests lentos | Suite tarda >5 min, nadie la corre | Paralelizar, usar mocks, separar integración de unitarios |
| Aggregates gigantes | `Order` tiene 50 campos | Separar en Aggregates más pequeños, contexts |
| Sobre-modelado DDD | Todo es Entity/VO/Aggregate | Usar pragmáticamente, no dogmáticamente |

---

## 📚 Recursos

- [Test Driven Development - Kent Beck](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [Domain-Driven Design - Eric Evans](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
- [The Cucumber Book](https://pragprog.com/titles/hwcuc2/the-cucumber-book-second-edition/)
- [Property-Based Testing with Hypothesis](https://hypothesis.works/)
