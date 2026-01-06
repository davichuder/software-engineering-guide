# Plantillas y Artefactos

> Plantillas reutilizables para decisiones técnicas, planificación y operaciones.

---

## 📝 Introducción

**Qué:** Templates reutilizables para documentar decisiones, procedimientos y respuestas a incidentes.

**Por qué:** Evitar reinventar la rueda, mantener consistencia, facilitar onboarding.

**Cómo:** Usar templates como punto de partida, adaptar según necesidad.

---

## 📓 Decision Journal

**Qué es:** Registro de decisiones técnicas con contexto, alternativas y outcome.

**Cuándo usar:**

- Decisiones arquitectónicas importantes
- Trade-offs no obvios
- Cambios que afectan múltiples equipos

### Template

```markdown
# Decision Journal: [Título de la Decisión]

**Fecha:** YYYY-MM-DD
**Decidido por:** [Nombre/Equipo]
**Stakeholders:** [Quiénes se ven afectados]

## Contexto
[Situación actual, constraints, por qué necesitamos decidir]

## Opciones Consideradas

### Opción 1: [Nombre]
**Descripción:** [Qué es]
**Pros:**
- [Pro 1]
- [Pro 2]

**Cons:**
- [Con 1]
- [Con 2]

**Costo estimado:** [Tiempo/dinero]

### Opción 2: [Nombre]
[Mismo formato]

### Opción 3: [Nombre]
[Mismo formato]

## Criterios de Decisión
| Criterio | Peso | Opción 1 | Opción 2 | Opción 3 |
|:---------|:----:|:--------:|:--------:|:--------:|
| Performance | 30% | 8/10 | 6/10 | 9/10 |
| Costo | 25% | 6/10 | 9/10 | 4/10 |
| Mantenibilidad | 25% | 7/10 | 8/10 | 5/10 |
| Time to market | 20% | 5/10 | 9/10 | 3/10 |
| **Total** | | **6.6** | **8.0** | **5.5** |

## Decisión Tomada
**Opción elegida:** [Opción X]

**Razón:** [Por qué elegimos esta opción]

**Trade-offs aceptados:** [Qué sacrificamos]

## Outcome Esperado
- [Resultado 1]
- [Resultado 2]

## Outcome Real (actualizar después)
**Fecha de revisión:** YYYY-MM-DD

**Resultados:**
- [Resultado real 1]
- [Resultado real 2]

**¿Tomaríamos la misma decisión?** [Sí/No/Depende]

**Lecciones aprendidas:**
- [Lección 1]
- [Lección 2]
```

---

### Ejemplo Real

```markdown
# Decision Journal: Base de Datos para Sistema de Pagos

**Fecha:** 2015
**Decidido por:** Tech Lead + Architect
**Stakeholders:** Backend team, DevOps, Finance

## Contexto
Necesitamos elegir DB para nuevo sistema de pagos. Requisitos:
- Transacciones ACID (crítico para consistencia financiera)
- Throughput: 1000 TPS
- Latency: p99 <100ms
- Compliance: Auditoría completa de transacciones

## Opciones Consideradas

### Opción 1: PostgreSQL
**Pros:**
- ACID garantizado
- Expertise del equipo (ya usamos en otros sistemas)
- Costo bajo (open-source)

**Cons:**
- Escalado vertical limitado
- Sharding manual si necesitamos escalar horizontalmente

**Costo:** $0 (open-source) + $500/mes cloud hosting

### Opción 2: MongoDB
**Pros:**
- Escalado horizontal fácil
- Flexible schema

**Cons:**
- ACID solo en transacciones multi-documento (desde v4.0)
- Menos expertise del equipo
- Riesgo para datos financieros críticos

**Costo:** $800/mes (Atlas M30)

### Opción 3: CockroachDB
**Pros:**
- ACID + escalado horizontal
- Compatible con PostgreSQL

**Cons:**
- Menos maduro que PostgreSQL
- Costo alto
- Poca expertise del equipo

**Costo:** $2000/mes (cloud)

## Decisión Tomada
**Opción elegida:** PostgreSQL

**Razón:**
- ACID es requisito no negociable
- Equipo tiene expertise
- 1000 TPS está dentro de capacidad de PostgreSQL bien tuneado
- Si necesitamos escalar, podemos migrar a CockroachDB (compatible)

**Trade-offs aceptados:**
- Escalado horizontal más difícil (pero no necesario en corto plazo)

## Outcome Esperado
- Latency p99 <100ms
- 0 inconsistencias en transacciones
- Time to market: 2 meses

## Outcome Real
**Fecha de revisión:** 2015

**Resultados:**
- Latency p99: 45ms ✅
- 0 inconsistencias ✅
- Time to market: 2.5 meses (delay por integración con payment gateway)

**¿Tomaríamos la misma decisión?** Sí

**Lecciones:**
- PostgreSQL fue la elección correcta para nuestro scale
- Invertir en monitoring desde día 1 fue clave para detectar bottlenecks temprano
```

---

## ⚠️ Pre-Mortem

**Qué es:** Ejercicio de anticipar qué puede salir mal ANTES de lanzar.

**Cuándo usar:**

- Antes de lanzar features críticas
- Antes de migraciones complejas
- Antes de eventos de alto tráfico

### Template Pre-Mortem

```markdown
# Pre-Mortem: [Proyecto/Feature]

**Fecha:** YYYY-MM-DD
**Participantes:** [Equipo]
**Proyecto:** [Descripción breve]

## Escenario: "El proyecto falló. ¿Qué salió mal?"

### Escenario de Fallo 1: [Título]
**Descripción:** [Qué salió mal]
**Probabilidad:** [Alta/Media/Baja]
**Impacto:** [Alto/Medio/Bajo]
**Señales de alerta:** [Cómo detectarlo temprano]
**Mitigación:** [Qué hacer para prevenir]

### Escenario de Fallo 2: [Título]
[Mismo formato]

## Plan de Mitigación Priorizado

| Escenario | Probabilidad | Impacto | Prioridad | Acción | Responsable |
|:----------|:-------------|:--------|:----------|:-------|:------------|
| [Escenario 1] | Alta | Alto | 🔴 P0 | [Acción] | [Persona] |
| [Escenario 2] | Media | Alto | 🟠 P1 | [Acción] | [Persona] |
| [Escenario 3] | Alta | Medio | 🟠 P1 | [Acción] | [Persona] |

## Checklist Pre-Launch
- [ ] [Mitigación 1 implementada]
- [ ] [Mitigación 2 implementada]
- [ ] [Monitoring configurado]
- [ ] [Rollback plan testeado]
```

---

## 📖 Runbook

**Qué es:** Procedimiento operacional paso a paso.

**Cuándo usar:**

- Deployments
- Rollbacks
- Tareas operacionales repetitivas

### Template Runbook

```markdown
# Runbook: [Título del Procedimiento]

**Propósito:** [Para qué sirve este runbook]
**Frecuencia:** [Cuándo se ejecuta]
**Duración estimada:** [Tiempo]
**Responsable:** [Rol/Equipo]

## Pre-requisitos
- [ ] [Requisito 1]
- [ ] [Requisito 2]
- [ ] [Acceso a X sistema]

## Pasos

### 1. [Título del paso]
**Acción:**
```bash
# Comando exacto
comando --flag valor
```

**Output esperado:**

```text
[Output esperado]
```

**Si falla:**

- [Qué hacer]
- [A quién contactar]

### 2. [Título del paso]

[Mismo formato]

## Rollback

### Si algo sale mal en paso X

```bash
# Comandos de rollback
```

## Verificación

- [ ] [Verificación 1]
- [ ] [Verificación 2]
- [ ] [Verificación 3]

## Troubleshooting

### Problema: [Descripción]

**Síntomas:** [Cómo se manifiesta]
**Causa:** [Por qué pasa]
**Solución:** [Cómo arreglarlo]

## Contactos

- **On-call:** [Slack channel / PagerDuty]
- **Escalation:** [Manager / Tech Lead]

```text

---

## 🚨 Incident Response Playbook

**Qué es:** Plan de respuesta para diferentes tipos de incidentes.

**Cuándo usar:**
- Outages
- Security breaches
- Data loss

### Template

```markdown
# Incident Response Playbook: [Tipo de Incidente]

## Severidad

| Nivel | Descripción | Ejemplo | Response Time |
|:------|:------------|:--------|:--------------|
| **SEV1** | Outage completo | Sistema down | <15 min |
| **SEV2** | Degradación severa | Latency 10x | <30 min |
| **SEV3** | Degradación menor | Feature no crítica down | <2 hours |

## Roles

| Rol | Responsabilidad | Persona |
|:----|:----------------|:--------|
| **Incident Commander** | Coordinar respuesta | On-call SRE |
| **Tech Lead** | Debugging técnico | On-call Engineer |
| **Comms Lead** | Comunicación stakeholders | PM/EM |

## Response Flow

### 1. Detection (0-5 min)
- [ ] Alerta recibida (PagerDuty / Slack)
- [ ] Incident Commander asignado
- [ ] Crear incident channel (#incident-YYYY-MM-DD-HH-MM)

### 2. Assessment (5-15 min)
- [ ] Determinar severidad (SEV1/2/3)
- [ ] Identificar sistemas afectados
- [ ] Estimar impacto (usuarios, revenue)
- [ ] Comunicar status inicial

### 3. Mitigation (60 min)
- [ ] Implementar fix temporal (si es posible)
- [ ] Rollback (si es deployment reciente)
- [ ] Escalar a on-call senior (si no se resuelve en 30 min)

### 4. Resolution
- [ ] Implementar fix permanente
- [ ] Verificar que sistema está estable
- [ ] Comunicar resolución

### 5. Post-Incident (48 hours)
- [ ] Escribir post-mortem
- [ ] Identificar action items
- [ ] Asignar responsables

## Communication Templates

### Status Update (cada 30 min durante incidente)
```

**Status:** [Investigating / Identified / Monitoring / Resolved]
**Impact:** [Descripción]
**ETA:** [Estimado]
**Next update:** [Tiempo]

```text

### Resolution Message
```

**Incident:** [Título]
**Duration:** [Tiempo]
**Impact:** [Descripción]
**Root cause:** [Causa]
**Prevention:** [Qué haremos para evitarlo]

```text

## Runbooks por Tipo de Incidente

### Database Outage
1. Check DB health: `pg_isready -h [host]`
2. Check connections: `SELECT count(*) FROM pg_stat_activity;`
3. Check disk space: `df -h`
4. If connections maxed: Kill idle connections
5. If disk full: Clear logs, expand disk
6. Escalate to DBA if not resolved in 15 min

### API Latency Spike
1. Check APM (Datadog / New Relic)
2. Identify slow endpoints
3. Check DB query performance
4. Check external dependencies
5. Scale up if traffic spike
6. Rollback if recent deployment

### Security Breach
1. **DO NOT** try to fix immediately
2. Isolate affected systems
3. Preserve evidence (logs, snapshots)
4. Escalate to Security team
5. Follow security incident protocol
```

---

## 🏛️ Architecture Decision Record (ADR)

**Qué es:** Documento ligero para decisiones arquitectónicas.

**Cuándo usar:**

- Cambios arquitectónicos significativos
- Elección de tecnologías
- Patterns aplicados

### Template ADR

```markdown
# ADR-[número]: [Título]

**Status:** [Proposed / Accepted / Deprecated / Superseded by ADR-XXX]
**Date:** YYYY-MM-DD
**Deciders:** [Nombres]

## Context
[Descripción del problema y contexto]

## Decision
[Decisión tomada]

## Rationale
[Por qué tomamos esta decisión]

## Consequences

### Positive
- [Consecuencia positiva 1]
- [Consecuencia positiva 2]

### Negative
- [Consecuencia negativa 1]
- [Consecuencia negativa 2]

### Neutral
- [Consecuencia neutral 1]

## Alternatives Considered
- [Alternativa 1]: [Por qué no la elegimos]
- [Alternativa 2]: [Por qué no la elegimos]

## References
- [Link 1]
- [Link 2]
```

---

### Ejemplo Real ADR

```markdown
# ADR-007: Usar Event Sourcing para Sistema de Auditoría

**Status:** Accepted
**Date:** 2020
**Deciders:** Tech Lead, Architect, Compliance Lead

## Context
Necesitamos auditoría completa de cambios en datos financieros para compliance (SOC2).
Requisitos:
- Inmutabilidad de audit log
- Reconstruir estado en cualquier punto del tiempo
- Consultas eficientes de historial

## Decision
Implementar Event Sourcing para entidades críticas (Transactions, Accounts).

## Rationale
- Inmutabilidad: Events nunca se modifican, solo se agregan
- Time travel: Podemos reconstruir estado en cualquier timestamp
- Audit trail: Events son el audit log
- Compliance: Cumple requisitos de SOC2

## Consequences

### Positive
- Audit trail completo y inmutable
- Debugging más fácil (replay events)
- Cumple compliance

### Negative
- Complejidad: Curva de aprendizaje para el equipo
- Storage: Más espacio (guardamos todos los events)
- Eventual consistency: Read model puede estar desactualizado

### Neutral
- Necesitamos event store (usaremos EventStoreDB)
- Proyecciones para read models

## Alternatives Considered
- **Audit table tradicional**: Más simple pero mutable, no permite time travel
- **Database triggers**: Acoplado a DB, difícil de testear
- **Change Data Capture (CDC)**: Complejo de configurar, no es source of truth

## References
- [Event Sourcing - Martin Fowler](https://martinfowler.com/eaaDev/EventSourcing.html)
- [EventStoreDB](https://www.eventstore.com/)
```

---

## 📚 Recursos

- [ADR GitHub](https://adr.github.io/)
- [Pre-Mortem Technique - HBR](https://hbr.org/2007/09/performing-a-project-premortem)
- [Incident Response - PagerDuty](https://response.pagerduty.com/)
- [Runbook Template - Atlassian](https://www.atlassian.com/incident-management/devops/runbooks)
