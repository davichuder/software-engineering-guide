# Infraestructura y Arquitectura Cloud

> Patrones, servicios y estrategias para construir sistemas escalables, resilientes y cost-effective en la nube.

---

## ☁️ Cloud Computing

**Qué:** Infraestructura y servicios on-demand vía internet.

**Por qué:** Escalabilidad, pago por uso, global deployment, menor gestión operativa.

**Quién:** DevOps, Platform Engineers, Cloud Architects.

**Esfuerzo:** OpEx vs CapEx, facturación mensual variable.

> **Nota:** **OpEx** (Gastos Operativos) son costos recurrentes como la factura mensual de AWS. **CapEx** (Gastos de Capital) son inversiones iniciales grandes como comprar servidores físicos. Cloud mueve el gasto de CapEx a OpEx.
>
> **Conceptos Relacionados:** Para entender las estrategias de **Escalabilidad Vertical vs Horizontal** que son fundamentales en decisiones de infraestructura cloud, ver [Escalabilidad en Arquitectura](./arquitectura-patrones.md#escalabilidad-vertical-vs-horizontal).

---

## 🌍 Proveedores Cloud

| Proveedor | Qué | Cuándo | Fortalezas |
| :---------- | :----- | :----- | :----------- |
| [AWS](https://aws.amazon.com/) | Líder del mercado, mayor catálogo | Default para mayoría de casos | Madurez, features, ecosistema |
| [Azure](https://azure.microsoft.com/) | Cloud de Microsoft | Empresas Microsoft-centric | Integración .NET, AD, Office 365 |
| [GCP](https://cloud.google.com/) | Cloud de Google | ML/AI, analytics, Kubernetes | BigQuery, ML APIs, Kubernetes nativo |
| [DigitalOcean](https://www.digitalocean.com/) | Developer-friendly, simple | Startups, apps pequeñas | Simplicidad, precios claros |
| [Hetzner](https://www.hetzner.com/) | Europeo, económico | Apps en Europa, cost-conscious | Precio/performance ratio |

---

## 🏗️ Modelos de Servicio

| Modelo | Qué | Gestiona Provider | Gestiona Cliente | Use Case |
| :------- | :----- | :------------------ | :----------------- | :--------- |
| **IaaS** (Infrastructure) | VMs, networking, storage | Hardware, virtualización | OS, runtime, apps | Control total, lift-and-shift |
| **PaaS** (Platform) | Runtime, escalado | Infra + OS + runtime | Solo código | Apps web, APIs |
| **SaaS** (Software) | Aplicación completa | Todo | Solo usar | Gmail, Salesforce, Slack |
| **FaaS** (Functions) | Funciones serverless | Todo menos función | Solo código de función | Event-driven, APIs ligeras |
| **CaaS** (Containers) | Orquestación contenedores | Infra + Kubernetes | Contenedores, manifests | Microservicios |

---

## 🚀 Serverless

**Qué:** Ejecutar código sin gestionar servidores.

**Por qué:** Cero gestión infra, escalado automático, pago por uso real.

| Servicio | Qué | Cuándo | Pricing |
| :--------- | :----- | :----- | :-------- |
| [AWS Lambda](https://aws.amazon.com/lambda/) | Funciones event-driven | APIs, jobs, ETL | Por invocación + GB-segundo |
| [Google Cloud Functions](https://cloud.google.com/functions) | Funciones GCP | Similar Lambda | Por invocación |
| [Azure Functions](https://azure.microsoft.com/en-us/products/functions) | Funciones Azure | Ecosistema Microsoft | Por ejecución |
| [Cloudflare Workers](https://workers.cloudflare.com/) | Edge compute global | Latencia ultra-baja | Por request |
| [Vercel](https://vercel.com/) | Deploy frontend + serverless | Next.js, frontend | Por función + bandwidth |

**Limitaciones:**

- Cold starts (500ms)
- Timeout (típico 15min)
- Stateless
- Vendor lock-in

---

## 🐳 Contenedores

| Tecnología | Qué | Cuándo |
| :----------- | :----- | :----- |
| [Docker](https://www.docker.com/) | Empaquetar apps con deps | Todo desarrollo moderno |
| [Kubernetes](https://kubernetes.io/) | Orquestar contenedores | Prod con >5 servicios |
| [ECS](https://aws.amazon.com/ecs/) | Contenedores AWS-native | Ya en AWS, menos complejidad que K8s |
| [Cloud Run](https://cloud.google.com/run) | Contenedores serverless GCP | Simplicity + containers |
| [Nomad](https://www.nomadproject.io/) | Orquestador simple | Alternativa K8s más liviana |

---

## 🗄️ Storage

| Tipo | Servicio | Cuándo | Características |
| :----- | :--------- | :----- | :---------------- |
| **Object** | [S3](https://aws.amazon.com/s3/), [GCS](https://cloud.google.com/storage), [Azure Blob](https://azure.microsoft.com/en-us/products/storage/blobs) | Archivos, backups, assets | Infinito, económico, durable |
| **Block** | [EBS](https://aws.amazon.com/ebs/), [Persistent Disk](https://cloud.google.com/persistent-disk) | Discos para VMs/containers | High IOPS, attached a instancia |
| **File** | [EFS](https://aws.amazon.com/efs/), [Filestore](https://cloud.google.com/filestore) | Shared filesystem | NFS, múltiples instancias |
| **Archive** | [Glacier](https://aws.amazon.com/glacier/), [Coldline](https://cloud.google.com/storage/docs/storage-classes#coldline) | Archivos long-term | Ultra-barato, retrieval lento |

---

## 🌐 Networking

| Concepto | Qué | Servicios |
| :--------- | :----- | :---------- |
| **VPC** | Virtual Private Cloud | Aislar recursos, subnets públicas/privadas |
| **Load Balancer** | Distribuir tráfico | [ALB](https://aws.amazon.com/elasticloadbalancing/), [NLB](https://aws.amazon.com/elasticloadbalancing/network-load-balancer/), [Cloud Load Balancing](https://cloud.google.com/load-balancing) |
| **CDN** | Content Delivery Network | [CloudFront](https://aws.amazon.com/cloudfront/), [Cloud CDN](https://cloud.google.com/cdn), [Cloudflare](https://www.cloudflare.com/) |
| **DNS** | Domain Name System | [Route 53](https://aws.amazon.com/route53/), [Cloud DNS](https://cloud.google.com/dns) |
| **API Gateway** | Entry point APIs | [AWS API Gateway](https://aws.amazon.com/api-gateway/), [Apigee](https://cloud.google.com/apigee) |
| **Service Mesh** | Inter-service communication | [Istio](https://istio.io/), [Linkerd](https://linkerd.io/) |

---

## 🔐 Identity & Access

| Concepto | Qué | Servicios |
| :--------- | :----- | :---------- |
| **IAM** | Gestión permisos | [AWS IAM](https://aws.amazon.com/iam/), [GCP IAM](https://cloud.google.com/iam) |
| **SSO** | Single Sign-On | [AWS SSO](https://aws.amazon.com/single-sign-on/), [Azure AD](https://azure.microsoft.com/en-us/products/active-directory) |
| **Secrets** | Gestión credenciales | [Secrets Manager](https://aws.amazon.com/secrets-manager/), [Secret Manager GCP](https://cloud.google.com/secret-manager) |
| **KMS** | Key Management | [AWS KMS](https://aws.amazon.com/kms/), [Cloud KMS](https://cloud.google.com/kms) |

---

## 📊 Managed Services

### Databases

| Tipo | AWS | GCP | Azure |
| :----- | :---- | :---- | :------ |
| **SQL** | RDS (PostgreSQL, MySQL) | Cloud SQL | Azure SQL Database |
| **NoSQL** | DynamoDB | Firestore | Cosmos DB |
| **Cache** | ElastiCache (Redis) | Memorystore | Azure Cache for Redis |
| **Analytics** | Redshift | BigQuery | Synapse Analytics |
| **Graph** | Neptune | - | Cosmos DB (Gremlin) |

### Messaging

| Tipo | AWS | GCP | Azure |
| :----- | :---- | :---- | :------ |
| **Queue** | SQS | Pub/Sub | Service Bus |
| **Streaming** | Kinesis | Pub/Sub | Event Hubs |
| **Event Bus** | EventBridge | Eventarc | Event Grid |

---

## 🌍 Multi-Cloud & Hybrid

**Qué:** Usar múltiples proveedores o combinar on-prem + cloud.

**Por qué:** Evitar vendor lock-in, redundancia, mejores precios.

| Estrategia | Qué | Cuándo | Trade-offs |
| :----------- | :----- | :----- | :----------- |
| **Multi-Cloud** | AWS + GCP + Azure | Redundancia, pricing | ✅ Resiliencia; ❌ Complejidad operativa |
| **Hybrid** | On-prem + cloud | Compliance, legacy | ✅ Gradual migration; ❌ Gestión dual |
| **Cloud-Agnostic** | Herramientas neutrales | Flexibilidad futura | ✅ Portabilidad; ❌ No usar features específicas |

**Herramientas:** [Terraform](https://www.terraform.io/), [Pulumi](https://www.pulumi.com/), [Crossplane](https://www.crossplane.io/)

---

## 📍 Edge Computing

**Qué:** Procesamiento cerca del usuario (edge locations).

**Por qué:** Baja latencia, menos bandwidth.

| Servicio | Qué | Caso de Uso |
| :--------- | :----- | :--------- |
| [Cloudflare Workers](https://workers.cloudflare.com/) | JS en edge global | APIs ultra-rápidas |
| [AWS Lambda@Edge](https://aws.amazon.com/lambda/edge/) | Lambda en CloudFront | Personalización responses |
| [Fastly Compute@Edge](https://www.fastly.com/products/edge-compute) | WebAssembly en edge | Custom logic en CDN |

---

## 💰 Cost Optimization

| Técnica | Qué | Cómo | Ahorro |
| :-------- | :----- | :---- | :------- |
| **Right-sizing** | Ajustar tamaño instancias | Monitorear uso, reducir oversized | 40% |
| **Reserved Instances** | Compromiso 1-3 años | Comprar RIs para workloads estables | 70% |
| **Spot Instances** | Capacidad no usada | Workloads fault-tolerant | 90% |
| **Auto-scaling** | Escalar según demanda | Policies basadas en métricas | 50% |
| **S3 Lifecycle** | Mover a storage barato | Glacier para archivos viejos | 90% |
| **Tagging** | Identificar costos | Tags por proyecto/equipo | Visibilidad |

---

## 🏛️ Well-Architected Framework

### 5 Pilares (AWS)

| Pilar | Qué | Principios Clave |
| :------ | :----- | :----------------- |
| **Operational Excellence** | Ejecutar y monitorear | IaC, CI/CD, runbooks |
| **Security** | Proteger datos y sistemas | Least privilege, encryption, logging |
| **Reliability** | Recuperarse de fallos | Multi-AZ, backups, chaos engineering |
| **Performance** | Usar recursos eficientemente | Caching, auto-scaling, CDN |
| **Cost Optimization** | Evitar gastos innecesarios | Right-sizing, RIs, monitoring |

---

## 🔄 Disaster Recovery

| Estrategia | RTO | RPO | Costo | Descripción |
| :----------- | :---- | :---- | :------ | :------------ |
| **Backup & Restore** | Horas | Horas | Bajo | Restaurar desde backups |
| **Pilot Light** | Minutos | Minutos | Medio | Core siempre on, escalar al activar |
| **Warm Standby** | Segundos | Segundos | Alto | Ambiente reducido siempre activo |
| **Multi-Site Active** | Instantáneo | Cero | Muy alto | Activo en múltiples regiones |

**RTO:** Recovery Time Objective (tiempo hasta recuperar)  
**RPO:** Recovery Point Objective (pérdida de datos aceptable)

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Lift-and-shift sin optimizar** | No aprovechar cloud | Refactor para cloud-native |
| **Sin auto-scaling** | Pagar por capacidad ociosa | Implementar auto-scaling |
| **Single AZ** | Sin redundancia | Multi-AZ deployment |
| **Sin tagging** | Costos opacos | Tagging strategy |
| **Monolito en Lambda** | Cold starts enormes | Funciones pequeñas, especializadas |

---

## 📚 Recursos

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google Cloud Architecture Framework](https://cloud.google.com/architecture/framework)
- [Azure Architecture Center](https://docs.microsoft.com/en-us/azure/architecture/)
- [Cloud Native Computing Foundation](https://www.cncf.io/)
