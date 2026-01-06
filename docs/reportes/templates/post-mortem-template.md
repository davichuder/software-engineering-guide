# 💀 Post-Mortem Report

## ℹ️ Meta Información

- **Incidente:** [Título del incidente]
- **Fecha:** [YYYY-MM-DD]
- **Estado:** [Draft / Review / Final]
- **Autores:** [Nombres]
- **Severidad:** [SEV-1 / SEV-2 / SEV-3]

---

## 📝 Resumen Ejecutivo

[Resumen de 1 párrafo: qué pasó, impacto, y cómo se resolvió. Para lectura de ejecutivos.]

---

## 📊 Impacto

- **Duración:** [Tiempo total de caída/degradación]
- **Usuarios afectados:** [% o número absoluto]
- **Pérdida estimada:** [Si aplica]
- **SLA Breached:** [Sí/No]

---

## 🕒 Cronología (Timeline)

Todas las horas en UTC

- **[HH:MM]** - Inicio del incidente (alerta disparada o reporte de usuario).
- **[HH:MM]** - Ingeniero on-call recibe alerta.
- **[HH:MM]** - Se identifica la causa raíz preliminar.
- **[HH:MM]** - Se aplica fix temporal (mitigación).
- **[HH:MM]** - Servicio restaurado.
- **[HH:MM]** - Se aplica fix permanente.

---

## 🔍 Causa Raíz (5 Porqués)

1. **¿Por qué falló el sistema?**
   [Respuesta]
2. **¿Por qué ocurrió eso?**
   [Respuesta]
3. **¿Por qué...?**
   [Respuesta]
4. **¿Por qué...?**
   [Respuesta]
5. **¿Por qué...?**
   [Causa raíz fundamental: proceso, falta de test, etc.]

---

## 🛠️ Resolución y Recuperación

[Qué se hizo para mitigar y resolver. Qué funcionó y qué no.]

---

## 🎓 Lecciones Aprendidas

**Lo que salió bien:**

- [ ] Alertas funcionaron rápido.
- [ ] Rollback fue exitoso.

**Lo que salió mal:**

- [ ] Logs insuficientes para debuggear rápido.
- [ ] Nadie tenía acceso a la DB de producción.

**Donde tuvimos suerte:**

- [ ] Ocurrió en horario de bajo tráfico.

---

## ✅ Acciones (Action Items)

| Tarea | Tipo | Dueño | Prioridad | Ticket |
| :------ | :----- | :------ | :---------- | :------- |
| Agregar alerta de latencia | Preventivo | @dev | Alta | JIRA-101 |
| Mejorar documentación de runbook | Proceso | @sre | Media | JIRA-102 |
| Fix bug de race condition | Reparación | @backend | Crítica | JIRA-103 |

---

[➡️ Ver Ejemplo](../examples/post-mortem-example.md)
