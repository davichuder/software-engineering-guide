# 📘 Guía Integral de Ingeniería de Software

[![MkDocs](https://img.shields.io/badge/MkDocs-1.6.1-blue)](https://www.mkdocs.org/)
[![Material for MkDocs](https://img.shields.io/badge/Material-9.7.0-blue)](https://squidfunk.github.io/mkdocs-material/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> 🚀 Un recurso vivo y colaborativo para estandarizar, guiar y potenciar el desarrollo de software

## 📖 Descripción

Esta es una **Guía Integral de Ingeniería de Software** diseñada como un compendio de sabiduría colectiva que destila las mejores prácticas de la industria, lecciones aprendidas y decisiones técnicas para construir software de clase mundial.

La documentación abarca todo el ciclo de vida del desarrollo de software, desde los fundamentos del código limpio hasta estrategias avanzadas de arquitectura, DevOps, seguridad y gestión de producto.

## 📄 Documentación Online

La guía está publicada en GitHub Pages y se puede consultar aquí:

👉 **[Ver la Guía Online](https://davichuder.github.io/software-engineering-guide/)**

## ✨ Características

- 📚 **28+ secciones temáticas** cubriendo todos los aspectos de ingeniería de software
- 🎨 **Interfaz moderna** con Material Design y soporte para modo claro/oscuro
- 🔍 **Búsqueda integrada** para encontrar información rápidamente
- 📊 **Diagramas interactivos** con Mermaid y PlantUML
- 🕐 **Fechas de revisión** automáticas con git-revision-date
- 🌐 **Totalmente en español** con localización completa
- 📱 **Responsive** - funciona en desktop, tablet y móvil

## 🎯 Audiencia

Esta guía está diseñada para:

- **Nuevos ingresos**: Como hoja de ruta para el onboarding y entendimiento de la cultura técnica
- **Desarrolladores experimentados**: Como referencia rápida de patrones, estándares y herramientas
- **Líderes técnicos**: Como base para la toma de decisiones y la mentoría
- **Arquitectos**: Para diseñar sistemas robustos y escalables
- **Product Managers**: Para entender el contexto técnico y tomar mejores decisiones
- **DevOps/SRE**: Para implementar mejores prácticas de operaciones

## 📋 Contenido

### 🎯 Fundamentos

- Niveles de criticidad, reglas generales de código, reglas por lenguaje y framework

### 🔬 Desarrollo y Testing

- TDD, BDD, ATDD, DDD, FDD, MDD, PBT
- Testing de backend, frontend, mobile, performance

### 🏗️ Arquitectura y Diseño

- Arquitecturas de software, patrones de diseño, FSM

### 🚀 Operaciones

- DevOps, CI/CD, seguridad, observabilidad, performance

### 💾 Datos y APIs

- Bases de datos (SQL, NoSQL, Time Series, Graph)
- APIs (REST, GraphQL, gRPC, WebSockets)

### 📱 Interfaces y Experiencia

- Desarrollo móvil, UI/UX, accesibilidad

### ☁️ Infraestructura

- Multi-cloud, serverless, containerization, edge computing

### 🤖 Datos Avanzados

- Machine Learning, Deep Learning, MLOps, Ciencia de Datos

### ✅ Calidad y Gestión

- Code coverage, static analysis, linting, peer review

### 🛠️ Resolución de Problemas

- Ishikawa, 5 Porqués, Pareto, Six Sigma, Kaizen, Lean

### 📊 Estrategia y Negocio

- FODA, PESTEL, Porter, Product Management, OKRs, KPIs

### 👥 Cultura y Colaboración

- Pair programming, code review, postmortems, FinOps

### 📝 Documentación

- Markdown, Mermaid, PlantUML, C4, convenciones, templates

## 🚀 Quick Start

Ver [GETTING_STARTED.md](GETTING_STARTED.md) para instrucciones detalladas de instalación y uso.

### Instalación Rápida

```bash
# Clonar el repositorio
git clone <repository-url>
cd mkdocs

# Crear entorno virtual
python -m venv env

# Activar entorno virtual
# En Windows:
.\env\Scripts\activate
# En Linux/Mac:
source env/bin/activate

# Instalar dependencias
pip install -r requeriments.txt

# Servir localmente
mkdocs serve
```

Visita <http://127.0.0.1:8000> en tu navegador.

## 🛠️ Tecnologías

- **[MkDocs](https://www.mkdocs.org/)** - Generador de sitios estáticos
- **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** - Tema moderno y responsive
- **[PlantUML](https://plantuml.com/)** - Diagramas UML
- **[Mermaid](https://mermaid.js.org/)** - Diagramas en markdown
- **[Git Revision Date Plugin](https://github.com/timvink/mkdocs-git-revision-date-localized-plugin)** - Fechas de última modificación

## 📂 Estructura del Proyecto

```text
mkdocs/
├── docs/                           # Documentación en Markdown
│   ├── index.md                   # Página principal
│   ├── indice.md              # Índice general
│   ├── fundamentos.md         # Fundamentos
│   ├── ...                       # Más secciones (28)
│   └── reportes/                  # Templates y ejemplos
│       ├── templates/            # Plantillas de reportes
│       └── examples/             # Ejemplos de reportes
├── mkdocs.yml                     # Configuración de MkDocs
├── requeriments.txt              # Dependencias Python
├── env/                          # Entorno virtual (no versionado)
└── README.md                     # Este archivo
```

## 🤝 Contribuciones

Este documento es vivo y colaborativo. ¡Tus contribuciones son bienvenidas!

### Cómo Contribuir

1. **Proponer mejoras**: Abre un Pull Request con tus cambios sugeridos
2. **Reportar errores**: Crea un Issue con la etiqueta `docs`
3. **Agregar ejemplos**: Incluye ejemplos concisos con enlaces
4. **Actualizar herramientas**: Mantén versiones y links actualizados

### Proceso de Contribución

```bash
# 1. Fork el repositorio
# 2. Crea una rama para tu feature
git checkout -b feature/mi-mejora

# 3. Realiza tus cambios
# 4. Commit con mensaje descriptivo
git commit -m "docs: agregar sección sobre microservicios"

# 5. Push a tu fork
git push origin feature/mi-mejora

# 6. Abre un Pull Request
```

## 📝 Niveles de Criticidad

| Criticidad | Emoji | Explicación                                  |
| ---------- | ----- | -------------------------------------------- |
| Crítico    | 🔴    | Incumplimiento = bug de seguridad o caída    |
| Alto       | 🟠    | Afecta mantenibilidad o rendimiento          |
| Estilo     | 🟢    | Preferencia de equipo, sin impacto funcional |

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

## 👤 Mantenedores

- **David Rolón** - [@davichuder](https://github.com/davichuder)

## 📚 Recursos Adicionales

- [Refactoring Guru - Patrones de Diseño](https://refactoring.guru/design-patterns)
- [Martin Fowler - Architecture](https://martinfowler.com/architecture/)
- [OWASP - Security](https://owasp.org/)
- [12 Factor App](https://12factor.net/)
- [Google SRE Book](https://sre.google/books/)

## 🌟 Agradecimientos

Gracias a todos los contribuidores que han ayudado a hacer de esta guía un recurso valioso para la comunidad.

---

> _"La excelencia no es un acto, sino un hábito."_ - Aristóteles

**¿Listo para comenzar?** 👉 [Ver Getting Started](GETTING_STARTED.md)
