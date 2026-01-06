# Caso de Estudio: Alarma por Ubicación (Geofencing Alarm)

> **Proyecto**: Sistema de Alarma Geo-referenciada de Alta Fiabilidad
>
> **Stack**: Flutter (UI) + Kotlin/Swift (Native Core), SQLite, Serverless (AWS Lambda)
>
> **Duración**: 14 semanas
>
> **Equipo**: 1 Mobile Engineer + 1 Backend/DevOps

---

## 📋 Resumen Ejecutivo

Este caso de estudio documenta el desarrollo de una aplicación móvil crítica donde el **fallo no es una opción**. La app despierta al usuario cuando llega a su destino, un problema aparentemente simple que esconde una enorme complejidad técnica debido a las restricciones de los sistemas operativos móviles modernos (Doze mode, matar procesos en background).

### Tecnologías Principales

- **Mobile Frontend**: Flutter (para UI multiplataforma rápida y expresiva).
- **Mobile Core**: Kotlin (Android Foreground Services) y Swift (iOS Background Tasks) para la lógica crítica.
- **Arquitectura**: Hexagonal / Clean Architecture (independencia entre UI y Core).
- **Persistencia**: SQLite (Drift) con enfoque Offline-First.
- **Backend**: AWS Serverless (Lambda + DynamoDB) para sincronización de favoritos (costo casi cero).

### Métricas de Éxito

- ✅ **North Star Metric**: Frecuencia de Viajes Monitoreados con Éxito (> 98%).
- ✅ **Eficiencia de Batería**: < 5% de consumo por hora de monitoreo activo.
- ✅ **Latencia de Activación**: < 30 segundos desde cruzar el geofence.
- ✅ **Customer Effort Score (CES)**: < 2.0 (Muy bajo esfuerzo para configurar una alarma).

---

## 🎯 1. PRD (Product Requirements Document)

El modelo de negocio se basa en **LTV > CAC** con un enfoque Freemium (Ads no intrusivos + Donaciones).

### 1.1 Contexto y Job To Be Done (JTBD)

**Cuando** viajo en transporte público y estoy cansado,  
**Lo contrato para** que me despierte automáticamente al llegar a mi destino,  
**Para así** poder dormir tranquilo sin miedo a pasarme de parada.

### 1.2 Requisitos Funcionales

| ID | Requisito | Prioridad | Complejidad |
| :--- | :--- | :--- | :--- |
| **RF-01** | **Monitoreo en 2do Plano**: La app debe monitorear la ubicación incluso si está cerrada o el teléfono bloqueado. | 🔴 Crítica | Muy Alta |
| **RF-02** | **Alarma Crítica**: El sonido debe sonar incluso si el teléfono está en modo "No Molestar" o Silencio. | 🔴 Crítica | Alta |
| **RF-03** | **Gestión de Ubicaciones**: CRUD de alarmas favoritas con nombre, radio y configuración. | 🟠 Alta | Media |
| **RF-04** | **Búsqueda de Lugares**: Integración con Google Places / Mapbox para buscar destinos. | 🟠 Alta | Media |
| **RF-05** | **Historial de Viajes**: Registro de alarmas disparadas exitosamente y fallidas. | 🟢 Media | Baja |
| **RF-06** | **Modo Ahorro**: Desactivar GPS automáticamente si el usuario no se mueve por 15 min. | 🟢 Media | Alta |
| **RF-07** | **Sincronización Cloud**: Respaldo de favoritos en la nube (Login opcional). | 🟢 Baja | Media |

### 1.3 Requisitos No Funcionales

| ID | Requisito | Métrica / Target | Criticidad |
| :--- | :--- | :--- | :--- |
| **RNF-01** | **Consumo de Batería** | < 5% por hora de viaje | 🔴 Crítica (Retention Killer) |
| **RNF-02** | **Latencia de Disparo** | < 30s tras cruzar el perímetro | 🔴 Crítica |
| **RNF-03** | **Tasa de Falsos Positivos** | < 1% (Despertar antes de tiempo) | 🟠 Alta |
| **RNF-04** | **Reinicio Automático** | < 5s tras reinicio del SO | 🔴 Crítica |
| **RNF-05** | **Tiempo de Inicio (TTI)** | < 1.5s para ver el mapa | 🟢 Media |
| **RNF-06** | **Disponibilidad Offline** | 100% (Funciona sin internet) | 🔴 Crítica |

### 1.4 Requisitos de Seguridad y Privacidad

| Requisito | Justificación | Implementación |
| :--- | :--- | :--- |
| **Permisos de Ubicación** | Datos sensibles (GDPR/CCPA). El usuario debe confiar en la app. | Solicitar "Solo al usar" primero, luego escalar a "Todo el tiempo" con explicación clara (UI nativa). |
| **Minimización de Datos** | No rastrear al usuario cuando no hay alarma activa. | El servicio de ubicación se DESTRUYE completamente al finalizar la alarma. |
| **Anonimización Cloud** | Si usa sync, no vincular lugares con identidad real. | IDs aleatorios en DynamoDB. Auth con "Sign in with Apple/Google" (ocultar email). |

---

## 🏗️ 2. Decisiones de Arquitectura

### 2.1 ¿Por qué Flutter + Nativo (Híbrido Optimizado)?

Esta fue la decisión técnica más importante del proyecto. Analizamos 3 opciones:

| Criterio | PWA | React Native / Flutter (Puro) | Híbrido Optimizado (Flutter UI + Core Nativo) |
| :--- | :--- | :--- | :--- |
| **Desarrollo UI** | ✅ Muy Rápido | ✅ Muy Rápido | ✅ Muy Rápido |
| **Performance UI** | ⚠️ DOM lento | 🟢 60fps | 🟢 **120fps (Skia/Impeller)** |
| **Background Execution** | 🔴 No soportado (iOS) | ⚠️ Limitado / Inestable | 🟢 **Nativo (Foreground Service / BG Tasks)** |
| **Acceso a Hardware** | ⚠️ API Web limitada | 🟠 Plugins de terceros | 🟢 **API Directa (Kotlin/Swift)** |
| **Riesgo de "Kill" por OS** | 🔴 Muy Alto | 🟠 Medio | 🟢 **Bajo (Prioridad Alta)** |

**Decisión**: Usar **Flutter** solo para la capa de presentación (UI) y comunicación. Escribir toda la lógica de la "Máquina de Estados de Alarma" en **Kotlin (Android)** y **Swift (iOS)**.

**Justificación**: Las librerías de geofencing de la comunidad open-source suelen ser genéricas y fallan en casos borde (Doze mode agresivo en Samsung/Xiaomi). Al escribir el core nativo, tenemos control total sobre los `WakeLocks` y las notificaciones de alta prioridad.

### 2.2 ¿Por qué SQLite (Offline-First)?

**Decisión**: Usar SQLite (vía `Drift` en Flutter) como *Source of Truth*.

**Justificación**:

1. **Fiabilidad**: La alarma debe funcionar en el subte, en la ruta o en modo avión. Una base de datos local garantiza esto.
2. **Performance**: Consultas instantáneas sin latencia de red.
3. **Persistencia de Estado**: Si el teléfono se reinicia, el servicio nativo consulta SQLite para saber si había una alarma activa y la restaura automáticamente (`BOOT_COMPLETED`).

---

## ⚙️ 3. Core: La Máquina de Estados Finita (FSM)

Para manejar la complejidad asíncrona del GPS y el estado del usuario, implementamos una FSM robusta.

### Diagrama de Estados

| Estado | Evento (Trigger) | Transición | Lógica de Negocio |
| :--- | :--- | :--- | :--- |
| **IDLE** | Usuario activa alarma | → PENDING | Registrar Lat/Long destino. Validar permisos. Iniciar Servicio. |
| **PENDING** | `distance < 5 * umbral` | → APPROACHING | **Aumentar muestreo GPS** (de 5min a 30s). WakeLock parcial. |
| **APPROACHING** | `distance <= umbral` | → ALERT_ACTIVE | **Disparar Alarma Crítica**. Romper modo "No Molestar" (AudioAttributes.USAGE_ALARM). |
| **ALERT_ACTIVE** | Botón "Stop" / Salir de zona | → RESOLVED | Detener sonido. Guardar historial. Mostrar Ad. |
| **SNOOZED** | Timer acaba | → APPROACHING | Reiniciar verificación de distancia. |

### Implementación del Guard (Kotlin/Pseudo-código)

El FSM utiliza **Guards** para evitar falsos positivos por "rebote" del GPS (Multipath error).

```kotlin
// Android Foreground Service Logic
fun onLocationUpdate(location: Location) {
    val distance = calculateDistance(target, location)
    
    when (currentState) {
        State.PENDING -> {
            // Guard: Solo pasar a APPROACHING si estamos consistentemente cerca
            if (distance < approachThreshold) {
                consecutiveNearReadings++
                // Debounce: Esperar 3 lecturas seguidas para confirmar
                if (consecutiveNearReadings >= 3) transitionTo(State.APPROACHING)
            } else {
                consecutiveNearReadings = 0
            }
        }
        State.APPROACHING -> {
            // Adaptive Polling: Más cerca = Más frecuencia (Ahorro batería)
            locationRequest.interval = calculateDynamicInterval(distance)
            
            if (distance <= triggerThreshold) {
                transitionTo(State.ALERT_ACTIVE)
            }
        }
        // ...
    }
}
```

---

## 🛡️ 4. Resiliencia, Calidad y Batería

Este sistema es análogo a un **soporte vital**: si falla, el usuario sufre una consecuencia real (perderse).

### 4.1 Estrategia de Consumo de Batería (FinOps Aplicado)

Usamos **Muestreo Adaptativo** (Adaptive Polling).

- **Lejos (> 10km)**: Usar Geofencing del OS (despierta la app solo al entrar en la región grande). Consumo casi nulo.
- **Cerca (< 5km)**: Muestreo GPS cada 2-5 minutos.
- **Muy Cerca (< 1km)**: Muestreo GPS cada 30 segundos (Alta precisión).

### 4.2 Fallback Circuit Breaker

Si el GPS no responde en **2 minutos** (túnel, error de hardware):

1. **Fallback**: Intentar obtener ubicación por Red Celular/Wi-Fi (Menor precisión, mayor disponibilidad).
2. **Degradación Graciosa**: Si falla la ubicación, usar el acelerómetro para detectar movimiento y estimar distancia (Dead Reckoning simple) o alertar al usuario con vibración preventiva y notificación "Señal GPS perdida".

---

## 🎨 5. Experiencia de Usuario (UI/UX)

La UI fue diseñada siguiendo principios de **"Calm Technology"**.

### Principios Aplicados

1. **Don't Make Me Think**:
    - *Problema*: El usuario no sabe cuántos metros poner de radio.
    - *Solución*: "Smart Radius". La app sugiere el radio basado en la velocidad de acercamiento (ej. si vas en tren rápido, radio de 2km; si caminas, 200m).
2. **Feedback Visceral**:
    - Al activar la alarma, una animación de "onda" expansiva confirma visualmente que el monitoreo comenzó.
    - Haptic Feedback (vibración) acompaña las transiciones de estado.
3. **Accesibilidad**:
    - **Modo Oscuro Real**: (OLED Black) automático para ahorro de batería nocturno.
    - **TTS (Text-to-Speech)**: "Estás llegando a Casa". Vital para usuarios con discapacidad visual o auriculares puestos.

---

## 🧪 6. Estrategia de Testing

Debido a la naturaleza híbrida y de background, el testing fue complejo.

| Tipo de Test | Cobertura | Herramientas | Qué se prueba |
| :--- | :--- | :--- | :--- |
| **Unitarios** | 100% Core Logic | `flutter_test`, `JUnit`, `XCTest` | Lógica de FSM, conversores de unidades, cálculo de distancias. |
| **Widget Tests** | 90% UI | `flutter_test` | Flujos de navegación, validación de formularios, renderizado de listas. |
| **Integration** | Críticos | `patrol` / `integration_test` | Comunicación Flutter ↔ Nativo (Method Channels). |
| **E2E & Stress** | Key Flows | Device Farm (Firebase Test Lab) | **Simulación de Viaje**: Inyectar mock locations GPX para simular un recorrido completo y verificar disparo. |

---

## 📄 7. Documentación y ADRs (Architecture Decision Records)

### ADR-001: Lógica de Ubicación Nativa

- **Estado**: Aceptado
- **Contexto**: Necesitamos garantizar la ejecución en background en Android 14+ y iOS 17+. Flutter no tiene control fino sobre el ciclo de vida del *Service* de Android.
- **Decisión**: Implementar `LocationManager` en Kotlin y `CLLocationManager` en Swift, exponiendo métodos vía `MethodChannel`.
- **Consecuencias**: Aumenta la complejidad del codebase y requiere conocimientos de 3 lenguajes. Mejora drásticamente la fiabilidad (Metric: Crash-free sessions > 99.9%).

### ADR-002: Persistencia Local (SQLite)

- **Estado**: Aceptado
- **Contexto**: La app debe funcionar sin internet.
- **Decisión**: Usar SQLite con la librería `Drift`.
- **Consecuencias**: Requiere migraciones de esquema manuales. Garantiza funcionamiento offline 100%.

### ADR-003: Renderizado de Mapas

- **Estado**: Aceptado
- **Contexto**: Mostrar mapas consume muchos recursos y datos.
- **Decisión**: Usar `flutter_map` (OpenStreetMap) en lugar de Google Maps SDK.
- **Justificación**: Reducción de costos (Google Maps es caro a escala) y mayor flexibilidad de cacheo offline de tiles.

---

## 🔮 8. Lecciones Aprendidas

1. **No confíes en el emulador**: El comportamiento del GPS y Doze mode en emuladores no tiene nada que ver con dispositivos reales (especialmente marcas chinas con capas de optimización agresivas).
2. **El usuario miente sobre los permisos**: Siempre verificar el estado del permiso en `onResume`, no asumir que lo tenemos porque lo pedimos ayer.
3. **Audio Focus es difícil**: Interrumpir Spotify/Podcast para sonar la alarma requiere gestionar el "Audio Focus" nativo correctamente, bajando el volumen de la música (ducking) en lugar de pausarla abruptamente.

---

[⬆️ Volver arriba](#caso-de-estudio-alarma-por-ubicacion-geofencing-alarm) | [🏠 Volver a Casos de Estudio](./README.md)
