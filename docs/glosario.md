# Glosario de Términos

> Definiciones de acrónimos y términos técnicos utilizados en la guía.

---

## A

### API (Application Programming Interface)

Interfaz que permite que dos aplicaciones se comuniquen entre sí. Definición de contratos de entrada y salida.

### Action (Acción)

En el contexto de storytelling con datos, es el paso final donde se propone qué hacer basándose en los insights encontrados.

## B

### Buyer Persona

Representación semi-ficticia del cliente ideal basada en investigación de mercado y datos reales de clientes existentes. Se enfoca en motivaciones, objetivos y comportamiento.

## C

### CAC (Customer Acquisition Cost)

Costo de Adquisición de Cliente. Costo total de ventas y marketing dividido por el número de nuevos clientes adquiridos.

### CapEx (Capital Expenditure)

Gastos de capital. Inversión inicial en activos fijos (como servidores físicos) que se deprecia con el tiempo. Modelo tradicional de compra de IT.

### Churn

Tasa de cancelación. Porcentaje de clientes que dejan de usar un servicio en un período determinado.

### CLS (Cumulative Layout Shift)

Métrica de Core Web Vitals que mide la estabilidad visual de una página. Cuanto menor sea el puntaje, mejor.

### Cold Start

Latencia inicial que ocurre cuando una función serverless es invocada por primera vez después de estar inactiva, ya que el proveedor debe aprovisionar el entorno de ejecución.

## D

### Data Lake

Repositorio centralizado que permite almacenar todos los datos estructurados y no estructurados a cualquier escala, generalmente en su formato nativo.

### Data Leakage (Fuga de Datos)

En Machine Learning, ocurre cuando el modelo de entrenamiento tiene acceso accidental a datos del set de prueba o datos que no estarían disponibles en producción, inflando falsamente las métricas de rendimiento.

### Data Warehouse

Sistema utilizado para reportes y análisis de datos. Almacena datos estructurados y procesados, optimizados para consultas complejas (OLAP).

### DOWNTIME (Lean)

Acrónimo para recordar los 8 desperdicios de Lean:

- **D**efects (Defectos)
- **O**verproduction (Sobreproducción)
- **W**aiting (Esperas)
- **N**on-Utilized Talent (Talento no utilizado)
- **T**ransportation (Transporte innecesario)
- **I**nventory (Inventario exceso)
- **M**otion (Movimiento innecesario)
- **E**xtra-Processing (Sobre-procesamiento)

## E

### ETL / ELT (Extract, Transform, Load)

Procesos de integración de datos. ETL transforma antes de cargar (típico de Data Warehouses tradicionales), ELT carga y luego transforma (típico de Data Lakes y Cloud Warehouses modernos).

## F

### FID (First Input Delay)

Métrica de Core Web Vitals que mide la interactividad. El tiempo desde que un usuario interactúa por primera vez hasta que el navegador responde.

### FinOps

Práctica cultural y disciplina de gestión financiera en la nube que permite a las organizaciones obtener el máximo valor de negocio ayudando a equipos de ingeniería, finanzas y negocio a colaborar en decisiones de gasto basadas en datos.

### Framework

Conjunto de herramientas y librerías que proveen una base estructurada para el desarrollo de software.

## I

### IaaS (Infrastructure as a Service)

Modelo de servicio cloud donde el proveedor ofrece recursos de computación virtualizados (VMs, redes, almacenamiento) sobre internet. El usuario gestiona el OS y las aplicaciones.

### ICP (Ideal Customer Profile)

Perfil de Cliente Ideal. Descripción hipotética del tipo de empresa que obtendría más valor de tu producto o servicio y que, a su vez, genera más valor para tu negocio. Se enfoca en datos firmográficos (tamaño, industria, budget).

## L

### Latency (Latencia)

Tiempo que tarda un paquete de datos en viajar desde un origen hasta un destino. En servicios web, suele ser el tiempo de respuesta de una solicitud.

### LCP (Largest Contentful Paint)

Métrica de Core Web Vitals que mide la velocidad de carga percibida. Marca el punto en la línea de tiempo de carga de la página cuando el contenido principal se ha cargado.

### LTV (Lifetime Value)

Valor de Vida del Cliente. Estimación de los ingresos netos totales que generará un cliente durante toda su relación con la empresa.

## N

### NPS (Net Promoter Score)

Métrica que mide la lealtad del cliente basándose en una sola pregunta: "¿Qué tan probable es que recomiende nuestro producto a un amigo o colega?".

## O

### OpEx (Operational Expenditure)

Gastos operativos. Costos recurrentes necesarios para mantener un producto o sistema, como suscripciones de cloud (pago por uso). Se deduce impuestos en el mismo año.

### Overfitting (Sobreajuste)

En Machine Learning, cuando un modelo aprende demasiado bien los detalles y el "ruido" del set de entrenamiento hasta el punto de impactar negativamente su rendimiento con datos nuevos.

## P

### PaaS (Platform as a Service)

Modelo de servicio cloud donde el proveedor ofrece hardware y herramientas de software (runtime, bases de datos) para desarrollo de aplicaciones. El usuario solo gestiona el código y los datos.

### p99 (Percentil 99)

Valor por debajo del cual cae el 99% de las observaciones. En latencia, indica que el 99% de las peticiones son más rápidas que este valor. Es útil para medir la experiencia de los "peores" casos (tail latency).

## R

### Reserved Instances (Instancias Reservadas)

Opción de compra en la nube que ofrece un descuento significativo (comparado con precios on-demand) a cambio de comprometerse a un contrato de uso por un período (generalmente 1 o 3 años).

### Right-sizing

Proceso de ajustar los recursos de la nube (como tipos de instancia o almacenamiento) para que coincidan lo más cerca posible con las necesidades de rendimiento y capacidad de la carga de trabajo, minimizando el desperdicio.

## S

### SaaS (Software as a Service)

Modelo de distribución de software donde el proveedor aloja las aplicaciones y las pone a disposición de los clientes a través de internet. El usuario solo consume el software.

### Serverless

Modelo de ejecución en la nube donde el proveedor gestiona dinámicamente la asignación y aprovisionamiento de servidores. El precio se basa en los recursos reales consumidos por la aplicación en ejecución.

### Smoke Tests (Pruebas de Humo)

Pruebas preliminares para revelar fallos simples lo suficientemente severos como para rechazar un lanzamiento de software prospectivo. "Donde hay humo, hay fuego".

### Spot Instances

Capacidad de computación en la nube no utilizada que está disponible a precios muy reducidos (hasta 90% de descuento), pero que el proveedor puede reclamar con poco aviso. Ideal para cargas de trabajo tolerantes a fallos.

### Stakeholder

Parte interesada. Cualquier persona o grupo que tiene interés en el proyecto o es afectado por sus resultados (ej. clientes, equipo de desarrollo, inversores).

## T

### Throughput

Cantidad de trabajo (requests, transacciones, bits) que un sistema puede procesar en un período de tiempo determinado.

## U

### Underfitting (Subajuste)

En Machine Learning, ocurre cuando un modelo no puede capturar la tendencia subyacente de los datos, generalmente porque el modelo es demasiado simple para la complejidad del problema.
