# Índice General

> Documento vivo de estándares, buenas prácticas y decisiones técnicas del equipo.
>
> Si se encuentra una mejora, ¡se agradece su actualización!
>
> Última actualización: `YYYY-MM-DD`

---

## 📖 Índice General

### 🎯 Fundamentos

- [Fundamentos](./manuales/fundamentos.md)
    - Niveles de criticidad
    - Fundamentos de red (Cliente-Servidor, IP, DNS, Load Balancer)
    - Reglas generales de código
    - Reglas por lenguaje
    - Reglas por framework
- [Onboarding](./manuales/onboarding.md)
    - Guía de inicio, Arquitectura, Primer PR

### 🔬 Desarrollo y Testing

- [Disciplinas de Desarrollo](./manuales/disciplinas-desarrollo.md)
    - TDD, BDD, ATDD, DDD, FDD, MDD, PBT
- [Testing](./manuales/testing.md)
    - Backend, Frontend, Mobile, Performance, Testing Avanzado
- [Gestión de Calidad](./manuales/gestion-calidad.md)
    - Code coverage, Static analysis, Linting, Peer review

### 🏗️ Arquitectura y Diseño

- [Arquitectura y Patrones](./manuales/arquitectura-patrones.md)
    - Arquitecturas de software
    - Teorema CAP (Consistency, Availability, Partition Tolerance)
    - Escalabilidad: Vertical vs Horizontal
    - Arquitectura Hexagonal (Ports & Adapters)
    - Onion Architecture (Arquitectura de Cebolla)
    - Clean Architecture
    - Domain-Driven Design (DDD)
    - Screaming Architecture
    - Patrones de diseño
    - Patrones arquitectónicos avanzados
    - FSM (Finite State Machines)
- [Sesgos Cognitivos, Falacias y Leyes](./manuales/sesgos-falacias.md)
    - Sesgos cognitivos, Falacias lógicas, Leyes paradójicas, Efectos psicológicos

### 🚀 Operaciones

- [DevOps](./manuales/devops.md)
    - CI/CD, IaC, Contenedores, Patrones de despliegue
- [Seguridad](./manuales/seguridad.md)
    - Principios de seguridad, Herramientas, Patrones avanzados
- [Observabilidad y Telemetría](./manuales/observabilidad.md)
    - Logging, Metrics, Tracing, APM, Alerting, Health checks

### 🛠️ Resolución de Problemas y Mejora

- [Herramientas de Análisis de Problemas](./manuales/herramientas-problemas.md)
    - Ishikawa, 5 Porqués, Pareto, FTA, 5W2H, Lluvia de ideas
- [Metodologías de Mejora Continua](./manuales/mejora-continua.md)
    - Six Sigma, Kaizen, Lean, PDCA, 5S, 8D, Kanban, MTBF

### ⚡ Performance y Producción

- [Optimización de Performance](./manuales/performance.md)
    - Optimización de DB, Frontend, Backend, Caching
- [Checklist de Producción](./manuales/checklist-produccion.md)
    - Validaciones pre-deploy, Post-deploy verification, Rollback criteria

### 💾 Datos y APIs

- [Bases de Datos](./manuales/bases-datos.md)
    - SQL, NoSQL, Time Series, Graph, Columnar, In-memory
- [APIs y Protocolos](./manuales/apis-protocolos.md)
    - REST, GraphQL, gRPC, WebSockets, Event-Driven
    - Documentación por protocolo
    - Patrones de comunicación

### 📱 Interfaces y Experiencia

- [Mobile, UI y UX](./manuales/mobile-ui-ux.md)
    - Desarrollo móvil, UI, UX, Accesibilidad

### ☁️ Infraestructura y Costos

- [Infraestructura y Cloud](./manuales/infraestructura-cloud.md)
    - Multi-cloud, Serverless, Containerization, Edge computing, Escalabilidad
- [Optimización de Costos (FinOps)](./manuales/cost-optimization.md)
    - FinOps, Right-sizing, Reserved Instances, Cloud cost monitoring

### 🤖 Datos Avanzados

- [Machine Learning y Deep Learning](./manuales/machine-learning.md)
    - ML supervisado/no supervisado, DL, MLOps, NLP, RL
- [Ciencia de Datos](./manuales/ciencia-datos.md)
    - Limpieza, Visualización, Reproducibilidad, Modelado
- [Data Governance](./manuales/data-governance.md)
    - Data Lineage, Data Quality, MDM, Privacy by Design

### 📊 Estrategia y Negocio

- [Análisis Estratégico](./manuales/analisis-estrategico.md)
    - FODA, PESTEL, Porter, VRIO, CAME, Buyer Persona, ICP
- [Product Management](./manuales/product-management.md)
    - JTBD, User Story Mapping, OKRs, North Star Metric
- [Métricas y KPIs](./manuales/metricas-kpis.md)
    - HEART, AARRR, DORA, NPS, SLIs/SLOs/SLAs

### 👥 Roles y Cultura

- [Roles y Responsabilidades](./manuales/roles-responsabilidades.md)
    - Roles técnicos, Producto y negocio, Calidad, Operaciones, Datos, RACI Matrix
- [Colaboración y Cultura](./manuales/colaboracion-cultura.md)
    - Pair Programming, Code Review, Postmortems, Escalation

### 📝 Documentación y Convenciones

- [Documentación y Diagramas](./manuales/documentacion-diagramas.md)
    - Markdown, Mermaid, LaTeX, PlantUML, C4, ER, UML
    - Tipos de diagramas: flujo, secuencia, clases, estado
- [Convenciones](./manuales/convenciones.md)
    - Nomenclatura, Git/GitOps, i18n/l10n, Configuración, Dependencias

### 🤖 AI y Automatización

- [Prompts y Agentes de IA](agentes/README.md)
    - The Gentleman (agente principal), 57 Agentes especializados, Prompt engineering
- [Estrategia de IA y Automatización](./manuales/estrategia-ia-automatizacion.md)
    - Casos de uso prácticos, Límites de la IA, Integración en CI/CD

### ⚖️ Ética y Gobernanza

- [Ética y Gobernanza de IA](./manuales/etica-gobernanza-ia.md)
    - Bias en ML, Fairness metrics, Explicabilidad (XAI), Privacy, Gobernanza

### 📝 Comunicación y Artefactos

- [Comunicación y Contenido Técnico](./manuales/comunicacion-contenido.md)
    - Escritura para diferentes audiencias, Storytelling técnico, Content repurposing, SEO
- [Plantillas y Artefactos](./manuales/plantillas-artefactos.md)
    - Decision Journal, Pre-Mortem, Runbook, Incident Response Playbook, ADR

### 🔧 Gestión Técnica

- [Gestión de Dependencias y Deuda Técnica](./manuales/dependencias-deuda-tecnica.md)
    - Dependency management, Technical debt tracking, Refactoring strategies, Breaking changes
- [Priorización y Roadmapping](./manuales/priorizacion-roadmapping.md)
    - RICE Framework, MoSCoW, Kano Model, Value vs Effort Matrix, Roadmapping
- [Gestión de Secretos](./manuales/gestion-secretos.md)
    - Secret management tools, Secret rotation, Least privilege, Secrets en CI/CD, Detección

### 🛡️ Resiliencia y Datos

- [Chaos Engineering y Resiliencia](./manuales/chaos-engineering.md)
    - Chaos Engineering principles, Failure injection, Game Days, Resiliencia patterns
- [Data Literacy](./manuales/data-literacy.md)
    - Data literacy fundamentals, Self-service analytics, Data storytelling, Data quality

### 📝 Gobernanza Low-Code/No-Code (LCNC)

- [Gobernanza Low-Code/No-Code (LCNC)](./manuales/lowcode-nocode.md)
    - ¿Qué es LCNC Governance?, Riesgos Clave de LCNC, Políticas de Seguridad y Acceso, Data Governance para LCNC, Ciclo de Vida y Auditoría, Roles y Accountability, Anti-patrones, Recursos.

### 📚 Casos de Estudio

- [Casos de Estudio](casos-de-estudio/README.md)
    - Análisis detallado de proyectos reales con decisiones técnicas y arquitectónicas justificadas
    - **Portafolio Personal** (TypeScript, Angular, SQLite): Arquitectura Hexagonal, Screaming Architecture, i18n, ADRs

### 📝 Recursos de práctica de código y preparación para entrevistas

- [Recursos de práctica de código y preparación para entrevistas](./manuales/recursos-entrevistas.md)
    - Coding interview questions, Coding interview preparation, Coding interview tips, Coding interview resources.

### 📝 Glosario

- [Glosario](./glosario.md)
    - Glosario de términos técnicos y conceptos.

### 📝 Reportes y Templates

| Tipo de Reporte | Template | Ejemplo |
|:----------------|:---------|:--------|
| **Bug Report** | [📄 Ver Template](./reportes/templates/bug-report-template.md) | [🐛 Ver Ejemplo](./reportes/examples/bug-report-example.md) |
| **Feature Request** | [📄 Ver Template](./reportes/templates/feature-request-template.md) | [💡 Ver Ejemplo](./reportes/examples/feature-request-example.md) |
| **Post-Mortem** | [📄 Ver Template](./reportes/templates/post-mortem-template.md) | [💀 Ver Ejemplo](./reportes/examples/post-mortem-example.md) |
| **RFC** | [📄 Ver Template](./reportes/templates/rfc-template.md) | [📝 Ver Ejemplo](./reportes/examples/rfc-example.md) |

---

## 🎯 Cómo usar esta guía

### Para nuevos desarrolladores

1. Comenzar por [Fundamentos](./manuales/fundamentos.md)
2. Leer [Onboarding](./manuales/onboarding.md)
3. Consultar [Disciplinas de Desarrollo](./manuales/disciplinas-desarrollo.md)
4. Revisar convenciones del lenguaje/framework que usarás

### Para arquitectos

1. Revisar [Arquitectura y Patrones](./manuales/arquitectura-patrones.md)
2. Consultar [Infraestructura y Cloud](./manuales/infraestructura-cloud.md)
3. Validar contra [Seguridad](./manuales/seguridad.md)
4. Implementar [Observabilidad](./manuales/observabilidad.md)

### Para product managers

1. Estudiar [Product Management](./manuales/product-management.md)
2. Definir [Métricas y KPIs](./manuales/metricas-kpis.md)
3. Usar [Análisis Estratégico](./manuales/analisis-estrategico.md)
4. Aplicar [Herramientas de Problemas](./manuales/herramientas-problemas.md)

### Para DevOps/SRE

1. Implementar [DevOps](./manuales/devops.md)
2. Configurar [Observabilidad](./manuales/observabilidad.md)
3. Optimizar [Performance](./manuales/performance.md)
4. Gestionar [Infraestructura Cloud](./manuales/infraestructura-cloud.md)

### Para resolución de problemas

1. Aplicar [Herramientas de Problemas](./manuales/herramientas-problemas.md)
2. Usar [Mejora Continua](./manuales/mejora-continua.md)
3. Consultar [Testing](./manuales/testing.md)
4. Revisar [Observabilidad](./manuales/observabilidad.md)

---

## 📋 Niveles de Criticidad

| Criticidad | Abrev. | Explicación                                  |
| ---------- | ------ | -------------------------------------------- |
| Crítico    | 🔴     | Incumplimiento = bug de seguridad o caída.   |
| Alto       | 🟠     | Afecta mantenibilidad o rendimiento.         |
| Estilo     | 🟢     | Preferencia de equipo, sin impacto funcional.|

---

## 🤝 Contribuciones

Este documento es vivo y colaborativo:

1. **Proponer mejoras**: Abrir PR con cambios sugeridos
2. **Reportar errores**: Issues con etiqueta `docs`
3. **Agregar ejemplos**: Ejemplos concisos con enlaces
4. **Actualizar herramientas**: Mantener versiones y links actualizados

---

## 📚 Recursos Adicionales

- [Refactoring Guru - Patrones de Diseño](https://refactoring.guru/design-patterns)
- [Martin Fowler - Architecture](https://martinfowler.com/architecture/)
- [OWASP - Security](https://owasp.org/)
- [12 Factor App](https://12factor.net/)
- [Google SRE Book](https://sre.google/books/)

---

**Mantenedores**: David Rolón (<https://github.com/davichuder>)
