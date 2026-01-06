# Optimización de Performance

> Técnicas y estrategias para mejorar velocidad, throughput y eficiencia de sistemas.

---

## 🎯 Principios de Performance

**Qué:** Optimización basada en mediciones, no suposiciones.

**Por qué:** Performance impacta UX, conversión y costos. 100ms extra = -1% conversión (Amazon).

**Quién:** Developers, architects, SREs.

**Esfuerzo:** Medir primero (profiling), optimizar cuellos de botella, validar mejora.

---

## 🗄️ Base de Datos

| Técnica | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :-------- | :----- | :---- | :----- | :---- | :------------- |
| **Indexing** | Estructuras para búsqueda rápida | O(log n) vs O(n) | Columnas en WHERE, JOIN, ORDER BY | `CREATE INDEX idx_users_email ON users(email)` | [EXPLAIN ANALYZE](https://www.postgresql.org/docs/current/sql-explain.html) |
| **Query Optimization** | Mejorar queries lentas | Reducir scans completos | Queries > 100ms | Evitar SELECT *, usar covering indexes, LIMIT | [DataGrip](https://www.jetbrains.com/datagrip/), [pgAdmin](https://www.pgadmin.org/) |
| **Connection Pooling** | Reutilizar conexiones | Evitar overhead de TCP handshake | Siempre | Pool de 50 conexiones | [HikariCP](https://github.com/brettwooldridge/HikariCP), [pgBouncer](https://www.pgbouncer.org/) |
| **Read Replicas** | Réplicas para lecturas | Escalar horizontalmente reads | Read:Write ratio > 70:30 | Master escribe, replicas leen | [PostgreSQL Replication](https://www.postgresql.org/docs/current/warm-standby.html) |
| **Sharding** | Particionar horizontalmente | Superar límites de un servidor | > 1TB datos, > 10k writes/sec | Shard por user_id, region | [Vitess](https://vitess.io/), [Citus](https://www.citusdata.com/) |
| **Materialized Views** | Precalcular queries complejas | Leer agregaciones sin calcular | Reportes, dashboards | `CREATE MATERIALIZED VIEW`, refresh periódico | PostgreSQL, MySQL 8+ |
| **Denormalization** | Duplicar datos para evitar JOINs | Reducir complejidad de queries | Alta carga de lectura | Almacenar campos calculados | Trade-off: consistencia vs performance |

---

## 🚀 Backend

| Técnica | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :-------- | :----- | :---- | :----- | :---- | :------------- |
| **Caching** | Almacenar resultados para reutilizar | Evitar cómputo/DB repetidos | Datos que cambian poco | Cache-Aside, Write-Through, TTL | [Redis](https://redis.io/), [Memcached](https://memcached.org/) |
| **Async Processing** | Desacoplar operaciones lentas | No bloquear request | Emails, reports, ML inference | Job queues, event-driven | [Celery](https://docs.celeryq.dev/), [BullMQ](https://docs.bullmq.io/) |
| **Rate Limiting** | Limitar requests por cliente | Prevenir abuse, proteger recursos | APIs públicas | Token bucket, sliding window | [redis-cell](https://github.com/brandur/redis-cell), [express-rate-limit](https://github.com/express-rate-limit/express-rate-limit) |
| **Pagination** | Retornar datos en páginas | Evitar payloads gigantes | Listas largas | Limit/Offset o Cursor-based | Backend frameworks |
| **Compression** | Comprimir responses HTTP | Reducir bandwidth | Responses > 1KB | Gzip, Brotli | Nginx, middleware |
| **HTTP/2** | Multiplexing, server push | Reducir latencia | Siempre (HTTPS) | Habilitar en server | Nginx, Caddy |
| **Database Prepared Statements** | Cachear plan de ejecución | Reducir parsing overhead | Queries repetidas | `PREPARE`, `EXECUTE` | ORMs built-in |

---

## 💻 Frontend

| Técnica | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :-------- | :----- | :---- | :----- | :---- | :------------- |
| **Code Splitting** | Dividir bundle en chunks | Cargar solo lo necesario | SPAs grandes | Dynamic imports, route-based splitting | [Webpack](https://webpack.js.org/), [Vite](https://vitejs.dev/) |
| **Lazy Loading** | Cargar recursos al scrollear | Reducir initial load | Imágenes, componentes below fold | `loading="lazy"`, Intersection Observer | [react-lazyload](https://github.com/twobin/react-lazyload) |
| **Tree Shaking** | Eliminar código no usado | Reducir bundle size | Siempre | ES6 imports, `sideEffects: false` | Webpack, Rollup |
| **Minification** | Remover espacios, renombrar vars | Reducir tamaño | Producción | Terser, UglifyJS | Build tools |
| **CDN** | Servir assets desde edge | Baja latencia global | Assets estáticos | Subir a CDN, cache headers | [Cloudflare](https://www.cloudflare.com/), [CloudFront](https://aws.amazon.com/cloudfront/) |
| **Prefetch/Preload** | Cargar recursos anticipadamente | Percepción de velocidad | Recursos críticos next page | `<link rel="prefetch">`, `<link rel="preload">` | HTML hints |
| **Virtual Scrolling** | Renderizar solo elementos visibles | Listas con 1000s de items | Tablas grandes | Renderizar ventana visible + buffer | [react-window](https://github.com/bvaughn/react-window), [vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller) |
| **Image Optimization** | Comprimir, formatos modernos | Reducir tamaño sin perder calidad | Todas las imágenes | WebP, AVIF, responsive images | [Sharp](https://sharp.pixelplumbing.com/), [ImageOptim](https://imageoptim.com/) |
| **Service Workers** | Cachear assets offline | PWA, velocidad | Apps que necesitan offline | Cache API, precache assets | [Workbox](https://developer.chrome.com/docs/workbox/) |

---

## 🌐 Networking

| Técnica | Qué | Por qué | Cuándo | Cómo |
| :-------- | :----- | :---- | :----- | :---- |
| **HTTP Keep-Alive** | Reutilizar conexión TCP | Evitar handshakes | Siempre | `Connection: keep-alive` header |
| **DNS Prefetch** | Resolver DNS antes de click | Reducir latencia | Links externos | `<link rel="dns-prefetch" href="//example.com">` |
| **HTTP/3 (QUIC)** | UDP en vez de TCP | Reducir latency en redes lossy | Mobile, alta latencia | Habilitar en CDN/server |
| **gRPC** | RPC binario | Más eficiente que JSON | Microservicios internos | Protocol Buffers, HTTP/2 |

---

## 📊 Profiling y Diagnóstico

| Herramienta | Qué | Cuándo | Cómo leer |
| :------------ | :----- | :----- | :---------- |
| [Chrome DevTools](https://developer.chrome.com/docs/devtools/) | Performance tab, Network, Lighthouse | Frontend | Flamegraphs, waterfall charts |
| [py-spy](https://github.com/benfred/py-spy) | Profiler Python sin modificar código | Backend Python | Flamegraph de CPU time |
| [async-profiler](https://github.com/async-profiler/async-profiler) | Profiler Java low-overhead | Backend Java | Flamegraphs, allocation profiling |
| [clinic.js](https://clinicjs.org/) | Profiler Node.js | Backend Node.js | Doctor, bubbleprof, flame |
| [ab](https://httpd.apache.org/docs/2.4/programs/ab.html) | Benchmark HTTP simple | Load testing básico | `ab -n 1000 -c 10 URL` |
| [wrk](https://github.com/wg/wrk) | Benchmark HTTP avanzado | Load testing con scripts Lua | RPS, latency distribution |

---

## 🎯 Métricas Clave

| Métrica | Objetivo | Cómo medir |
| :-------- | :--------- | :----------- |
| **TTFB** (Time To First Byte) | < 200ms | Chrome DevTools Network |
| **FCP** (First Contentful Paint) | < 1.8s | Lighthouse, Web Vitals |
| **LCP** (Largest Contentful Paint) | < 2.5s | Core Web Vitals |
| **TTI** (Time To Interactive) | < 3.8s | Lighthouse |
| **CLS** (Cumulative Layout Shift) | < 0.1 | Web Vitals |
| **FID** (First Input Delay) | < 100ms | Real User Monitoring |
| **Throughput** | Requests/sec | Load testing tools |
| **p95 Latency** | < 500ms | APM, Prometheus |

---

## 💾 Caching Strategies

| Strategy | Qué | Cuándo | Ejemplo |
| :--------- | :----- | :----- | :-------- |
| **Cache-Aside** | App lee cache, si miss → DB → cache | Lectura intensiva | `getUser() → check Redis → query DB → set Redis` |
| **Write-Through** | Escribir en cache y DB simultáneamente | Consistencia fuerte | `updateUser() → write DB + write Redis` |
| **Write-Behind** | Escribir en cache, async a DB | Alta carga escritura | Logs, metrics (eventual consistency OK) |
| **Refresh-Ahead** | Refrescar cache antes de expirar | Evitar cache misses | Precarga de datos populares |

**TTL (Time To Live):**

- Datos estáticos: 24h+
- Datos frecuentes: 1-5min
- Datos en tiempo real: 30s

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **N+1 Queries** | Un query por item en lista | Eager loading, JOIN |
| **SELECT \*** | Traer columnas innecesarias | SELECT solo lo necesario |
| **Sin indexes** | Full table scans | Indexar columnas en WHERE |
| **Cachear todo** | Cache invalidation compleja | Cachear solo lo necesario |
| **Optimización prematura** | Complejidad sin beneficio | Medir, luego optimizar |
| **Sincronizar operaciones async** | Bloquear esperando | Usar async/await correctamente |

---

## 📚 Recursos

- [High Performance Browser Networking](https://hpbn.co/)
- [Web.dev Performance](https://web.dev/performance/)
- [Database Performance Tuning](https://use-the-index-luke.com/)
- [Systems Performance - Brendan Gregg](https://www.brendangregg.com/systems-performance-2nd-edition-book.html)
