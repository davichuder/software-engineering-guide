# Documentación y Diagramas

> Lenguajes, herramientas y tipos de diagramas para documentar sistemas de software efectivamente.

---

## 📝 Documentación de Software

**Qué:** Información escrita o visual que explica cómo funciona, se usa o se mantiene un sistema.

**Por qué:** Código se lee 10x más de lo que se escribe. Buena doc = onboarding rápido, menos bugs, mejor mantenimiento.

**Quién:** Developers, technical writers, architects.

**Cuándo:** Continuo, como parte del desarrollo (no después).

**Esfuerzo:** 15% del tiempo de desarrollo, paga dividendos en mantenimiento.

---

## 📖 Lenguajes de Documentación

| Lenguaje | Qué | Cuándo | Ejemplo |
| :--------- | :----- | :----- | :-------- |
| [Markdown](https://www.markdownguide.org/) | Formato texto plano ligero | READMEs, wikis, docs generales | `# Título\n**negrita**` |
| [AsciiDoc](https://asciidoc.org/) | Markdown con esteroides | Docs técnicas complejas, libros | Soporte tablas, includes |
| [reStructuredText](https://docutils.sourceforge.io/rst.html) | Formato Python docs | Sphinx, docs Python | `.. code-block:: python` |
| [LaTeX](https://www.latex-project.org/) | Typesetting matemático/científico | Papers, tesis, documentación científica | Ecuaciones, referencias |
| [MDX](https://mdxjs.com/) | Markdown + JSX components | Docs interactivas | Markdown con React components |

---

## 🎨 Herramientas de Diagramas

### Diagramas como Código

| Herramienta | Qué | Sintaxis | Caso de Uso |
| :------------ | :----- | :--------- | :--------- |
| [Mermaid](https://mermaid.js.org/) | Diagramas en Markdown | Texto declarativo | Flowcharts, secuencia, Gantt, ER |
| [PlantUML](https://plantuml.com/) | UML y más | Texto declarativo | Clases, secuencia, componentes |
| [D2](https://d2lang.com/) | Diagramas declarativos modernos | Texto declarativo | Arquitectura, diagramas técnicos |
| [Graphviz](https://graphviz.org/) | Layout automático de grafos | DOT language | Dependencias, grafos |

**Ventajas diagrams-as-code:**

- ✅ Versionado en Git
- ✅ Diffs legibles
- ✅ CI/CD automation
- ✅ No desincroniza con código

### Herramientas Visuales

| Herramienta | Qué | Cuándo |
| :------------ | :----- | :----- |
| [draw.io](https://www.draw.io/) | Diagramas drag-and-drop free | Prototipado rápido |
| [Lucidchart](https://www.lucidchart.com/) | Diagramas profesionales | Colaboración, org charts |
| [Excalidraw](https://excalidraw.com/) | Sketch-style diagramas | Brainstorming, wireframes |
| [Miro](https://miro.com/) | Whiteboard colaborativo | Workshops, planning |
| [FigJam](https://www.figma.com/figjam/) | Whiteboard Figma | Diseño + diagramas |

---

## 📊 Tipos de Diagramas

### Diagrama de Flujo (Flowchart)

**Qué:** Representa proceso con pasos y decisiones.

**Cuándo:** Algoritmos, procesos de negocio, workflows.

**Mermaid:**

```mermaid
graph TD
    A[Start] --> B{Usuario autenticado?}
    B -->|Sí| C[Mostrar dashboard]
    B -->|No| D[Redirigir a login]
    C --> E[End]
    D --> E
```

**Símbolos:**

- Óvalo: Inicio/Fin
- Rectángulo: Proceso
- Rombo: Decisión
- Paralelogramo: Input/Output

---

### C4 Model (Context, Containers, Components, Code)

**Qué:** Niveles de abstracción para arquitectura de software.

**Por qué:** Comunicar arquitectura a diferentes audiencias.

**Niveles:**

| Nivel | Audiencia | Detalle | Ejemplo |
| :------ | :---------- | :-------- | :-------- |
| **C1: Context** | Todos (non-tech inclusive) | Sistema y usuarios/sistemas externos | "E-commerce interactúa con Payment Gateway" |
| **C2: Containers** | Tech leads, architects | Apps, DBs, servicios | "Web App (React) → API (FastAPI) → PostgreSQL" |
| **C3: Components** | Developers | Módulos dentro de container | "UserController, AuthService, UserRepository" |
| **C4: Code** | Developers | Clases, métodos | UML class diagram |

**Ejemplo C2 (Mermaid):**

```mermaid
C4Container
    Person(user, "Usuario", "Cliente de e-commerce")
    Container_Boundary(system, "Sistema E-commerce") {
        Container(web, "Web App", "React", "SPA")
        Container(api, "API", "FastAPI", "REST API")
        Container(db, "Database", "PostgreSQL", "Datos")
    }
    System_Ext(payment, "Payment Gateway", "Stripe")
    
    Rel(user, web, "Usa", "HTTPS")
    Rel(web, api, "Llama", "JSON/HTTPS")
    Rel(api, db, "Lee/Escribe", "SQL")
    Rel(api, payment, "Procesa pagos", "HTTPS")
```

**Herramientas:** [Structurizr](https://structurizr.com/), [C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML)

---

### Diagrama Entidad-Relación (ER)

**Qué:** Modela entidades, atributos y relaciones de base de datos.

**Cuándo:** Diseñar schema de BD, documentar modelo de datos.

**Mermaid:**

```mermaid
erDiagram
    USER || -- o{ ORDER : places
    USER {
        int id PK
        string email
        string name
    }
    ORDER || -- |{ ORDER_ITEM : contains
    ORDER {
        int id PK
        int user_id FK
        datetime created_at
        string status
    }
    ORDER_ITEM {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
    }
    PRODUCT || -- o{ ORDER_ITEM : "ordered in"
    PRODUCT {
        int id PK
        string name
        decimal price
    }
```

**Cardinalidad:**

- `|| -- ||`: One-to-one
- `|| -- o{`: One-to-many
- `}o--o{`: Many-to-many

---

### Diagrama de Clases (UML)

**Qué:** Muestra clases, atributos, métodos y relaciones OOP.

**Cuándo:** Diseñar arquitectura OOP, refactoring.

**PlantUML:**

```puml
@startuml
class User {
  - id: UUID
  - email: String
  - password: String
  + authenticate(): boolean
  + resetPassword(): void
}

class Order {
  - id: UUID
  - userId: UUID
  - status: OrderStatus
  + cancel(): void
  + complete(): void
}

class OrderItem {
  - quantity: int
  - price: Decimal
}

User "1" --> "*" Order : places
Order "1" *-- "*" OrderItem : contains
@enduml
```

**Relaciones:**

- `-->`: Asociación
- `*--`: Composición
- `o--`: Agregación
- `<| --`: Herencia
- `<..`: Dependencia

---

### Diagrama de Secuencia

**Qué:** Muestra interacciones entre objetos en el tiempo.

**Cuándo:** Documentar flujos complejos, API calls, async operations.

**Mermaid:**

```mermaid
sequenceDiagram
    participant U as Usuario
    participant W as Web App
    participant A as API
    participant D as Database
    participant P as Payment Gateway
    
    U->>W: Click "Comprar"
    W->>A: POST /orders
    A->>D: INSERT order
    D-->>A: order_id
    A->>P: POST /charge
    P-->>A: success
    A->>D: UPDATE order status
    A-->>W: 201 Created
    W-->>U: Confirmación
```

**Símbolos:**

- `->`: Llamada síncrona
- `-->`: Respuesta
- `->>`: Llamada asíncrona
- `-->>`: Respuesta asíncrona

---

### Diagrama de Componentes

**Qué:** Muestra organización y dependencias entre componentes del sistema.

**Cuándo:** Arquitectura modular, microservicios.

**PlantUML:**

```puml
@startuml
package "Frontend" {
  [React App]
  [Redux Store]
}

package "Backend" {
  [API Gateway]
  [Auth Service]
  [User Service]
  [Order Service]
}

package "Data" {
  database "PostgreSQL"
  database "Redis"
}

[React App] --> [API Gateway]
[API Gateway] --> [Auth Service]
[API Gateway] --> [User Service]
[API Gateway] --> [Order Service]
[Auth Service] --> [Redis]
[User Service] --> [PostgreSQL]
[Order Service] --> [PostgreSQL]
@enduml
```

---

### Diagrama de Estados (State Machine)

**Qué:** Muestra estados posibles de un objeto y transiciones.

**Cuándo:** Workflows, FSM, ciclos de vida.

**Mermaid:**

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Review: submit()
    Review --> Approved: approve()
    Review --> Rejected: reject()
    Approved --> Published: publish()
    Rejected --> Draft: revise()
    Published --> Archived: archive()
    Archived --> [*]
```

---

### Diagrama de Arquitectura (System Design)

**Qué:** Vista de alto nivel de arquitectura del sistema.

**Cuándo:** Onboarding, design reviews, documentación.

**Ejemplo (texto Mermaid):**

```mermaid
graph TB
    subgraph "Client Layer"
        Web[Web App<br/>React]
        Mobile[Mobile App<br/>React Native]
    end
    
    subgraph "API Layer"
        Gateway[API Gateway<br/>Kong]
        Auth[Auth Service]
        Users[Users Service]
        Orders[Orders Service]
    end
    
    subgraph "Data Layer"
        PG[(PostgreSQL)]
        Redis[(Redis)]
        S3[(S3)]
    end
    
    Web --> Gateway
    Mobile --> Gateway
    Gateway --> Auth
    Gateway --> Users
    Gateway --> Orders
    Auth --> Redis
    Users --> PG
    Orders --> PG
    Orders --> S3
```

---

### Diagrama de Despliegue

**Qué:** Muestra infraestructura física/lógica donde corre el sistema.

**Cuándo:** DevOps, infraestructura, deployment planning.

**PlantUML:**

```puml
@startuml
node "AWS" {
  node "EKS Cluster" {
    component "Frontend Pods" as fe
    component "Backend Pods" as be
  }
  database "RDS PostgreSQL" as db
  storage "S3 Bucket" as s3
}

node "Cloudflare" {
  component "CDN" as cdn
}

cdn --> fe
fe --> be
be --> db
be --> s3
@enduml
```

---

## 📚 Tipos de Documentación

| Tipo | Audiencia | Contenido | Ejemplo |
| :----- | :---------- | :---------- | :-------- |
| **README** | Developers | Qué es, setup, uso básico | `README.md` en root |
| **API Docs** | API consumers | Endpoints, schemas, auth | Swagger UI, Redoc |
| **Architecture Decision Records (ADR)** | Team | Por qué tomamos decisión X | `docs/adr/0use-postgres.md` |
| **Runbooks** | DevOps, on-call | Cómo responder a incidents | "Qué hacer si DB está lenta" |
| **User Guides** | End users | Cómo usar el producto | Tutorials, FAQs |
| **Code Comments** | Developers | Por qué existe código complejo | `// Workaround for bug X` |

---

## ✍️ Best Practices Documentación

| Práctica | Por qué | Cómo |
| :--------- | :---- | :----- |
| **Docs como código** | Versionado, CI/CD | Markdown en repo, auto-deploy |
| **Single Source of Truth** | Evitar duplicación | Una ubicación canónica |
| **Living Documentation** | Actualizado con código | PR incluye doc changes |
| **Progresive Disclosure** | No overwhelming | Overview → detalles on-demand |
| **Examples First** | Más fácil entender | Code snippets antes de teoría |
| **Search-Friendly** | Encontrar rápido | Headings claros, keywords |

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Docs desactualizadas** | Peor que no tener docs | CI checks, code+docs juntos |
| **Over-documentation** | Nadie lee 100 páginas | Conciso, TL;DR sections |
| **Documentar obvio** | Ruido | Documentar el "por qué", no el "qué" |
| **Diagramas en binarios** | No versionables | Diagrams-as-code |
| **Solo comentarios en código** | No discoverable | README, wiki para overview |

---

## 📚 Recursos

- [Mermaid Live Editor](https://mermaid.live/)
- [PlantUML Documentation](https://plantuml.com/)
- [C4 Model](https://c4model.com/)
- [Write the Docs](https://www.writethedocs.org/)
- [Diátaxis Framework](https://diataxis.fr/) - Docs structure
