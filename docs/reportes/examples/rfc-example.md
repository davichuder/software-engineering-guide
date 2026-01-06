# 📝 RFC: Implementación de Redis para Caching de Sesiones

## ℹ️ Meta Información

- **RFC ID:** RFC-042
- **Título:** Redis Session Caching
- **Autor:** David Developer
- **Estado:** Review
- **Fecha:** 2028

---

## 📋 Contexto y Alcance

Actualmente, las sesiones de usuario se guardan en la base de datos PostgreSQL principal (`users_sessions`). Con el crecimiento de usuarios, la tabla de sesiones recibe el 60% de las escrituras totales, causando bloqueos y latencia en otras operaciones críticas.
El objetivo es mover el almacenamiento de sesiones a una solución en memoria (Redis) para aliviar la DB y mejorar la latencia de autenticación.

---

## 🏗️ Diseño Propuesto

Utilizar Redis Cluster como store de sesiones.

**Arquitectura:**

- Auth Service escribirá/leerá de Redis en lugar de Postgres.
- TTL (Time To Live) de 24 horas en las keys de Redis.

**Modelo de Datos (Redis Key-Value):**

- Key: `session:{session_id}`
- Value: JSON `{ user_id: 123, roles: ["admin"], created_at: ... }`

**Cambios en API:**

- Transparentes para el frontend (sigue enviando cookie/token).

---

## 🔄 Alternativas Consideradas

1. **Memcached:**
   - Pros: Más simple, multithreaded.
   - Cons: No persistencia (si reinicia, todos deslogueados), estructuras de datos limitadas.
   - Decisión: Descartado por falta de persistencia opcional y features avanzadas.

2. **DynamoDB:**
   - Pros: Serverless, escalable.
   - Cons: Costo más alto por request, latencia mayor que in-memory.
   - Decisión: Descartado por latencia.

---

## ⚠️ Riesgos y Desafíos

- **Persistencia:** Si Redis cae y no hay persistencia AOF, los usuarios perderán sesión. (Mitigación: Habilitar AOF fsync every second).
- **Consistencia:** No habrá consistencia fuerte inmediata si usamos réplicas de lectura (Eventual consistency es aceptable para sesiones).

---

## ✅ Plan de Implementación

1. **Fase 1:** Levantar Redis Cluster en infraestructura.
2. **Fase 2:** Auth Service escribe en AMBOS (Postgres + Redis), lee de Postgres (Shadow write).
3. **Fase 3:** Switch de lectura a Redis.
4. **Fase 4:** Dejar de escribir en Postgres.

---

## ❓ Preguntas Abiertas

- ¿Necesitamos encriptar los datos en reposo en Redis? (Probablemente sí por compliance).
