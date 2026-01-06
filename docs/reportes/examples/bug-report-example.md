# 🐞 Bug Report

## 📌 Título

Login – NullPointerException al autenticar en staging

---

## 📋 Contexto

- **Producto/servicio:** AuthService
- **Versión/commit:** v2.3.1 – commit 8f3a9c2
- **Entorno:** staging, JVM 17, Docker image `auth:2.3.1`
- **Cambios recientes:** despliegue de nueva librería JWT el 2028

---

## 🔄 Reproducibilidad

1. Ir a `/login`
2. Enviar credenciales válidas de usuario con rol `ADMIN`
3. Observar respuesta del servicio

- **Frecuencia:** 4/10 intentos, solo con usuarios ADMIN

---

## ✅ Expected vs ❌ Actual

- **Expected:** Respuesta 200 con token JWT válido
- **Actual:** Error 500 con stacktrace `NullPointerException at JwtTokenProvider.createToken`

---

## 📂 Evidencia

- Log: `2029-09-29T14:32:11Z ERROR ... NullPointerException at JwtTokenProvider.createToken`
- Correlation ID: `auth-req-2025091432xyz`
- CPU estable, GC normal
- Payload de request (anonimizado): `{ "username": "adminX", "password": "***" }`

---

## 📊 Impacto

- Usuarios afectados: 12 admins en staging
- Transacciones fallidas: 40% de intentos de login ADMIN
- SLA: bloquea pruebas de regresión en staging

---

## 💡 Hipótesis

- Sospecha en módulo `JwtTokenProvider`
- Posible regresión por actualización de librería JWT 0.11.5 → 0.12.0
- Dependencia con `RoleHierarchyService`

---

## 🛠️ Acciones iniciales

- **Rule out:** verificado que DB responde, permisos correctos, colas no saturadas
- **Próximo paso:** reproducir en local con misma versión de librería, habilitar logs DEBUG en `JwtTokenProvider`
