# Prompts y Agentes de IA

> Definiciones de agentes de IA especializados para asistir en desarrollo, arquitectura, testing y mejores prácticas.

---

## 🤖 Introducción

**Qué:** Agentes de IA configurados con expertise específico para asistir en diferentes aspectos del desarrollo de software.

**Por qué:** Un agente bien configurado actúa como un senior developer/architect disponible 24/7, proporcionando feedback de calidad, detectando problemas y sugiriendo mejores prácticas.

**Cómo:** Cada agente tiene un prompt base que define su personalidad, expertise, comportamiento y áreas de especialización.

### Filosofía: AI como Jarvis, Developer como Tony Stark

- **AI no reemplaza**: AI es una herramienta que amplifica capacidades
- **AI ejecuta, Developer dirige**: El developer toma decisiones arquitectónicas, AI implementa
- **Colaboración, no subordinación**: AI debe cuestionar, proponer alternativas, no solo decir "sí"
- **Conceptos > Código**: AI ayuda a implementar, pero el developer debe entender los fundamentos

---

## 👔 The Gentleman - Agente Principal

> [!TIP]
> **Definición completa disponible**: [the-gentleman.md](./the-gentleman.md)
>
> El archivo incluye toda la configuración en formato Claude Code para usar directamente en `.claude/agentes/`

**Rol:** Senior Architect & Code Reviewer con 15+ años de experiencia

**Experiencia:** PSF Fellow, FastAPI/Django Core Contributors, Java Champion, Angular GDE, React Core Contributor, Kaggle Grandmaster, MySQL/MongoDB/PostgreSQL Community Contributors & Champions, Microsoft MVP (Data Platform), PMI-PMP/PMI-ACP, Scrum Alliance CSPO/CSM, ISTQB Certified, SREcon Speaker

**Personalidad:** Apasionado por la educación pero harto de la mediocridad, atajos y contenido superficial. No busca caer bien, busca que aprendas de verdad.

**Características clave:**

- NUNCA es un yes-man - siempre verifica antes de acordar
- Partner colaborativo, no subordinado
- Lenguaje directo: Español Rioplatense + English sin filtros
- Filosofía: Conceptos > Código, Fundamentos Sólidos, AI como herramienta

Para ver el comportamiento completo, reglas de lenguaje, áreas de expertise detalladas y ejemplos de interacción, consultar [the-gentleman.md](./the-gentleman.md).

---

## 🔮 Agentes Especializados

> [!NOTE]
> Los agentes especializados están definidos en archivos individuales siguiendo el **formato de Claude Code** para subagentes.
> Cada agente tiene su propia definición completa con YAML frontmatter, expertise, comportamiento y prompts de ejemplo.
> Haz clic en los enlaces para ver la definición completa de cada agente.

### 📖 Sobre el Formato Claude Code

Los agentes están definidos usando el formato estándar de **Claude Code Subagents**:

```markdown
---
name: agent-name
description: Descripción de cuándo invocar este agente
tools: Read, Write, Edit, Grep, Glob, Bash
model: sonnet
---

# Agent Name

Prompt del sistema y comportamiento del agente...
```

**Beneficios del formato Claude Code:**

- ✅ **Reutilizables**: Pueden usarse en diferentes proyectos
- ✅ **Versionables**: Se mantienen en control de versiones
- ✅ **Compartibles**: Fácil de compartir con el equipo
- ✅ **Estándar**: Sigue convenciones de la industria
- ✅ **Mantenibles**: Definiciones separadas facilitan actualizaciones

**Cómo usar estos agentes:**

1. Copiar el archivo del agente a `.claude/agentes/` (proyecto) o `~/.claude/agentes/` (usuario)
2. Claude Code los detectará automáticamente
3. Invocar explícitamente: "Usa el agente java-expert para revisar este código"
4. O dejar que Claude los invoque automáticamente según el contexto

---

### 🔧 Backend

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Java Expert** | Java 17+, Maven, Gradle, JUnit 5, Mockito | Diseño OOP, debugging, performance, best practices Java moderno | [java-expert.md](./backend/java-expert.md) |
| **Spring Expert** | Spring Boot 3.x, Spring Security, Spring Data, Actuator | Microservicios, seguridad OAuth2/JWT, reactive programming | [spring-expert.md](./backend/spring-expert.md) |
| **Python Expert** | Python 3.11+, pytest, black, mypy, ruff | Code review, patterns Pythonic, type hints, async/await | [python-expert.md](./backend/python-expert.md) |
| **FastAPI Expert** | FastAPI, Uvicorn, Pydantic v2, SQLAlchemy 2.0 | APIs rápidas, validación, async endpoints, dependency injection | [fastapi-expert.md](./backend/fastapi-expert.md) |
| **Django Expert** | Django 4.x+, DRF, Celery, Redis, pytest-django | Arquitectura monolítica, ORM avanzado, background tasks | [django-expert.md](./backend/django-expert.md) |
| **Node.js Expert** | Node.js 20+, npm/pnpm, ESM, streams, worker threads | Event loop, async patterns, streams, performance | [nodejs-expert.md](./backend/nodejs-expert.md) |
| **Express Expert** | Express 4.x, middleware, helmet, PM2, winston | APIs REST, middlewares, error handling, security | [express-expert.md](./backend/express-expert.md) |
| **NestJS Expert** | NestJS, TypeORM, Passport, Swagger, Jest | Arquitectura modular, DI, decorators, guards, interceptors | [nestjs-expert.md](./backend/nestjs-expert.md) |

---

### 🎨 Frontend

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **TypeScript Expert** | TypeScript 5.x, ESLint, type-fest, zod | Tipado avanzado (generics, conditional types), migraciones JS→TS | [typescript-expert.md](./frontend/typescript-expert.md) |
| **React Expert** | React 18+, Hooks, Redux Toolkit, React Query, Vite | Component design, state management, performance optimization | [react-expert.md](./frontend/react-expert.md) |
| **Next.js Expert** | Next.js 14+, App Router, Server Components, Middleware | SSR, SSG, ISR, routing, performance, SEO | [nextjs-expert.md](./frontend/nextjs-expert.md) |
| **Angular Expert** | Angular 17+, Signals, RxJS, NgRx, Standalone Components | Arquitectura de apps, reactividad, state management, forms | [angular-expert.md](./frontend/angular-expert.md) |
| **Vue Expert** | Vue 3, Composition API, Pinia, Vite, Vitest | Reactivity system, composables, state management | [vue-expert.md](./frontend/vue-expert.md) |
| **CSS/Styling Expert** | CSS3, Sass, Tailwind CSS, CSS Modules, styled-components | Layouts (Flexbox, Grid), responsive design, animations | [css-styling-expert.md](./frontend/css-styling-expert.md) |

---

### 🗄️ Bases de Datos

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **PostgreSQL Expert** | PostgreSQL 15+, pgAdmin, EXPLAIN ANALYZE, partitioning | Modelado, índices, query tuning, partitioning | [postgresql-expert.md](./databases/postgresql-expert.md) |
| **MSSQL Expert** | SQL Server 2022, T-SQL, SSMS, execution plans | Stored procedures, triggers, índices, performance tuning | [mssql-expert.md](./databases/mssql-expert.md) |
| **MongoDB Expert** | MongoDB 6.x+, Compass, aggregation framework, sharding | Modelado NoSQL, aggregation pipelines, sharding | [mongodb-expert.md](./databases/mongodb-expert.md) |
| **Redis Expert** | Redis 7.x, data structures, pub/sub, Lua scripting | Caching strategies, session storage, rate limiting | [redis-expert.md](./databases/redis-expert.md) |
| **SQLite Expert** | SQLite 3, migrations, WAL mode, FTS5 | Testing/local, migraciones, limitaciones de concurrencia | [sqlite-expert.md](./databases/sqlite-expert.md) |

---

### 🏗️ Arquitectura y DevOps

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Software Architecture Expert** | C4 Model, UML, ADR, Mermaid, PlantUML | Decision records, trade-offs, patterns arquitectónicos | [software-architecture-expert.md](./architecture-devops/software-architecture-expert.md) |
| **Microservices Expert** | Docker, Kubernetes, gRPC, Kafka, Istio | Bounded contexts, comunicación, resiliencia, service mesh | [microservices-expert.md](./architecture-devops/microservices-expert.md) |
| **Event-Driven Expert** | Kafka, RabbitMQ, EventBridge, Debezium | Diseño de eventos, idempotencia, ordering, CQRS | [event-driven-expert.md](./architecture-devops/event-driven-expert.md) |
| **API Design Expert** | OpenAPI, AsyncAPI, REST, GraphQL, gRPC | Diseño de APIs, versionado, documentación, contratos | [api-design-expert.md](./architecture-devops/api-design-expert.md) |
| **CI/CD Expert** | GitHub Actions, GitLab CI, Jenkins, ArgoCD | Pipelines, gates, deployment strategies, rollback | [cicd-expert.md](./architecture-devops/cicd-expert.md) |
| **IaC Expert** | Terraform, Pulumi, CloudFormation, Ansible | Infraestructura reproducible, modularización, drift detection | [iac-expert.md](./architecture-devops/iac-expert.md) |
| **Observability Expert** | Prometheus, Grafana, ELK, Jaeger, OpenTelemetry | Instrumentación (metrics, logs, traces), alerting, SLOs | [observability-expert.md](./architecture-devops/observability-expert.md) |
| **Security Expert** | OWASP Top 10, SAST, DAST, Snyk, threat modeling | Security reviews, SAST/DAST, threat modeling (STRIDE) | [security-expert.md](./architecture-devops/security-expert.md) |
| **Kubernetes Expert** | Kubernetes, Helm, Kustomize, kubectl, k9s | Deployments, Services, Ingress, RBAC, autoscaling | [kubernetes-expert.md](./architecture-devops/kubernetes-expert.md) |

---

### 🧪 Testing

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **TDD Expert** | TDD workflow, JUnit, pytest, Jest, Vitest | Test-first development, red-green-refactor, anti-patterns | [tdd-expert.md](./testing/tdd-expert.md) |
| **BDD Expert** | Cucumber, Gherkin, SpecFlow, Behave | Criterios de aceptación, escenarios Given-When-Then | [bdd-expert.md](./testing/bdd-expert.md) |
| **Performance Testing Expert** | JMeter, k6, Locust, Gatling | Planes de carga (load, stress, spike), benchmarks | [performance-testing-expert.md](./testing/performance-testing-expert.md) |
| **E2E Testing Expert** | Playwright, Cypress, Selenium, Puppeteer | Automatización UI tests, page object model, flaky tests | [e2e-testing-expert.md](./testing/e2e-testing-expert.md) |
| **QA Automation Expert** | Selenium, Cypress, Playwright, test frameworks | Estrategia de automatización, test pyramid, mantenimiento | [qa-automation-expert.md](./testing/qa-automation-expert.md) |

---

### 📊 Datos, ML y Finanzas

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Data Science Expert** | pandas, NumPy, scikit-learn, Jupyter, MLflow | EDA, feature engineering, pipelines reproducibles | [data-science-expert.md](./data-ml-finance/data-science-expert.md) |
| **ML/DL Expert** | TensorFlow, PyTorch, Keras, Hugging Face | Model design, training, hyperparameter tuning, deployment | [ml-dl-expert.md](./data-ml-finance/ml-dl-expert.md) |
| **RL Expert** | OpenAI Gym, Stable Baselines3, Ray RLlib | Diseño de entornos, reward shaping, algoritmos (DQN, PPO) | [rl-expert.md](./data-ml-finance/rl-expert.md) |
| **NLP Expert** | Hugging Face, spaCy, NLTK, LangChain | Text processing, embeddings, fine-tuning LLMs, RAG | [nlp-expert.md](./data-ml-finance/nlp-expert.md) |
| **Computer Vision Expert** | OpenCV, TensorFlow/PyTorch, YOLO, Detectron2 | Object detection, segmentation, image classification | [computer-vision-expert.md](./data-ml-finance/computer-vision-expert.md) |
| **Quant Finance Expert** | NumPy, pandas, QuantLib, zipline, TA-Lib | Modelos financieros, pricing, risk metrics, backtesting | [quant-finance-expert.md](./data-ml-finance/quant-finance-expert.md) |
| **Trading Systems Expert** | FIX protocol, low-latency patterns, order management | Arquitectura de trading, simulación, order execution | [trading-systems-expert.md](./data-ml-finance/trading-systems-expert.md) |
| **Data Visualization Expert** | Matplotlib, Seaborn, Plotly, D3.js, Tableau | Diseño de gráficos, dashboards, storytelling visual | [data-visualization-expert.md](./data-ml-finance/data-visualization-expert.md) |

---

### 📝 Documentación y Contenido

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Technical Writer Expert** | Markdown, MkDocs, Docusaurus, Sphinx, Vale | Redacción de manuales, API docs, tutorials, style guides | [technical-writer-expert.md](./documentation/technical-writer-expert.md) |
| **API Documentation Expert** | OpenAPI/Swagger, AsyncAPI, Redoc, Stoplight | Documentación de APIs, ejemplos, authentication | [api-documentation-expert.md](./documentation/api-documentation-expert.md) |
| **Content Creator Expert** | LinkedIn/Twitter formats, copywriting, Canva | Adaptar contenido técnico a posts, threads, visuales | [content-creator-expert.md](./documentation/content-creator-expert.md) |
| **Diagram Expert** | Mermaid, PlantUML, draw.io, Excalidraw, C4 | Diagramas de arquitectura, flujo, secuencia, clases | [diagram-expert.md](./documentation/diagram-expert.md) |

---

### 👥 Roles de Negocio

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **PM/PO Advisor** | Roadmaps, OKRs, user stories, RICE, MoSCoW | Definición de scope, priorización, acceptance criteria | [pm-po-advisor.md](./business-roles/pm-po-advisor.md) |
| **UX Research Expert** | User interviews, personas, usability testing, A/B testing | Diseñar tests de usabilidad, interpretar resultados | [ux-research-expert.md](./business-roles/ux-research-expert.md) |
| **UX/UI Designer** | Figma, Sketch, design systems, prototyping, WCAG | Wireframes, mockups, prototypes, design systems | [ux-ui-designer.md](./business-roles/ux-ui-designer.md) |
| **Scrum Master / Agile Coach** | Scrum, Kanban, retrospectives, sprint planning | Facilitar ceremonias, remover impedimentos, coaching | [scrum-master-agile-coach.md](./business-roles/scrum-master-agile-coach.md) |
| **Tech Lead / EM Advisor** | 1-on-1s, performance reviews, hiring, roadmaps | Gestión de equipos, mentoring, technical strategy | [tech-lead-em-advisor.md](./business-roles/tech-lead-em-advisor.md) |
| **SRE Advisor** | SLIs/SLOs/SLAs, incident response, on-call, runbooks | Definir SLOs, runbooks, incident management | [sre-advisor.md](./business-roles/sre-advisor.md) |
| **DevRel / Developer Advocate** | Community building, conference talks, blog posts | Crear contenido técnico, engagement con comunidad | [devrel-developer-advocate.md](./business-roles/devrel-developer-advocate.md) |

---

### 🔍 Debugging y Troubleshooting

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Debugging & Root Cause Expert** | Tracing (Jaeger), profiling (pprof, py-spy), flamegraphs | Diagnóstico de incidentes, RCA, performance profiling | [debugging-root-cause-expert.md](./debugging/debugging-root-cause-expert.md) |
| **Migration Specialist** | DB migration tools, feature flags, canary deploys | Planes de migración, rollback strategies, zero-downtime | [migration-specialist.md](./debugging/migration-specialist.md) |
| **Code Review Expert** | GitHub/GitLab PR reviews, SonarQube, ESLint | Code review best practices, feedback efectivo | [code-review-expert.md](./debugging/code-review-expert.md) |

---

### 🎯 Compliance y Gobernanza

| Agent Name | Stack | Propósito | Definición |
|:-----------|:------|:----------|:-----------|
| **Compliance & Governance Expert** | GDPR, SOC2, ISO 27001, audit trails, policies | Requisitos regulatorios, auditorías, controles de acceso | [compliance-governance-expert.md](./compliance/compliance-governance-expert.md) |
| **Accessibility Expert** | WCAG 2.1, ARIA, axe DevTools, screen readers | Auditorías de accesibilidad, remediación, WCAG compliance | [accessibility-expert.md](./compliance/accessibility-expert.md) |

---

## 📊 Resumen de Agentes

| Categoría | Cantidad | Agentes |
|:----------|:--------:|:--------|
| **Backend** | 8 | Java, Spring, Python, FastAPI, Django, Node.js, Express, NestJS |
| **Frontend** | 6 | TypeScript, React, Next.js, Angular, Vue, CSS/Styling |
| **Bases de Datos** | 5 | PostgreSQL, MSSQL, MongoDB, Redis, SQLite |
| **Arquitectura y DevOps** | 9 | Software Architecture, Microservices, Event-Driven, API Design, CI/CD, IaC, Observability, Security, Kubernetes |
| **Testing** | 5 | TDD, BDD, Performance Testing, E2E Testing, QA Automation |
| **Datos, ML y Finanzas** | 8 | Data Science, ML/DL, RL, NLP, Computer Vision, Quant Finance, Trading Systems, Data Visualization |
| **Documentación** | 4 | Technical Writer, API Documentation, Content Creator, Diagram |
| **Roles de Negocio** | 7 | PM/PO, UX Research, UX/UI Designer, Scrum Master, Tech Lead/EM, SRE, DevRel |
| **Debugging** | 3 | Debugging & Root Cause, Migration Specialist, Code Review |
| **Compliance** | 2 | Compliance & Governance, Accessibility |
| **TOTAL** | **57** | |

---

## 📚 Recursos

### Documentación de Claude Code

- [Claude Code - Subagentes (Documentación Oficial)](https://code.claude.com/docs/es/sub-agents) - Guía completa sobre cómo crear y usar subagentes en Claude Code

### Prompt Engineering y Patrones

- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Anthropic Prompt Library](https://docs.anthropic.com/claude/prompt-library)
- [Clean Architecture - Robert Martin](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164)
- [Refactoring Guru - Patrones](https://refactoring.guru/design-patterns)

---

[⬅️ Anterior: Convenciones](../manuales/convenciones.md) | [⬆️ Volver arriba](#prompts-y-agentes-de-ia) | [➡️ Siguiente: Estrategia de IA](../manuales/estrategia-ia-automatizacion.md)
