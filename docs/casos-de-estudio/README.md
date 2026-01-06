# Casos de Estudio

> Análisis detallado de proyectos reales con todas las decisiones técnicas, arquitectónicas y de diseño justificadas.
>
> Cada caso de estudio documenta el contexto, las decisiones tomadas y las lecciones aprendidas.

---

## 📖 ¿Qué es un Caso de Estudio?

Un **caso de estudio** es un análisis profundo de un proyecto real que documenta:

- **Contexto del negocio**: Qué necesita el cliente y por qué
- **Decisiones técnicas**: Elección de lenguajes, frameworks y herramientas
- **Decisiones arquitectónicas**: Patrones, estructura y organización
- **Justificación**: Por qué se tomó cada decisión (trade-offs, alternativas consideradas)
- **Lecciones aprendidas**: Qué funcionó, qué no, y qué se haría diferente

---

## 🎯 Objetivo de esta Sección

Los casos de estudio sirven para:

1. **Aprender de la experiencia**: Ver cómo se aplican los conceptos de la guía en proyectos reales
2. **Justificar decisiones**: Entender el razonamiento detrás de cada elección técnica
3. **Evitar errores**: Conocer los problemas encontrados y cómo se resolvieron
4. **Inspirar soluciones**: Usar estos casos como referencia para proyectos similares

---

## 📚 Casos de Estudio Disponibles

- [**Portafolio Personal con TypeScript, Angular y SQLite**](./portafolio-personal-ts-angular.md)
    - **Stack**: TypeScript, Angular, SQLite
    - **Conceptos**: Arquitectura Hexagonal, Screaming Architecture, i18n, Design System
    - **Decisiones clave**: Por qué TypeScript sobre JavaScript, Angular sobre React, SQLite sobre PostgreSQL
    - **Artefactos**: ADRs, Decision Journal, Pre-Mortem

- [**Diario Digital de Emociones (Mood Tracker)**](./diario-emociones-mood-tracker.md)
    - **Stack**: FastAPI, React/Next.js, PostgreSQL + TimescaleDB, OAuth 2.0
    - **Conceptos**: 3 Modelos de Emociones (Ekman, Plutchik, PAD), ML (K-Means), Visualizaciones 3D, Series Temporales
    - **Decisiones clave**: Por qué 3 modelos, PostgreSQL + TimescaleDB sobre MongoDB, FastAPI sobre Django, Wizard multinivel
    - **Artefactos**: ADRs, JTBD, North Star Metric, Pattern Analysis

- [**Voice Volume Tracker para Windows**](./voice-volume-tracker-windows.md)
    - **Stack**: C# + .NET 8, WPF, NAudio, ML.NET/ONNX, Windows Service
    - **Conceptos**: Speaker Verification (ML), Procesamiento de Audio en Tiempo Real, FSM (Finite State Machine), DirectX Overlay
    - **Decisiones clave**: C# sobre Python/Electron, SpeechBrain ECAPA-TDNN, Arquitectura Hexagonal, Encriptación AES-256
    - **Artefactos**: ADRs, Property-Based Testing, Performance Profiling, Anti-patrones documentados
    - **Partes**: [Parte 1 (Arquitectura)](./voice-volume-tracker-windows.md) | [Parte 2 (ML y Alertas)](./voice-volume-tracker-parte2.md) | [Parte 3 (Seguridad)](./voice-volume-tracker-parte3.md)

- [**Alarma por Ubicación (Geofencing Alarm)**](./geofencing-alarm-mobile.md)
    - **Stack**: Flutter, Kotlin/Swift (Nativo), SQLite, AWS Serverless
    - **Conceptos**: Foreground Services, FSM (Máquina de Estados), Optimización de Batería, Offline-First
    - **Decisiones clave**: Híbrido Optimizado (Flutter UI + Core Nativo), Muestreo Adaptativo de GPS, Arquitectura Hexagonal
    - **Artefactos**: ADRs, Circuit Breaker Fallback, Pre-Mortem

---

## 🔗 Relación con otros Capítulos

Los casos de estudio integran conceptos de múltiples capítulos:

| Concepto | Capítulo de Referencia |
| :--------- | :---------------------- |
| **Arquitectura Hexagonal** | [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md) |
| **Screaming Architecture** | [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md) |
| **ADR (Architecture Decision Record)** | [34 - Plantillas y Artefactos](../manuales/plantillas-artefactos.md) |
| **Decision Journal** | [34 - Plantillas y Artefactos](../manuales/plantillas-artefactos.md) |
| **Pre-Mortem** | [34 - Plantillas y Artefactos](../manuales/plantillas-artefactos.md) |
| **i18n/l10n** | [29 - Convenciones](../manuales/convenciones.md) |
| **Design System** | [17 - Mobile, UI y UX](../manuales/mobile-ui-ux.md) |
| **Defensive Programming** | [09 - Seguridad](../manuales/seguridad.md) |
| **TDD/BDD** | [03 - Disciplinas de Desarrollo](../manuales/disciplinas-desarrollo.md) |

---

## 📝 Cómo Leer un Caso de Estudio

Cada caso de estudio sigue esta estructura:

1. **Resumen Ejecutivo**: Vista rápida del proyecto y tecnologías
2. **Contexto del Cliente**: Qué necesita y por qué
3. **Decisiones de Tecnología**: Stack elegido con justificación
4. **Decisiones de Arquitectura**: Patrones y estructura
5. **Decisiones de Diseño (UX/UI)**: Interfaz y experiencia de usuario
6. **Requisitos No Funcionales**: Seguridad, performance, calidad
7. **ADRs**: Decisiones arquitectónicas documentadas
8. **Lecciones Aprendidas**: Qué funcionó y qué no
9. **Métricas de Éxito**: Cómo se midió el resultado

---

## 🎓 Casos de Estudio Futuros

Próximos casos de estudio planificados:

- **E-commerce con Microservicios** (Node.js, React, PostgreSQL, Redis)
- **API REST con Clean Architecture** (Python, FastAPI, MongoDB)
- **Aplicación Mobile Multiplataforma** (React Native, Firebase)
- **Dashboard de Analytics en Tiempo Real** (Vue.js, WebSockets, InfluxDB)
- **Sistema de Gestión de Inventario** (Java, Spring Boot, MySQL)

---

## 🤝 Contribuir con un Caso de Estudio

Si tienes un proyecto que quieres documentar como caso de estudio:

1. **Usar la plantilla**: Seguir la estructura definida arriba
2. **Documentar decisiones**: Incluir ADRs y justificaciones
3. **Agregar diagramas**: Arquitectura, flujos, modelos de datos
4. **Incluir código relevante**: Snippets que ilustren decisiones clave
5. **Compartir lecciones**: Qué funcionó, qué no, y por qué
