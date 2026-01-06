# APIs y Protocolos

> Interfaces para comunicación entre sistemas: REST, GraphQL, gRPC, WebSockets y más.

---

## 🎯 Elegir Protocolo

**Qué:** Definir cómo sistemas se comunican.

**Por qué:** Impacta performance, developer experience, escalabilidad.

**Quién:** Arquitectos, backend developers.

**Esfuerzo:** Decisión difícil de cambiar una vez adoptada.

---

## 🌐 REST (Representational State Transfer)

**Qué:** APIs basadas en HTTP con recursos y verbos.

**Por qué:** Estándar universal, cacheable, stateless.

| Aspecto | Qué | Cómo |
| :-------- | :----- | :---- |
| **Recursos** | Sustantivos (entidades) | `/users`, `/orders`, `/products` |
| **Verbos HTTP** | Acciones sobre recursos | GET (leer), POST (crear), PUT (actualizar), DELETE (borrar) |
| **Códigos Status** | Resultado de operación | 200 OK, 201 Created, 400 Bad Request, 404 Not Found, 500 Server Error |
| **Idempotencia** | Mismo request → mismo resultado | GET, PUT, DELETE idempotentes. POST no |
| **HATEOAS** | Hypermedia as Engine of State | Responses incluyen links a recursos relacionados |

**Ejemplo:**

```http
GET /api/v1/users/123
Authorization: Bearer <token>

Response 200:
{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com",
  "_links": {
    "self": "/users/123",
    "orders": "/users/123/orders"
  }
}
```

**Ventajas:**

- Simplicidad y curva de aprendizaje baja.  
- Amplio soporte en browsers, frameworks y herramientas.  
- Caching nativo vía HTTP.  
- Idempotencia clara en GET/PUT/DELETE.  
- Estándar universal, interoperable.

**Desventajas:**

- Overfetching/underfetching: el cliente recibe más o menos datos de los necesarios.  
- Multiplicidad de endpoints → mantenimiento más complejo.  
- Falta de tipado fuerte (JSON libre).  
- HATEOAS poco usado en la práctica.  

---

## 🔍 GraphQL

**Qué:** Query language para APIs con schema tipado.

**Por qué:** Cliente pide exactamente lo que necesita, no más ni menos.

| Concepto | Qué | Ejemplo |
| :--------- | :----- | :-------- |
| **Schema** | Tipos y relaciones | `type User { id: ID! name: String! posts: [Post] }` |
| **Query** | Leer datos | `{ user(id: "123") { name email } }` |
| **Mutation** | Modificar datos | `mutation { createUser(input: {...}) { id } }` |
| **Subscription** | Real-time updates | `subscription { messageAdded { id text } }` |
| **Resolver** | Función que obtiene datos | `User.posts: (parent) => fetchPosts(parent.id)` |

**Ventajas:**

- Evita overfetching/underfetching: cliente pide exactamente lo que necesita.  
- Un único endpoint centralizado.  
- Schema tipado y autodocumentado.  
- Soporta queries, mutations y subscriptions (real‑time).  
- Ecosistema rico (Apollo, Hasura).

**Desventajas:**

- Curva de aprendizaje mayor, requiere definir schema y resolvers.  
- Caching complejo (usa POST y un solo endpoint).  
- Riesgo de queries costosas (N+1).  
- Seguridad: requiere control de profundidad y complejidad de queries.

**Herramientas:** [Apollo](https://www.apollographql.com/), [Hasura](https://hasura.io/), [GraphQL Yoga](https://the-guild.dev/graphql/yoga-server)

---

## ⚡ gRPC

**Qué:** RPC framework con Protocol Buffers binario.

**Por qué:** Performance, tipado fuerte, streaming nativo.

| Aspecto | Qué | Ventaja |
| :-------- | :----- | :-------- |
| **Protocol Buffers** | Serialización binaria | Más pequeño y rápido que JSON |
| **HTTP/2** | Multiplexing, server push | Menos conexiones |
| **Streaming** | Bidireccional | Server/Client/Bidirectional streams |
| **Code generation** | Clientes/servers automáticos | Type-safe |

**Ejemplo .proto:**

```protobuf
service UserService {
  rpc GetUser (GetUserRequest) returns (User);
  rpc StreamUsers (StreamUsersRequest) returns (stream User);
}

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
}
```

**Ventajas:**

- Serialización binaria (Protocol Buffers) → payloads pequeños y rápidos.  
- HTTP/2: multiplexing, server push, menos conexiones.  
- Tipado fuerte y generación automática de clientes/servers.  
- Streaming bidireccional nativo.  
- Ideal para microservicios internos de alta performance.

**Desventajas:**

- Soporte limitado en browsers (requiere proxy/gateway).  
- Curva de aprendizaje alta (proto files, tooling).  
- Debugging más complejo que JSON/REST.  
- No aprovecha caching HTTP estándar.  

**Cuándo usar:** Microservicios internos, alta performance, streaming.

**Herramientas:** [gRPC](https://grpc.io/), [Buf](https://buf.build/), [grpcurl](https://github.com/fullstorydev/grpcurl)

---

## 🔌 WebSockets

**Qué:** Conexión bidireccional persistente sobre TCP.

**Por qué:** Real-time, baja latencia, push de servidor.

| Aspecto | Qué | Cuándo |
| :-------- | :----- | :------- |
| **Full-duplex** | Cliente y servidor envían simultáneamente | Chat, gaming |
| **Persistent** | Conexión abierta continuamente | Notificaciones en tiempo real |
| **Binary/Text** | Soporta ambos formatos | Flexible |

**Ejemplo:**

```javascript
const ws = new WebSocket('wss://api.example.com/ws');

ws.onmessage = (event) => {
  console.log('Message:', event.data);
};

ws.send(JSON.stringify({ type: 'subscribe', channel: 'updates' }));
```

**Ventajas:**

- Comunicación full‑duplex en tiempo real.  
- Baja latencia, ideal para chat, gaming, dashboards.  
- Soporta texto y binario.  
- Amplio soporte en navegadores.

**Desventajas:**

- No cacheable.  
- Escalabilidad más compleja (conexiones persistentes).  
- Requiere load balancers y proxies compatibles.  
- Difícil de depurar comparado con HTTP.

**Cuándo usar:** Chat, gaming, dashboards en tiempo real, collaborative editing.

**Alternativa:** Server-Sent Events (SSE) para one-way server→client.

---

## 📨 Server-Sent Events (SSE)

**Qué:** Streaming unidireccional server→client sobre HTTP.

**Por qué:** Más simple que WebSockets para notificaciones.

```javascript
const eventSource = new EventSource('/api/events');

eventSource.addEventListener('update', (event) => {
  console.log('Update:', event.data);
});
```

**Cuándo usar:** Notificaciones, cotizaciones, progress updates.

**Ventajas:**

- Simples: usan HTTP estándar.
- Reconexión automática integrada.  
- Event IDs permiten reanudar streams.  
- Menor complejidad que WebSockets para notificaciones unidireccionales.

**Desventajas:**

- Solo unidireccional (server → client).  
- Basado en texto (no binario).  
- Menor soporte en algunos entornos comparado con WebSockets.  
- No apto para escenarios de alta concurrencia bidireccional.

---

## 🎯 Event-Driven / Async

**Qué:** Comunicación basada en eventos via message brokers.

**Por qué:** Desacopla productores y consumidores, escalable horizontalmente.

**Cuándo usar:** Workflows complejos, alto throughput, logs.

| Broker | Qué | Cuándo |
| :------- | :----- | :----- |
| [RabbitMQ](https://www.rabbitmq.com/) | Message queue con routing | Workflows complejos |
| [Apache Kafka](https://kafka.apache.org/) | Event streaming distribuido | Alto throughput, logs |
| [AWS SQS](https://aws.amazon.com/sqs/) | Queue serverless | Desacople simple en AWS |
| [Redis Pub/Sub](https://redis.io/docs/manual/pubsub/) | Publish/Subscribe en memoria | Eventos en tiempo real |

**Patrones:**

- **Pub/Sub:** Broadcast a múltiples consumidores
- **Queues:** Work distribution, un consumidor procesa
- **Event Sourcing:** Eventos como fuente de verdad

**Ventajas:**

- Desacopla productores y consumidores.  
- Escalable horizontalmente.  
- Patrones flexibles: Pub/Sub, colas, event sourcing.  
- Alta tolerancia a fallos (Kafka, SQS).  
- Ideal para sistemas distribuidos.

**Desventajas:**

- Complejidad operativa (brokers, clusters).  
- Latencia mayor que RPC directo.  
- Requiere monitoreo y observabilidad avanzada.  
- Curva de aprendizaje de cada broker.  

---

## 🔄 Webhooks

**Qué:** HTTP callbacks cuando ocurre evento.

**Por qué:** Integración event-driven sin polling.

**Ejemplo:**

```json
POST https://yourapp.com/webhook
X-Signature: sha256=...

{
  "event": "payment.succeeded",
  "data": {
    "amount": 1000,
    "currency": "USD"
  }
}
```

**Seguridad:**

- Validar firma HMAC
- HTTPS obligatorio
- Retry exponential backoff
- Idempotencia en receptor

**Ventajas:**

- Simples: callbacks HTTP estándar.  
- Integración rápida entre sistemas.  
- Evitan polling.  
- Amplio soporte en SaaS y APIs públicas.

**Desventajas:**

- Seguridad: requieren validación de firmas y HTTPS.  
- Fiabilidad: necesitan reintentos e idempotencia.  
- Escalabilidad limitada (cada evento → request).  
- Difícil de depurar si el receptor falla.

---

## 📄 Documentación

| Protocolo | Herramienta | Ejemplo |
| :---------- | :------------ | :-------- |
| **REST** | [OpenAPI](https://www.openapis.org/) / [Swagger](https://swagger.io/) | Spec YAML → Swagger UI |
| **GraphQL** | Schema introspection | [GraphQL Playground](https://github.com/graphql/graphql-playground) |
| **gRPC** | [Protocol Buffers](https://protobuf.dev/) | `.proto` files → docs |
| **WebSockets** | [AsyncAPI](https://www.asyncapi.com/) | Spec para async APIs |
| **Webhooks** | [AsyncAPI](https://www.asyncapi.com/) / [OpenAPI](https://www.openapis.org/) | Spec para webhooks |

**Ejemplo OpenAPI:**

```yaml
openapi: 3.0.0
paths:
  /users/{id}:
    get:
      summary: Get user by ID
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: User found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
```

---

## 🔐 Autenticación

| Método | Qué | Cuándo | Header |
| :------- | :----- | :----- | :------- |
| **API Key** | String estático | Integraciones internas | `X-API-Key: abc123` |
| **Bearer Token** | JWT en header | SPAs, mobile apps | `Authorization: Bearer <jwt>` |
| **OAuth 2.0** | Delegación con tokens | Third-party integrations | `Authorization: Bearer <access_token>` |
| **Basic Auth** | Usuario:contraseña en base64 | Desarrollo, admin tools | `Authorization: Basic <base64>` |
| **mTLS** | Certificados cliente/servidor | Service-to-service | TLS handshake |

---

## 🎨 Diseño de APIs

| Principio | Qué | Ejemplo |
| :---------- | :----- | :-------- |
| **Versionado** | Mantener compatibilidad | `/api/v1/`, `/api/v2/` o header |
| **Paginación** | Evitar payloads gigantes | `?limit=20&offset=40` o cursor |
| **Filtrado** | Permitir búsquedas | `?status=active&role=admin` |
| **Ordenamiento** | Control de orden | `?sort=created_at:desc` |
| **Rate Limiting** | Prevenir abuse | `X-RateLimit-Remaining: 45` |
| **CORS** | Controlar origins | `Access-Control-Allow-Origin: *` |
| **Idempotency Keys** | Evitar duplicados | `Idempotency-Key: uuid` header |

## 📊 Comparación

| Característica | REST | GraphQL | gRPC | WebSocket | Webhook |
| :--------------- | :----- | :-------- | :----- | :---------- | :-------- |
| **Protocol** | HTTP | HTTP | HTTP/2 | TCP | HTTP |
| **Payload** | JSON | JSON | Protobuf | Binary/Text | JSON |
| **Typing** | No | Sí (schema) | Sí (protobuf) | No | No |
| **Streaming** | No | Sí (subscriptions) | Sí | Sí | No |
| **Caching** | Fácil (HTTP) | Difícil | No | No | No |
| **Browser Support** | ✅ | ✅ | Limitado | ✅ | ✅ |
| **Curva de Aprendizaje** | Bajo | Medio | Alto | Medio | Medio |

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Verbos en URLs** | `/getUser`, `/createOrder` | Usar HTTP verbs: GET, POST |
| **Sin versionado** | Breaking changes rompen clientes | `/v1/`, deprecation notices |
| **Respuestas inconsistentes** | Diferentes estructuras por endpoint | Estandarizar formato |
| **Sin rate limiting** | Abuse, DDoS | Implementar límites |
| **Exponer IDs internos** | Security risk (Enumeration attacks) | Usar UUIDs públicos |

---

## 📚 Recursos

- [REST API Tutorial](https://restfulapi.net/)
- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
- [gRPC Documentation](https://grpc.io/docs/)
- [WebSocket Protocol RFC](https://datatracker.ietf.org/doc/html/rfc6455)
