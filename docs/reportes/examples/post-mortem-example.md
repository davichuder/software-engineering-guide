# 💀 Post-Mortem Report

## ℹ️ Meta Información

- **Incidente:** Caída de Checkout en Black Friday
- **Fecha:** 2024
- **Estado:** Final
- **Autores:** Juan Perez, Maria Garcia
- **Severidad:** SEV-1

---

## 📝 Resumen Ejecutivo

Durante el pico de tráfico de Black Friday, el servicio de Checkout comenzó a responder con errores 503 debido a agotamiento de conexiones a la base de datos. El incidente duró 15 minutos, afectando al 100% de los intentos de compra. Se resolvió aumentando el pool de conexiones y reiniciando los pods.

---

## 📊 Impacto

- **Duración:** 15 minutos (14:00 - 14:15 UTC)
- **Usuarios afectados:** ~5,000 intentos de compra fallidos
- **Pérdida estimada:** ~$150,000 USD
- **SLA Breached:** Sí

---

## 🕒 Timeline

Todas las horas en UTC

- **[14:00]** - Alerta de "High Error Rate" en Checkout Service dispara en PagerDuty.
- **[14:02]** - On-call (Juan) reconoce alerta y entra al war room.
- **[14:05]** - Logs muestran `ConnectionPoolTimeoutException`.
- **[14:07]** - DB Metrics muestran 100% conexiones activas.
- **[14:09]** - Decisión: Escalar pool size en config y reiniciar.
- **[14:12]** - Deploy de config map actualizado.
- **[14:15]** - Servicio recuperado, error rate baja a 0%.

---

## 🔍 Causa Raíz (5 Whys)

1. **¿Por qué falló el checkout?**
   La base de datos rechazó nuevas conexiones.
2. **¿Por qué rechazó conexiones?**
   Se alcanzó el límite máximo configurado (max_connections=100).
3. **¿Por qué se alcanzó el límite?**
   El tráfico fue 5x lo normal y cada request abría una conexión nueva.
4. **¿Por qué cada request abría conexión nueva?**
   El servicio no estaba reusando conexiones eficientemente (connection leak en un endpoint legado).
5. **¿Por qué no se detectó en pruebas de carga?**
   Las pruebas de carga usaron el flujo nuevo, no el endpoint legado que recibió tráfico inesperado.

---

## 🛠️ Resolución y Recuperación

Se aumentó temporalmente el límite de conexiones de la DB y del pool de la aplicación. Se identificó el endpoint problemático y se deshabilitó temporalmente hasta fixearlo.

---

## 🎓 Lecciones Aprendidas

**Lo que salió bien:**

- El equipo reaccionó en < 2 minutos.
- El dashboard de métricas de DB fue claro.

**Lo que salió mal:**

- No teníamos un "Kill Switch" para el endpoint legado.
- Las pruebas de carga no cubrieron escenarios legacy.

---

## ✅ Action Items

| Tarea | Tipo | Dueño | Prioridad | Ticket |
|:------|:-----|:------|:----------|:-------|
| Fix connection leak en LegacyCheckout | Reparación | @backend | Crítica | JIRA-501 |
| Incluir flujos legacy en Load Tests | Preventivo | @qa | Alta | JIRA-502 |
| Implementar PgBouncer para pooling | Arquitectura | @sre | Media | JIRA-503 |
