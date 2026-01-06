# Priorización y Roadmapping

> Frameworks de priorización y estrategias de roadmapping para productos y proyectos técnicos.

---

## 🎯 RICE Framework

**Componentes:**

- **R**each: Cuántos usuarios afecta (por período)
- **I**mpact: Qué tanto impacta (0.25 = mínimo, 3 = masivo)
- **C**onfidence: Qué tan seguros estamos (0% - 100%)
- **E**ffort: Cuánto esfuerzo requiere (person-months)

**Fórmula:**

```text
RICE Score = (Reach × Impact × Confidence) / Effort
```

**Ejemplo:**

| Feature | Reach | Impact | Confidence | Effort | RICE Score |
| :-------- | :------ | :------- | :----------- | :------- | :----------- |
| Checkout 1-click | 10000/mes | 3 (masivo) | 80% | 2 months | **12000** |
| Dark mode | 5000/mes | 0.5 (bajo) | 100% | 1 month | **2500** |
| Notificaciones push | 8000/mes | 1 (medio) | 50% | 3 months | **1333** |

**Prioridad:** Checkout 1-click > Dark mode > Notificaciones push

---

## 📊 MoSCoW

**Categorías:**

- **M**ust have: Crítico, sin esto no lanzamos
- **S**hould have: Importante, pero podemos lanzar sin esto
- **C**ould have: Nice to have, si hay tiempo
- **W**on't have: Fuera de scope para esta iteración

**Ejemplo:**

| Feature | Categoría | Razón |
| :-------- | :---------- | :------ |
| Login con email/password | **Must** | Sin esto no hay producto |
| Login con Google | **Should** | Mejora UX pero no crítico |
| Login con biometría | **Could** | Nice to have |
| Login con blockchain | **Won't** | Fuera de scope |

---

## 😊 Kano Model

**Tipos de features:**

| Tipo | Descripción | Ejemplo |
| :----- | :------------ | :-------- |
| **Basic** | Esperadas, su ausencia causa insatisfacción | Login, búsqueda |
| **Performance** | Más es mejor, satisfacción lineal | Velocidad, precio |
| **Excitement** | Inesperadas, causan deleite | Recomendaciones personalizadas |

**Estrategia:**

1. **Basic**: Implementar primero (table stakes)
2. **Performance**: Optimizar continuamente
3. **Excitement**: Diferenciador competitivo

---

## 📈 Value vs Effort Matrix

**Cuadrantes:**

```text
Alto Valor
    │
    │  Quick Wins     │  Major Projects
    │  (Hacer YA)     │  (Planificar)
────┼─────────────────┼──────────────── Bajo Esfuerzo → Alto Esfuerzo
    │  Fill-ins       │  Money Pit
    │  (Si hay tiempo)│  (Evitar)
    │
Bajo Valor
```

**Ejemplo:**

| Feature | Valor | Esfuerzo | Cuadrante |
| :-------- | :------ | :--------- | :---------- |
| Fix bug crítico | Alto | Bajo | **Quick Win** |
| Migración a microservicios | Alto | Alto | **Major Project** |
| Cambiar color de botón | Bajo | Bajo | **Fill-in** |
| Reescribir en Rust | Bajo | Alto | **Money Pit** |

---

## 🗺️ Roadmapping

### Now-Next-Later

**Estructura:**

- **Now** (0-3 meses): Features en desarrollo
- **Next** (3-6 meses): Features planificadas
- **Later** (6-12 meses): Ideas, exploración

**Ventajas:**

- ✅ Flexible (no commitea fechas específicas)
- ✅ Fácil de comunicar
- ❌ Poco preciso para planning detallado

---

### Roadmap Basado en Temas

**Estructura:**

- Organizar por themes (ej: "Performance", "UX", "Security")
- Cada theme tiene múltiples initiatives
- Timeline aproximado por theme

**Ejemplo:**

| Q1 2024 | Q2 2024 | Q3 2024 |
| :-------- | :-------- | :-------- |
| **Performance**; - Optimizar DB; - CDN | **UX**; - Redesign checkout; - Mobile app | **Security**; - 2FA; - Audit logs |

---

### Roadmap Basado en Resultados

**Estructura:**

- Definir outcomes (resultados de negocio)
- Features son medios para lograr outcomes

**Ejemplo:**

| Outcome | Métrica | Initiatives |
| :-------- | :-------- | :------------ |
| Aumentar conversión | +15% conversion rate | Checkout 1-click, A/B testing, Optimizar performance |
| Reducir churn | - 20% churn | Onboarding mejorado, Notificaciones, Customer success |

---

## 📋 Artefactos

### RICE Scoring Template

```markdown
# RICE Scoring: [Feature Name]

## Reach
**Estimado:** [N] usuarios/mes
**Fuente:** [Analytics / User research / Estimación]

## Impact
**Score:** [0.25 / 0.5 / 1 / 2 / 3]
**Justificación:** [Por qué este score]

Escala:
- 3 = Masivo impacto
- 2 = Alto impacto
- 1 = Medio impacto
- 0.5 = Bajo impacto
- 0.25 = Mínimo impacto

## Confidence
**Score:** [%]
**Justificación:** [Por qué este nivel de confianza]

Escala:
- 100% = Datos sólidos
- 80% = Datos medios
- 50% = Estimación educada

## Effort
**Estimado:** [N] person-months
**Breakdown:**
- Design: [N] weeks
- Development: [N] weeks
- Testing: [N] weeks

## RICE Score
```

(Reach × Impact × Confidence) / Effort
= ([R] × [I] × [C]%) / [E]
= [RICE Score]

```text

## Prioridad
[Alta / Media / Baja] basado en RICE score
```

---

### Roadmap Template

```markdown
# Product Roadmap: [Product Name]

**Period:** [Q1 2024 - Q4 2024]
**Last updated:** [YYYY-MM-DD]

## Vision
[Visión de producto a 1-2 años]

## Goals
1. [Goal 1]: [Métrica objetivo]
2. [Goal 2]: [Métrica objetivo]

## Now (Q1 2024)

### Theme: [Theme Name]
**Outcome:** [Resultado esperado]
**Metrics:** [Cómo medimos éxito]

**Initiatives:**
- [x] [Initiative 1] - Completed
- [/] [Initiative 2] - In Progress (60%)
- [ ] [Initiative 3] - Planned

**Dependencies:**
- [Dependency 1]

## Next (Q2 2024)

### Theme: [Theme Name]
[Mismo formato]

## Later (Q3-Q4 2024)

### Theme: [Theme Name]
[Mismo formato]

## Out of Scope
- [Feature X]: [Razón]
- [Feature Y]: [Razón]
```

---

### Prioritization Workshop Guide

```markdown
# Prioritization Workshop

**Duration:** 2 hours
**Participants:** PM, Tech Lead, Designer, Stakeholders
**Facilitator:** [Name]

## Agenda

### 1. Context (15 min)
- Review product goals
- Review constraints (time, resources)

### 2. Brainstorm (30 min)
- Cada participante escribe ideas en post-its
- Sin filtros, todas las ideas son válidas

### 3. Clustering (15 min)
- Agrupar ideas similares
- Eliminar duplicados

### 4. Scoring (45 min)
- Usar RICE framework
- Cada idea se puntúa en grupo

### 5. Prioritization (15 min)
- Ordenar por RICE score
- Discutir top 5
- Decidir qué va a Now/Next/Later

## Output
- Lista priorizada de features
- Roadmap actualizado
- Action items con owners
```

---

## 📚 Recursos

- [RICE Framework - Intercom](https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/)
- [MoSCoW Method](https://www.productplan.com/glossary/moscow-prioritization/)
- [Kano Model](https://www.productplan.com/glossary/kano-model/)
