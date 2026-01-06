# Colaboración y Cultura de Desarrollo

> Prácticas y patrones para equipos de desarrollo efectivos, colaborativos y sostenibles.

---

## 🤝 Colaboración en Desarrollo

**Qué:** Trabajar juntos efectivamente para crear mejor software.

**Por qué:** Software complejo requiere múltiples perspectivas. Colaboración > suma individual.

**Quién:** Todo el equipo de desarrollo.

**Cuándo:** Diario, en todas las fases del desarrollo.

**Esfuerzo:** Inversión en comunicación y coordinación que multiplica productividad.

---

## 👥 Pair Programming

**Qué:** Dos desarrolladores trabajando juntos en la misma máquina/pantalla.

**Por qué:** Menos bugs, mejor diseño, conocimiento compartido, onboarding efectivo.

**Cuándo:** Código complejo, onboarding, debugging crítico, aprendizaje.

**Roles:**

- **Driver:** Escribe código, foco en táctico
- **Navigator:** Piensa estratégico, revisa, sugiere

**Rotación:** Cada 30 minutos cambiar roles.

**Tipos:**

| Estilo | Qué | Cuándo |
| :------- | :----- | :----- |
| **Driver-Navigator** | Clásico: uno escribe, otro guía | General |
| **Ping Pong** | TDD: uno escribe test, otro implementa, alternan | TDD estricto |
| **Strong-Style** | Navigator dicta ideas, Driver implementa sin cuestionar | Enseñar patterns |

**Beneficios:**

- 15% más lento escribir código
- 15% menos bugs
- Mejor diseño
- Knowledge transfer

**Herramientas remotas:** [VS Code Live Share](https://visualstudio.microsoft.com/services/live-share/), [Tuple](https://tuple.app/), [Pop](https://pop.com/)

---

## 👨‍👩‍👧‍👦 Mob Programming

**Qué:** Todo el equipo (3-8 personas) trabajando en el mismo problema simultáneamente.

**Por qué:** Decisiones colaborativas, problemas complejos, alineación total.

**Cuándo:** Arquitectura crítica, decisiones importantes, spikes, workshops.

**Formato:**

- **Typist/Driver:** Escribe sin decidir (15 min rotación)
- **Mob:** Discute y decide qué escribir
- **Facilitador:** Mantiene proceso, timekeeper

**Proceso:**

1. Problema claramente definido
2. Rotación cada 15 min con timer
3. Commits frecuentes (WIP OK)
4. Breaks cada 90 min

**Cuándo NO usar:**

- Tareas simples, repetitivas
- Features independientes
- Solo para llenar tiempo

**Ventajas:**

- Decisiones de calidad inmediata
- Cero handoffs
- Todo el equipo con contexto completo

---

## 🔍 Code Review

**Qué:** Revisión sistemática de código por pares antes de merge.

**Por qué:** Detectar bugs, mejorar diseño, compartir conocimiento, mantener estándares.

**Cuándo:** Todo código antes de merge a main (sin excepciones).

### Best Practices - Autor

| Práctica | Por qué | Cómo |
| :--------- | :---- | :----- |
| **PRs pequeños** | Más rápido revisar, menos errores | <400 líneas, 1 concepto |
| **Descripción clara** | Contexto para reviewer | Qué, por qué, cómo testear |
| **Self-review primero** | Catch obvious issues | Revisar diff antes de pedir review |
| **Tests incluidos** | Validación | Tests pasan, coverage OK |
| **Screenshots/GIFs** | Para cambios UI | Mostrar antes/después |

### Best Practices - Reviewer

| Práctica | Por qué | Cómo |
| :--------- | :---- | :----- |
| **Revisar pronto** | No bloquear | <24 horas, idealmente <4h |
| **Ser constructivo** | Ambiente de confianza | "¿Consideraste X?" vs "Esto está mal" |
| **Explicar el por qué** | Educativo | "Esto podría causar N+1 queries" |
| **Distinguir niveles** | Priorización | "Nit:" menor, "Blocker:" crítico |
| **Aprobar con comentarios** | No re-review trivial | Si cambios menores, aprobar + sugerencias |

### Checklist Reviewer

- [ ] Código cumple requisitos
- [ ] Lógica correcta, sin bugs obvios
- [ ] Tests adecuados, pasando
- [ ] Sin código comentado/debug
- [ ] Nombres claros y descriptivos
- [ ] Sin complejidad innecesaria
- [ ] Performance aceptable
- [ ] Seguridad considerada
- [ ] Documentación actualizada

**Herramientas:** [GitHub PR](https://docs.github.com/en/pull-requests), [GitLab MR](https://docs.gitlab.com/ee/user/project/merge_requests/), [Gerrit](https://www.gerritcodereview.com/)

---

## 📋 Blameless Post-Mortems

**Qué:** Análisis retrospectivo de incident sin culpar individuos.

**Por qué:** Aprender de fallos, mejorar sistemas, cultura psicológicamente segura.

**Cuándo:** Después de cada outage/incident significativo.

**Estructura:**

### 1. Timeline de Eventos

```text
14:23 - Deploy v2.3.4 a producción
14:31 - Alertas de error rate 15%
14:35 - Equipo notificado vía PagerDuty
14:42 - Identificado problema en migration
14:50 - Rollback iniciado
15:05 - Servicio restaurado
```

### 2. Impacto

- Usuarios afectados: 5,000
- Duración: 42 minutos
- Revenue perdido: $2,500
- Reputación: 23 quejas en redes

### 3. Causa Raíz (5 Porqués)

1. Migration falló
2. Script no validó datos existentes
3. No había test para ese edge case
4. No testeamos en staging con datos prod-like
5. **Causa raíz:** Proceso de migrations sin validación obligatoria

### 4. Qué funcionó bien

- Monitoring detectó rápido (8 min)
- Comunicación efectiva en Slack
- Rollback funcionó correctamente

### 5. Action Items

| Acción | Owner | Deadline | Status |
| :------- | :------ | :--------- | :------- |
| Agregar validación migrations | @alice | 2015 | Done ✅ |
| Actualizar runbook rollback | @bob | 2010 | Done ✅ |
| Setup staging con prod data anonymized | @carol | 2020 | In Progress |

**Principio clave:** "We blame systems, not people." Foco en cómo prevenir, no quién causó.

---

## 🔄 Retrospectivas

**Qué:** Reunión regular para reflexionar sobre proceso y mejorar.

**Por qué:** Mejora continua, equipo más feliz y productivo.

**Cuándo:** Cada sprint (2 semanas), después de milestones.

**Formato:**

### 1. Set the Stage (5 min)

- Check-in: "Una palabra que describe tu sprint"
- Recordar Prime Directive: "Todos hicieron lo mejor con lo que sabían"

### 2. Gather Data (15 min)

#### Técnica: Start-Stop-Continue

| Start | Stop | Continue |
| :------ | :----- | :--------- |
| Daily standups async | Meetings de 2h | Pair programming viernes |
| Code reviews en <4h | Interrupciones constantes | Pizza Fridays 🍕 |

#### Alternativa: Mad-Sad-Glad

- 😡 Mad: Frustraciones
- 😢 Sad: Decepciones
- 😊 Glad: Celebraciones

### 3. Generate Insights (15 min)

- Agrupar temas similares
- Votar (3 votos por persona)
- Identificar patrones

### 4. Decide What To Do (15 min)

- Elegir 1-3 acciones concretas
- Asignar owner
- Definir "done"

### 5. Close (5 min)

- Resumen de acciones
- Appreciation shoutouts

**Facilitación:**

- Rotar facilitador
- Timeboxear estricto
- Ambiente seguro (no managers si coarta)
- Seguir actions del retro anterior

---

## 🎯 Working Agreements

**Qué:** Acuerdos explícitos de cómo trabaja el equipo.

**Por qué:** Expectativas claras, menos fricción, autonomía.

**Cuándo:** Al formar equipo, revisar cada 6 meses.

**Ejemplos:**

### Comunicación

- Slack: responder en <4h horas hábiles
- Urgent: llamar directamente
- Updates asíncronos en daily doc, no meetings
- No mensajes fuera 9-18h salvo emergencia

### Code

- Main branch siempre deployable
- PR aprobado en <24h
- Coverage >80% en features nuevas
- Breaking changes requieren RFC

### Meetings

- No meetings antes 10am
- Máximo 2h focus time protegido (9-11am)
- Todas las reuniones con agenda
- Default 25/50 min (no 30/60)

### On-call

- Rotación semanal
- Handoff Lunes 10am
- Runbooks actualizados
- Post-mortem dentro 48h de incident

---

## 🗣️ Comunicación Efectiva

### Async-First

**Por qué:** Respeta tiempo, permite deep work, inclusivo para time zones.

**Cómo:**

- Documentar decisiones por escrito
- Updates en Slack/Notion vs meetings
- Grabar meetings para ausentes
- Threads organizados por tema

### RFC (Request for Comments)

**Qué:** Documento para proponer cambios técnicos significativos.

**Estructura:**

1. **Summary:** One-liner
2. **Motivation:** Por qué necesario
3. **Proposal:** Solución propuesta
4. **Alternatives:** Otras opciones consideradas
5. **Impact:** Qué equipos/sistemas afecta
6. **Timeline:** Cuándo implementar

**Proceso:**

- Autor publica RFC
- Team comenta (1 semana)
- Discusión en RFC review meeting
- Decisión documentada

---

## 🎓 Knowledge Sharing

| Práctica | Qué | Cuándo |
| :--------- | :----- | :----- |
| **Tech Talks** | Presentaciones internas | Viernes 1h mensual |
| **Brown Bags** | Lunch & learn informales | Ad-hoc |
| **Wiki/Docs** | Documentación centralizada | Continuo |
| **Pair/Mob** | Learning by doing | Semanal |
| **Book Club** | Leer y discutir libro tech | Trimestral |

---

## 📝 Cultura de Contribución

 **Principio:** "La documentación es responsabilidad de todos, no solo de un equipo de docs."

### Cómo Contribuir

 1. **Pequeñas mejoras:** (typos, links rotos) → Editar directamente en GitHub/GitLab y abrir PR.
 2. **Dudas o falta de claridad:** → Abrir Issue con etiqueta `docs`. Si tuviste que preguntar en Slack, falta en la doc.
 3. **Nueva Feature:** → La PR de código **debe** incluir cambios en doc (README, API docs).

 **Docs as Code:**

 Tratar la documentación como código:

- Versionada en Git
- Revisada en PRs (Code Review)
- Validada por CI (Linters, link checkers)

 > [!TIP]
 > **Regla de Boy Scout aplicada a Docs:** Si lees una doc y encuentras un error, arréglalo o repórtalo. No lo ignores.

 ---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Hero culture** | Depender de 1 persona | Knowledge sharing, documentation |
| **Blame culture** | Miedo a admitir errores | Blameless post-mortems |
| **Not invented here** | Rechazar soluciones externas | Pragmatismo, evaluar objetivamente |
| **Meetings innecesarios** | Pérdida de tiempo | Async-first, agenda obligatoria |
| **Silos de conocimiento** | Info atrapada en individuos | Pair programming, docs, rotación |

---

## 📚 Recursos

- [The Pragmatic Programmer - Hunt & Thomas](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)
- [Team Topologies - Skelton & Pais](https://teamtopologies.com/)
- [Extreme Programming Explained - Kent Beck](https://www.amazon.com/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)
- [Google's Project Aristotle](https://rework.withgoogle.com/print/guides/5721312655835136/)
