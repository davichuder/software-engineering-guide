# Onboarding de Nuevos Desarrolladores

> Proceso estructurado para incorporar nuevos miembros del equipo de forma rápida y efectiva.

---

## 🎯 Onboarding Efectivo

**Qué:** Proceso para que nuevo desarrollador sea productivo rápidamente y se sienta parte del equipo.

**Por qué:** Mal onboarding = 6 meses para ser productivo. Buen onboarding = 2-4 semanas.

**Quién:** Nuevo dev + buddy/mentor + tech lead.

**Cuándo:** Desde día 1, primeras 4-6 semanas.

**Cuánto:** Inversión de 30 horas del equipo, reduce turnover y acelera productividad.

---

## 📅 Timeline Sugerido

### Día 1: Setup y Bienvenida

**Mañana (9-12h):**

- Welcome meeting con equipo
- Setup laptop, accesos (email, Slack, GitHub, JIRA)
- Tour de oficina (si presencial)
- Lunch con equipo

**Tarde (18h):**

- Leer README principal del proyecto
- Clonar repositorios
- Setup ambiente de desarrollo local
- Primer build exitoso ✅

**Objetivo:** Sentirse bienvenido + ambiente funcionando.

### Semana 1: Contexto y Primeros Pasos

**Lunes-Miércoles:**

- Onboarding general empresa (HR, benefits, cultura)
- Meeting 1-on-1 con manager
- Leer docs de arquitectura
- Explorar codebase (grep, code reading)
- Shadowing: observar daily standup, planning

**Jueves-Viernes:**

- Primer ticket: bug fix pequeño o mejora de docs
- Pair programming con buddy
- Primera PR (con mucha ayuda)
- Code review de PRs del equipo (observar)

**Objetivo:** Entender contexto + primera contribución pequeña.

### Semana 2-3: Ramp Up

- Tickets de complejidad creciente
- Pair programming 2-3 veces/semana
- Participar activamente en dailies
- Hacer preguntas (crear cultura de curiosidad)
- Primera feature pequeña end-to-end

**Objetivo:** Autonomía creciente, confianza.

### Semana 4-6: Autonomía

- Features medianas independientes
- Menos pair programming, más async support
- Participar en design discussions
- Onboarding de siguiente nuevo dev (enseñar = aprender)

**Objetivo:** Contribuidor full, integrado al equipo.

---

## 📚 Onboarding Docs Esenciales

### 1. GETTING_STARTED.md

**Contenido:**

 **Estructura Esperada:**

 1. **Prerrequisitos:** Versiones exactas de lenguajes, DBs y herramientas (Docker).
 2. **Instalación:** Comandos paso a paso para levantar el entorno desde cero.
 3. **Configuración:** Explicación de variables de entorno (`.env`) y secrets.
 4. **Ejecución:** Cómo correr la app, tests y linters.
 5. **Troubleshooting:** Solución a errores comunes de setup.
 6. **Verificación:** Cómo confirmar que todo está funcionando ("It works!").

 **Ejemplo:**

```markdown
# Getting Started

## Prerequisites
- Node.js 18+
- Docker Desktop
- PostgreSQL 14+

## Setup (5 minutos)
1. Clone repo: `git clone ...`
2. Install deps: `npm install`
3. Copy env: `cp .env.example .env`
4. Start services: `docker-compose up -d`
5. Run migrations: `npm run migrate`
6. Start dev server: `npm run dev`
7. Open http://localhost:3000 (verificar puerto en .env/README)

## Verify Setup
- [ ] App loads sin errores
- [ ] Tests pasan: `npm test`
- [ ] Linter pasa: `npm run lint`

## Troubleshooting
**Error: "Port 3000 already in use"**
- Run: `lsof -ti:3000 | xargs kill -9`

**Need help?** Ask in #engineering-help Slack channel
```

### 2. ARCHITECTURE.md

**Secciones:**

- Overview diagram (C4 Context)
- Tech stack con versiones
- Estructura de carpetas
- Flujo request típico
- Decisiones arquitecturales (ADRs)
- Enlaces a docs detalladas

### 3. CONTRIBUTING.md

**Contenido:**

- Branch naming
- Commit messages (Conventional Commits)
- PR template y checklist
- Code review guidelines
- Testing requirements

### 4. ONBOARDING_TASKS.md

**Estructura Esperada:**

- **Por Tiempo:** Dividir tareas por Día 1, Semana 1, Mes 1.
- **Accionables:** Tareas concretas (leer X, instalar Y, deployar Z).
- **Social:** Incluir interacciones con el equipo (café, pair programming).
- **Checkboxes:** Formato interactivo para dar sensación de progreso.

 **Checklist interactivo (Ejemplo):**

```markdown
## Week 1
- [ ] Setup ambiente local
- [ ] Leer ARCHITECTURE.md
- [ ] Explorar codebase
- [ ] Completar ticket: #123 (docs update)
- [ ] Primera PR merged
- [ ] Conocer al equipo (lunch/coffee)

## Week 2-3
- [ ] Pair programming con 3 personas diferentes
- [ ] Fix bug: #456
- [ ] Implementar feature pequeña: #789
- [ ] Participar en retro
- [ ] Dar feedback sobre onboarding

## Week 4+
- [ ] Feature mediana end-to-end
- [ ] Presentar trabajo en team meeting
- [ ] Revisar PRs de otros
```

---

## 👥 Buddy System

**What:** Asignar mentor/buddy al nuevo dev.

**Responsabilidades del Buddy:**

- Responder preguntas (no juzgar ninguna pregunta)
- Pair programming primeras 2 semanas
- Code review de primeras PRs
- Introducir a equipo, cultura, unwritten rules
- Check-in diario primera semana, 2x/semana después

**Perfil ideal:**

- Mid-senior level (no junior, no staff)
- Paciente, buen comunicador
- Conoce bien el codebase
- Voluntario (no forzado)

**Rotación:** Rotar buddies cada 2-3 nuevos devs para evitar burnout.

---

## 🎓 Learning Path

### Recursos Internos

| Tipo | Contenido | Cuándo |
| :----- | :---------- | :------- |
| **Video walkthroughs** | Tour de codebase, demos | Semana 1 |
| **Tech talks grabados** | Decisiones arquitecturales, postmortems | Asíncrono |
| **Wiki interno** | Runbooks, FAQs, How-tos | Referencia continua |
| **Slack channels** | #engineering-help, #engineering-random | Desde día 1 |

### Primeros Tickets

**Características:**

- Bien definidos (acceptance criteria claros)
- Scope pequeño (< 1 día)
- No críticos (ok si toma más tiempo)
- Varias opciones para elegir según interés

**Ejemplos:**

- Actualizar README con screenshots
- Agregar tests a función existente
- Fix typo en error messages
- Mejorar logging de endpoint específico
- Agregar validación faltante

---

## 🔍 Explorar Codebase

**Técnicas:**

### 1. Code Reading Sessions

- 30 min diarios leyendo código
- Seguir un flujo completo (HTTP request → DB → response)
- Anotar preguntas

### 2. Grep/Search Patterns

```bash
# Encontrar todos los endpoints
rg "@app.route\|@app.get" --type py

# Encontrar uso de función
rg "sendEmail" --type ts

# Encontrar TODOs
rg "TODO|FIXME"
```

### 3. Git Blame/History

```bash
# Ver quién escribió línea X
git blame archivo.py

# Ver commits recientes de archivo
git log --oneline archivo.py

# Ver cambios en función específica
git log -L :functionName:archivo.py
```

### 4. IDE Features

- "Find all references"
- "Go to definition"
- "Show call hierarchy"

---

## 📊 Metrics de Onboarding

| Métrica | Target | Cómo medir |
| :-------- | :------- | :----------- |
| **Time to first commit** | < 3 días | Git history |
| **Time to first merged PR** | < 1 semana | GitHub/GitLab |
| **Time to independence** | < 4 semanas | Manager assessment |
| **Onboarding satisfaction** | > 4/5 | Survey post-onboarding |
| **Retention 90 días** | > 95% | HR metrics |

---

## 💬 Check-ins Regulares

### 1-on-1 con Manager

**Semana 1:**

- ¿Cómo se siente en el equipo?
- ¿Algo no quedó claro?
- ¿Se necesita acceso a algo?

**Semana 2:**

- ¿Estás bloqueado en algo?
- ¿El buddy está siendo útil?
- ¿Qué podemos mejorar del onboarding?

**Semana 4:**

- ¿Te sentís integrado al equipo?
- ¿Qué te gustaría aprender next?
- Feedback del onboarding para mejorarlo

---

## 🎉 Celebrar Hitos

| Hito | Celebración |
| :----- | :------------ |
| **Primera PR merged** | Shoutout en Slack, emoji reaction 🎉 |
| **Primera feature deployed** | Demo en team meeting |
| **Fin de onboarding (4-6 semanas)** | Team lunch, pequeño regalo (swag) |

**Cultura:** Todos celebran, no solo manager. Hace sentir bienvenido.

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **"Figure it out yourself"** | Frustración, lentitud | Buddy activo, docs claras |
| **Setup toma 3 días** | Mal primera impresión | Scripts automáticos, Docker |
| **Sin primera tarea clara** | No saber qué hacer | Backlog de "good first issues" |
| **Overwhelm con info** | Parálisis | Progressive disclosure, solo lo necesario |
| **Sin feedback** | No saber si va bien | Check-ins semanales |
| **Buddy ausente** | Nuevo dev abandondado | Backup buddy, manager interviene |

---

## 📋 Onboarding Checklist (Manager/TL)

### Antes del día 1

- [ ] Laptop y accesos configurados
- [ ] Buddy asignado (y aceptó)
- [ ] Primera tarea preparada
- [ ] Team notificado de llegada
- [ ] Calendario con meetings key
- [ ] Slack channels relevantes identificados

### Día 1

- [ ] Welcome message en Slack
- [ ] Intro con equipo completo
- [ ] Tour de oficina/intro a herramientas
- [ ] Setup completado exitosamente

### Semana 1

- [ ] 1-on-1 inicial (30 min)
- [ ] Primera PR revisada por buddy
- [ ] Invitado a todos los meetings de equipo

### Mes 1

- [ ] Check-in semanal
- [ ] Primera feature merged
- [ ] Participando activamente en discusiones
- [ ] Survey de feedback de onboarding

---

## 📝 Template: Feedback Survey

**Enviar después de 4-6 semanas:**

```text
Onboarding Feedback

1. En escala 1-5, ¿qué tan preparado te sentiste el día 1?
2. ¿Qué parte del onboarding fue más útil?
3. ¿Qué mejorarías del proceso?
4. ¿Tu buddy fue efectivo? ¿Por qué sí/no?
5. ¿Cuánto tiempo te tomó sentirte productivo?
6. ¿Algo faltó que hubieras necesitado?
7. Comentarios adicionales
```

**Usar feedback para iterar proceso.**

---

## 🌟 Onboarding Remoto

**Desafíos extra:**

- Menos interacciones casuales
- Difícil leer ambiente/cultura
- Setup puede ser más complejo

**Soluciones:**

| Desafío | Solución |
| :-------- | :--------- |
| **Aislamiento** | Daily video check-in, coffee chats virtuales |
| **Setup** | Video call screen-share para troubleshoot |
| **Cultura** | Enviar care package, virtual team building |
| **Comunicación** | Sobre-comunicar, usar video no solo texto |
| **Onboarding doc** | Aún más crítico, debe ser exhaustivo |

---

## 📚 Recursos

- [GitLab's Remote Onboarding](https://about.gitlab.com/company/culture/all-remote/learning-and-development/#how-do-you-onboard-new-team-members)
- [The Missing README - Chapter on Onboarding](https://github.com/readme/guides/onboarding-engineers)
- [First 90 Days - Michael Watkins](https://www.amazon.com/First-Days-Strategies-Expanded/dp/1422188612)
