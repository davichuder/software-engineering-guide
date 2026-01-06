# Caso de Estudio: Plataforma de Gestión de Gastos Compartidos (Diseño)

> **Ver Parte 2**: [Guía de Implementación Técnica](./gestion-gastos-compartidos-implementacion.md)
>
> **Referencia**: [97 - Casos de Estudio](./README.md)

---

## 1. Visión del Producto (PRD)

### 1.1 Contexto y Problema

Los grupos de amigos, parejas y compañeros de piso enfrentan fricción constante al gestionar finanzas compartidas. Las soluciones actuales son manuales (Excel), no soportan múltiples fuentes de dinero (Bancos, Efectivo) o carecen de inteligencia para procesar comprobantes automáticamente.

### 1.2 Job To Be Done (JTBD)

**Cuando** un usuario realiza un gasto grupal o recibe un resumen bancario,  
**Quiere** automatizar la carga, clasificación y división de esos gastos con mínima intervención,  
**Para así** tener visibilidad inmediata de sus deudas ("quién debe a quién") y liquidarlas con un solo clic.

### 1.3 Métricas de Éxito

- **Automatización**: >90% de los tickets procesados automáticamente.
- **Eficiencia**: Tiempo desde carga de ticket hasta deuda confirmada < 30 segundos.
- **Engagement**: WAU (Weekly Active Users) con >3 transacciones.
- **Calidad**: 0 errores de conciliación financiera.

---

## 2. Requisitos Funcionales y No Funcionales

### 2.1 Funcionalidades Principales (Core)

#### A. Gestión de Billeteras y Cuentas

- **Multi-Billetera**: Soporte para Efectivo, Cuentas Bancarias, Billeteras Virtuales (MercadoPago, PayPal) y Tarjetas de Crédito.
- **Cuentas Compartidas**: Monederos virtuales donde varios contribuyen (Fondo común).
- **Importación Inteligente**:
    - Lectura automática de resúmenes bancarios (PDF, Excel, CSV).
    - Parsing de comprobantes de transferencia (PDF/IMG).

#### B. Registro y División de Gastos (Splits)

- **División Flexible**:
    - **50/50**: División equitativa rápida.
    - **Porcentajes**: Configuración manual (ej. 60/40) o *Porcentajes por Defecto* (configurados en el perfil del usuario).
    - **Monto Fijo**: "Yo pongo $1000, dividan el resto".
    - **Shares**: División por unidades (ej. porciones de comida).
- **Categorización Avanzada**:
    - Tipos: Emergencias, Únicos, Recurrentes (Diario/Semanal/Mensual/Anual), Cuotas.
    - Etiquetas personalizadas para filtrado.

#### C. Dashboard y Flujo de Aprobación

- **Dashboard Financiero**: Vista de "Debes/Te Deben", Net Worth, Burn Rate, Totales por categoría. CRUD completo de movimientos.
- **Notificaciones Actionable**: Correo/Push para Aceptar, Rechazar o Corregir un gasto asignado.
- **Confirmación de Pagos**: Subida de comprobante de transferencia para saldar deudas (Settlement).

### 2.2 Funcionalidades Secundarias ("Nice to have")

- **Exportación**: Generación de reportes PDF/Excel para contabilidad.
- **Filtros Avanzados**: Por fecha, monto, miembro, etiqueta, cuenta.
- **Multimoneda**: Conversión automática al tipo de cambio del día (Integración API externa).
- **Historial**: Audit log visible de modificaciones ("Ana cambió el monto de $50 a $60").

### 2.3 Seguridad y Privacidad (Non-functional)

- **Encriptación**: PII (Identificadores Fiscales, Emails) cifrados en reposo (AES-256).
- **Privacidad**: Anonimización de tickets antes de enviar a APIs de IA externas.
- **Auth**: Autenticación doble factor (MFA) obligatoria para operaciones sensibles (Settlement).

---

## 3. Casos de Uso y Comportamiento (BDD)

### Feature: Procesamiento de Resumen Bancario

```gherkin
Feature: Importación de Resumen Bancario

  Scenario: Importación exitosa de CSV bancario
    Given el usuario "Ana" tiene una cuenta bancaria "Santander" configurada
    When sube un archivo "resumen_enero.csv" con 50 movimientos
    Then el sistema detecta 50 transacciones nuevas
    And clasifica automáticamente "Netflix" como "Suscripciones - Mensual"
    And detecta que "Transferencia a Juan" es un posible "Settlement"

  Scenario: Detección de duplicados (Caso Borde)
    Given ya existe un gasto de $500 en "Starbucks" el día 10/01
    When "Ana" sube un ticket de $500 de "Starbucks" con fecha 10/01
    Then el sistema marca el nuevo gasto como "Posible Duplicado"
    And solicita confirmación manual antes de guardarlo
```

### Feature: Split Complejo con Recurrencia

```gherkin
Feature: Gasto de Alquiler Recurrente

  Scenario: Configuración de Alquiler con Split Desigual
    Given que "Ana" gana el doble que "Beto"
    And tienen configurado "Split Proporcional Ingresos" (66% / 33%)
    When "Ana" carga el "Alquiler" de $100,000 como "Mensual"
    Then el sistema asigna $66,000 a "Ana" y $33,000 a "Beto"
    And programa la creación automática de este gasto para el día 1 del próximo mes
    And envía una notificación a "Beto" para aceptar su deuda de $33,000
```

---

## 4. Arquitectura de Software y Decisiones (ADRs)

### 4.1 Diagrama de Arquitectura (Microservicios)

```mermaid
graph TD
    Client[Angular FDA] --> API_GW[API Gateway]
    
    subgraph "Core Domain"
        API_GW --> Auth[Auth Service<br>(Keycloak)]
        API_GW --> Wallet[Wallet Service<br>(Accounts/Balances)]
        API_GW --> Expense[Expense Service<br>(Transactions/Splits)]
        API_GW --> Group[Group Service<br>(Members/Rules)]
    end
    
    subgraph "Intelligence Domain"
        API_GW --> Processor[Document Processor<br>(OCR/Parser)]
        Processor --> AI_Ext[OpenAI / Google Vision]
    end
    
    subgraph "Support"
        Wallet -.-> Kafka
        Expense -.-> Kafka
        Processor -.-> Kafka
        Kafka --> Notif[Notification Service]
        Kafka --> Audit[Audit Log Service]
    end
```

### 4.2 Architectural Decision Records (ADRs)

#### ADR-001: Backend Java + Spring Boot

- **Decisión**: Utilizar Java 21 y Spring Boot 3.2.
- **Por qué**: Necesidad de **tipado fuerte** para transacciones financieras (evitar errores de redondeo con `BigDecimal`). Ecosistema maduro para integración con Kafka y Batch processing (para resúmenes bancarios masivos).

#### ADR-002: Frontend Angular + Signals

- **Decisión**: Angular 17+ con Signals.
- **Por qué**: Manejo de formularios complejos (splits dinámicos) es nativo y robusto en Angular. Signals ofrece reactividad granular para el Dashboard sin overhead de Zone.js.

#### ADR-003: Integración MCP (Model Context Protocol)

- **Decisión**: Usar un Proxy MCP intermedio para servicios de IA.
- **Por qué**: Para centralizar la gestión de API Keys (rotación, rate limiting) y aplicar **sanitización de datos (PII scrubbing)** antes de que salgan de nuestra infraestructura.

#### ADR-004: Event-Driven para Procesamiento

- **Decisión**: Apache Kafka para subida de archivos.
- **Por qué**: El OCR y parsing son lentos. El usuario no debe esperar. Se sube archivo -> ACK inmediato -> Procesamiento asíncrono -> Notificación Push al finalizar.

---

## 5. Roadmap de Implementación

### Fase 1: MVP (Core Financiero)

- Login (Auth0/Keycloak).
- CRUD de Billeteras y Grupos.
- Carga Manual de Gastos.
- Split básico 50/50.
- Dashboard simple (Totales).

### Fase 2: Automatización (Data Entry Killer)

- Importadores CSV/Excel básicos.
- Integración OCR para tickets simples.
- Detección básica de duplicados (Monto + Fecha).

### Fase 3: Finanzas Avanzadas (Value Add)

- Motor de Settlement (Minimización de transferencias).
- Multimoneda.
- Dashboard avanzado (Burn Rate, Proyecciones).
- Presupuestos y Alertas (80% consumido).

### Fase 4: Inteligencia (AI)

- Categorización predictiva ("Aprendí que Jumbo es Supermercado").
- Detección de anomalías ("Este gasto es 300% mayor a tu promedio").
- Balance automático con liquidación 1-click (Integración Bancaria).

---

## 6. Riesgos y Mejoras Futuras

### Riesgos a Mitigar

1. **Fallos de Parsing**: Los bancos cambian formatos sin aviso. Solución: Arquitectura de "Drivers" de parsing actualizables independientemente.
2. **Exceso de Notificaciones**: Fatiga de usuario. Solución: Agrupar notificaciones ("Resumen diario") en lugar de 1 por gasto.
3. **Conflictos de Edición**: Race conditions en balances compartidos. Solución: Bloqueo optimista (Versioning) en base de datos.

### Mejoras Sugeridas (Futuro)

- **Modo Offline**: PWA con base de datos local (IndexedDB) y sincronización al conectar.
- **Calendario de Pagos**: Visualización de vencimientos en vista de mes.
