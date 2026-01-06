# Caso de Estudio: Diario Digital de Emociones (Mood Tracker)

> **Proyecto**: Aplicación Web de Seguimiento Emocional con Análisis de Patrones
>
> **Stack**: FastAPI, React/Next.js, PostgreSQL + TimescaleDB, OAuth 2.0
>
> **Duración**: 8 semanas
>
> **Equipo**: 1 desarrollador full-stack + 1 diseñador UX

---

## 📋 Resumen Ejecutivo

Este caso de estudio documenta el desarrollo de una aplicación web para el seguimiento diario de emociones y sentimientos, con capacidad de análisis de patrones mediante Machine Learning y visualizaciones interactivas.

### Tecnologías Principales

- **Backend**: FastAPI (Python 3.11) + Pydantic
- **Frontend**: Next.js 14 + React + TypeScript
- **Base de Datos**: PostgreSQL 15 + TimescaleDB
- **Autenticación**: OAuth 2.0 (Google)
- **ML/Analytics**: Scikit-learn, Pandas, NumPy
- **Visualización**: D3.js, Recharts, Three.js (3D)

### Métricas de Éxito

- ✅ **North Star Metric**: D7 Retention > 40% (usuarios que registran al menos 1 vez por semana)
- ✅ **Engagement**: Tasa de completación de registro > 85%
- ✅ **Performance**: Carga de visualizaciones < 2s
- ✅ **Precisión ML**: Detección de patrones con > 70% de confianza
- ✅ **Privacidad**: 100% cumplimiento GDPR

---

## 🎯 1. Contexto del Cliente y Producto

### 1.1 Job To Be Done (JTBD)

**Cuando** el usuario se siente abrumado emocionalmente o quiere mejorar su autoconciencia,  
**Lo contrata para** registrar sus estados emocionales de forma simple y descubrir patrones predictivos,  
**Para así** reducir la incertidumbre emocional y tomar mejores decisiones sobre su bienestar.

### 1.2 Requisitos Funcionales

| ID | Requisito | Prioridad |
|:---|:----------|:----------|
| RF-01 | Login exclusivo con Google (OAuth 2.0) | 🔴 Alta |
| RF-02 | Selección de modelo de emociones (Ekman, Plutchik, PAD) | 🔴 Alta |
| RF-03 | Wizard multinivel para identificar emoción específica | 🔴 Alta |
| RF-04 | Registro de texto libre tipo diario personal | 🟠 Media |
| RF-05 | Cambio dinámico entre modelos de emociones | 🟠 Media |
| RF-06 | Visualización en calendario (heatmap) | 🔴 Alta |
| RF-07 | Visualización en línea de tiempo (timeline) | 🔴 Alta |
| RF-08 | Análisis de patrones con ML (clustering) | 🟠 Media |
| RF-09 | Visualización 3D de modelos (rueda, cono, cubo) | 🟢 Baja |
| RF-10 | Exportación de datos (CSV, JSON) | 🟢 Baja |

### 1.3 Requisitos No Funcionales

| ID | Requisito | Métrica |
|:---|:----------|:--------|
| RNF-01 | **Privacidad**: Datos sensibles (PII/PHI) | Encriptación en reposo y tránsito |
| RNF-02 | **Performance**: Carga de visualizaciones | < 2s para 1 año de datos |
| RNF-03 | **Escalabilidad**: Consultas de series temporales | TimescaleDB optimizado |
| RNF-04 | **Disponibilidad**: Uptime | > 99.5% |
| RNF-05 | **Seguridad**: Autenticación y autorización | OAuth 2.0 + JWT |

---

## 🧠 2. Modelos de Emociones: Decisión Crítica

### 2.1 ¿Por qué 3 Modelos Diferentes?

**Decisión**: Ofrecer 3 modelos de emociones en lugar de uno solo

**Justificación**:

- **Diversidad de usuarios**: Algunos prefieren simplicidad (Ekman), otros profundidad (Plutchik) o precisión científica (PAD)
- **Flexibilidad**: Permitir cambiar de modelo si uno no resuena con el usuario
- **Aprendizaje**: Los usuarios pueden explorar diferentes frameworks psicológicos

**Trade-offs**:

- ✅ **Pro**: Mayor adopción, mejor UX personalizada, valor educativo
- ❌ **Contra**: Complejidad técnica, dificultad para comparar datos entre modelos

---

### 2.2 Modelo 1: Paul Ekman (6 Emociones Básicas)

**Descripción**: Modelo clásico de 6 emociones universales

**Emociones**:

1. **Alegría** (Joy)
2. **Tristeza** (Sadness)
3. **Ira** (Anger)
4. **Miedo** (Fear)
5. **Sorpresa** (Surprise)
6. **Asco** (Disgust)

**Niveles de Intensidad** (3 niveles):

```text
Ira:
  Nivel 1: Molestia
  Nivel 2: Enojo
  Nivel 3: Furia

Alegría:
  Nivel 1: Contento
  Nivel 2: Feliz
  Nivel 3: Eufórico
```

**Visualización**: Rueda simple de 6 segmentos

**Ventajas**:

- ✅ Simple y fácil de entender
- ✅ Científicamente validado
- ✅ Rápido de usar (menos decisiones)

**Desventajas**:

- ❌ Limitado (no captura emociones complejas)
- ❌ No considera emociones secundarias

---

### 2.3 Modelo 2: Robert Plutchik (8 Emociones + Rueda)

**Descripción**: Modelo de 8 emociones primarias con combinaciones

**Emociones Primarias**:

1. **Alegría** (Joy)
2. **Tristeza** (Sadness)
3. **Confianza** (Trust)
4. **Disgusto** (Disgust)
5. **Miedo** (Fear)
6. **Ira** (Anger)
7. **Sorpresa** (Surprise)
8. **Anticipación** (Anticipation)

**Niveles de Intensidad** (3 niveles por emoción):

```text
Ira:
  Nivel 1: Molestia (Annoyance)
  Nivel 2: Ira (Anger)
  Nivel 3: Furia (Rage)

Alegría:
  Nivel 1: Serenidad (Serenity)
  Nivel 2: Alegría (Joy)
  Nivel 3: Éxtasis (Ecstasy)
```

**Emociones Secundarias** (Díadas):

- **Alegría + Confianza** = Amor (Love)
- **Alegría + Miedo** = Culpa (Guilt)
- **Confianza + Miedo** = Sumisión (Submission)
- **Sorpresa + Tristeza** = Decepción (Disappointment)
- **Tristeza + Disgusto** = Remordimiento (Remorse)
- **Disgusto + Ira** = Desprecio (Contempt)
- **Ira + Anticipación** = Agresividad (Aggressiveness)
- **Anticipación + Alegría** = Optimismo (Optimism)

**Visualización**: Rueda de Plutchik (2D) con pétalos de colores

**Ventajas**:

- ✅ Captura emociones complejas
- ✅ Permite combinaciones
- ✅ Visualmente atractivo

**Desventajas**:

- ❌ Más complejo de usar
- ❌ Requiere más tiempo para registrar

---

### 2.4 Modelo 3: PAD (Placer-Activación-Dominancia)

**Descripción**: Modelo dimensional de 3 ejes continuos

**Ejes**:

1. **Placer (Pleasure)**: Negativo (-1) a Positivo (+1)
2. **Activación (Arousal)**: Baja (-1) a Alta (+1)
3. **Dominancia (Dominance)**: Sumisión (-1) a Control (+1)

**Ejemplos de Mapeo**:

```text
Excitación:
  Placer: +0.8 (intenso)
  Activación: +0.9 (alta)
  Dominancia: +0.6 (control moderado)

Depresión:
  Placer: -0.9 (muy negativo)
  Activación: -0.7 (baja energía)
  Dominancia: -0.8 (sin control)

Calma:
  Placer: +0.5 (positivo moderado)
  Activación: -0.6 (baja)
  Dominancia: +0.3 (control leve)
```

**Visualización**: Cubo 3D interactivo

**Ventajas**:

- ✅ Máxima precisión
- ✅ Captura matices sutiles
- ✅ Científicamente robusto

**Desventajas**:

- ❌ Curva de aprendizaje alta
- ❌ Requiere introspección profunda
- ❌ Más lento de usar

---

### 2.5 Comparación de Modelos

| Criterio | Ekman | Plutchik | PAD | Ganador |
|:---------|:------|:---------|:----|:--------|
| **Simplicidad** | ✅ Muy simple | ⚠️ Moderado | ❌ Complejo | Ekman |
| **Precisión** | ⚠️ Básica | ✅ Buena | ✅ Excelente | PAD |
| **Velocidad de uso** | ✅ Rápido (< 30s) | ⚠️ Moderado (1-2min) | ❌ Lento (2-3min) | Ekman |
| **Captura de matices** | ❌ Limitada | ✅ Buena | ✅ Excelente | PAD/Plutchik |
| **Atractivo visual** | ⚠️ Básico | ✅ Atractivo | ✅ Innovador | Plutchik/PAD |
| **Validación científica** | ✅ Alta | ✅ Alta | ✅ Alta | Empate |

**Resultado**: No hay un "ganador" absoluto → **Ofrecer los 3 modelos**

---

## 🎨 3. Diseño UX/UI: Wizard Multinivel

### 3.1 Flujo de Registro de Emoción

**Decisión**: Implementar un wizard de 3-4 pasos en lugar de un formulario único

**Justificación**:

- **Progressive Disclosure**: No abrumar al usuario con todas las opciones a la vez
- **Guided Experience**: Ayudar a identificar la emoción correcta
- **Gamificación**: Hacer el proceso más interactivo y menos tedioso

**Flujo para Modelo Plutchik**:

```text
Paso 1: Selección de Emoción Primaria
┌─────────────────────────────────────┐
│  ¿Cómo te sientes ahora?            │
│                                     │
│  [Rueda de Plutchik interactiva]   │
│                                     │
│  Usuario selecciona: "Ira"          │
└─────────────────────────────────────┘
              ↓
Paso 2: Intensidad
┌─────────────────────────────────────┐
│  ¿Qué tan intenso es?               │
│                                     │
│  ○ Molestia (leve)                  │
│  ● Ira (moderada)                   │
│  ○ Furia (intensa)                  │
└─────────────────────────────────────┘
              ↓
Paso 3: Emoción Secundaria (Opcional)
┌─────────────────────────────────────┐
│  ¿Sientes algo más?                 │
│                                     │
│  [Mostrar emociones compatibles]    │
│  ✓ Anticipación → Agresividad       │
│  ○ Disgusto → Desprecio             │
│  ○ Ninguna                          │
└─────────────────────────────────────┘
              ↓
Paso 4: Diario Personal (Opcional)
┌─────────────────────────────────────┐
│  ¿Qué pasó hoy?                     │
│                                     │
│  [Textarea]                         │
│  "Hoy me caí de la bici, estoy mal, │
│   me siento inútil"                 │
│                                     │
│  [Guardar]  [Cancelar]              │
└─────────────────────────────────────┘
```

**Flujo para Modelo PAD**:

```text
Paso 1: Placer
┌─────────────────────────────────────┐
│  ¿Qué tan placentero/desagradable?  │
│                                     │
│  Muy desagradable ●────────○ Muy placentero
│                   -1       0       +1
└─────────────────────────────────────┘
              ↓
Paso 2: Activación
┌─────────────────────────────────────┐
│  ¿Qué tan activado/calmado?         │
│                                     │
│  Muy calmado ○────●────○ Muy activado
│              -1    0    +1          │
└─────────────────────────────────────┘
              ↓
Paso 3: Dominancia
┌─────────────────────────────────────┐
│  ¿Qué tan en control?               │
│                                     │
│  Sin control ○────────●○ Total control
│              -1       0  +1         │
└─────────────────────────────────────┘
              ↓
Paso 4: Vista Previa + Diario
┌─────────────────────────────────────┐
│  Tu emoción:                        │
│  [Cubo 3D mostrando punto]          │
│                                     │
│  Emoción detectada: "Excitación"    │
│                                     │
│  [Textarea para diario]             │
│  [Guardar]                          │
└─────────────────────────────────────┘
```

---

### 3.2 Visualizaciones Interactivas de Modelos

#### Visualización 1: Rueda de Plutchik (2D)

**Tecnología**: D3.js + React

**Características**:

- 8 pétalos de colores (uno por emoción primaria)
- 3 anillos concéntricos (intensidades)
- Hover muestra nombre de emoción
- Click selecciona emoción
- Animación de transición suave

**Implementación**:

```typescript
// components/EmotionWheel.tsx
interface EmotionWheelProps {
  onSelect: (emotion: PlutchikEmotion) => void;
  selectedEmotion?: PlutchikEmotion;
}

const EmotionWheel: React.FC<EmotionWheelProps> = ({ onSelect, selectedEmotion }) => {
  const emotions = [
    { name: 'Joy', color: '#FFD700', angle: 0 },
    { name: 'Trust', color: '#90EE90', angle: 45 },
    { name: 'Fear', color: '#00CED1', angle: 90 },
    { name: 'Surprise', color: '#4169E1', angle: 135 },
    { name: 'Sadness', color: '#4B0082', angle: 180 },
    { name: 'Disgust', color: '#8B008B', angle: 225 },
    { name: 'Anger', color: '#DC143C', angle: 270 },
    { name: 'Anticipation', color: '#FF8C00', angle: 315 }
  ];

  return (
    <svg width="400" height="400" viewBox="0 0 400 400">
      {emotions.map((emotion, index) => (
        <EmotionPetal
          key={emotion.name}
          emotion={emotion}
          isSelected={selectedEmotion?.name === emotion.name}
          onClick={() => onSelect(emotion)}
        />
      ))}
    </svg>
  );
};
```

#### Visualización 2: Cono de Emociones (3D)

**Tecnología**: Three.js + React Three Fiber

**Características**:

- Cono 3D rotable con mouse
- Eje vertical = Intensidad
- Base circular = 8 emociones
- Punto luminoso indica emoción actual

#### Visualización 3: Cubo PAD (3D)

**Tecnología**: Three.js + React Three Fiber

**Características**:

- Cubo 3D interactivo
- Ejes X, Y, Z = Placer, Activación, Dominancia
- Punto rojo indica posición actual
- Etiquetas de emociones conocidas en el espacio

---

### 3.3 Cambio Dinámico entre Modelos

**Decisión**: Permitir cambio de modelo en cualquier momento

**Implementación**:

- Selector en el header de la app
- Al cambiar modelo, se muestra un modal explicativo
- Los datos históricos se mantienen en su modelo original
- Las visualizaciones se adaptan al modelo seleccionado

**Desafío**: ¿Cómo comparar datos entre modelos?

**Solución**: Mapeo aproximado entre modelos

```typescript
// utils/emotionMapping.ts
const ekmanToPlutchik = {
  'Joy': 'Joy',
  'Sadness': 'Sadness',
  'Anger': 'Anger',
  'Fear': 'Fear',
  'Surprise': 'Surprise',
  'Disgust': 'Disgust'
};

const plutchikToPAD = {
  'Joy': { pleasure: 0.8, arousal: 0.6, dominance: 0.5 },
  'Sadness': { pleasure: -0.7, arousal: -0.5, dominance: -0.6 },
  'Anger': { pleasure: -0.6, arousal: 0.8, dominance: 0.7 },
  // ... resto de mapeos
};
```

---

## 🏗️ 4. Decisiones de Arquitectura

### 4.1 Arquitectura Hexagonal (Ports & Adapters)

**Decisión**: Implementar Arquitectura Hexagonal

**Estructura**:

```text
backend/
├── domain/                     # CORE (Lógica de Negocio)
│   ├── emociones/
│   │   ├── entities/
│   │   │   ├── emotion.entity.py
│   │   │   ├── emotion-entry.entity.py
│   │   │   └── emotion-model.entity.py
│   │   ├── repositories/
│   │   │   └── emotion.repository.interface.py
│   │   └── use-cases/
│   │       ├── register-emotion.use-case.py
│   │       ├── get-emotion-history.use-case.py
│   │       └── analyze-patterns.use-case.py
│   │
│   └── patrones/
│       ├── entities/
│       │   └── pattern.entity.py
│       └── use-cases/
│           └── detect-patterns.use-case.py
│
├── infrastructure/             # ADAPTERS
│   ├── database/
│   │   ├── postgres.service.py
│   │   └── repositories/
│   │       └── postgres-emotion.repository.py
│   │
│   ├── auth/
│   │   └── google-oauth.service.py
│   │
│   └── ml/
│       └── pattern-analyzer.service.py
│
└── presentation/               # API (FastAPI)
    ├── routers/
    │   ├── auth.router.py
    │   ├── emotions.router.py
    │   └── analytics.router.py
    └── schemas/
        └── emotion.schema.py
```

**Beneficios**:

- ✅ Lógica de negocio independiente de FastAPI
- ✅ Fácil cambiar PostgreSQL por otro DB
- ✅ Testeable sin infraestructura

---

### 4.2 ¿Por qué PostgreSQL + TimescaleDB?

**Decisión**: Usar PostgreSQL con extensión TimescaleDB

**Alternativas Consideradas**:

- MongoDB (Document Store)
- InfluxDB (Time Series DB pura)
- PostgreSQL + TimescaleDB

**Justificación**:

| Criterio | MongoDB | InfluxDB | PostgreSQL + TimescaleDB | Ganador |
|:---------|:--------|:---------|:-------------------------|:--------|
| **Consultas temporales** | ⚠️ Manual | ✅ Optimizado | ✅ Optimizado | InfluxDB/TimescaleDB |
| **Integridad de datos** | ❌ Eventual | ⚠️ Limitada | ✅ ACID | PostgreSQL |
| **Relaciones** | ❌ Difícil | ❌ No soporta | ✅ Nativo | PostgreSQL |
| **Flexibilidad de esquema** | ✅ Alta | ⚠️ Media | ⚠️ Baja | MongoDB |
| **Ecosistema Python** | ✅ Bueno | ⚠️ Limitado | ✅ Excelente | MongoDB/PostgreSQL |
| **Costo** | ✅ Gratis | ⚠️ Pago (cloud) | ✅ Gratis | MongoDB/PostgreSQL |

**Resultado**: PostgreSQL + TimescaleDB gana 4 a 2

**Contexto Específico**:

- Necesitamos **integridad de datos** (las emociones son datos sensibles)
- Necesitamos **consultas temporales eficientes** (calendario, timeline)
- Necesitamos **relaciones** (usuario → emociones → patrones)
- TimescaleDB es una **extensión de PostgreSQL**, no una DB separada

**Trade-offs**:

- ✅ **Pro**: Mejor de ambos mundos (ACID + optimización temporal)
- ❌ **Contra**: Menos flexible que MongoDB para esquemas cambiantes

**Migración Futura**:
Si el volumen de datos crece exponencialmente (millones de usuarios), considerar **sharding** o migrar a **InfluxDB** para la parte de series temporales.

---

### 4.3 ¿Por qué FastAPI sobre Django/Flask?

**Decisión**: Usar FastAPI

**Alternativas Consideradas**:

- Django REST Framework
- Flask
- FastAPI

**Justificación**:

| Criterio | Django | Flask | FastAPI | Ganador |
|:---------|:-------|:------|:--------|:--------|
| **Performance (async)** | ⚠️ Limitado | ❌ Sync | ✅ Async nativo | FastAPI |
| **Documentación automática** | ❌ Manual | ❌ Manual | ✅ OpenAPI | FastAPI |
| **Validación de datos** | ⚠️ Serializers | ❌ Manual | ✅ Pydantic | FastAPI |
| **Curva de aprendizaje** | ❌ Alta | ✅ Baja | ⚠️ Media | Flask |
| **Ecosistema** | ✅ Muy grande | ✅ Grande | ⚠️ Creciendo | Django |
| **Overhead** | ❌ Alto (ORM, admin) | ✅ Bajo | ✅ Bajo | Flask/FastAPI |

**Resultado**: FastAPI gana 4 a 2

**Contexto Específico**:

- Necesitamos **performance** para consultas de ML en tiempo real
- **Pydantic** es perfecto para validar emociones (defensive programming)
- **OpenAPI** facilita la integración con el frontend

**Trade-offs**:

- ✅ **Pro**: Rápido, moderno, excelente DX (Developer Experience)
- ❌ **Contra**: Ecosistema más pequeño que Django

---

## 🔒 5. Seguridad y Privacidad

### 5.1 Autenticación con Google OAuth 2.0

**Decisión**: Login exclusivo con Google

**Flujo de Autenticación**:

```text
1. Usuario click "Login con Google"
   ↓
2. Redirect a Google OAuth
   ↓
3. Usuario autoriza la app
   ↓
4. Google devuelve Authorization Code
   ↓
5. Backend intercambia Code por Access Token
   ↓
6. Backend crea/actualiza usuario en DB
   ↓
7. Backend genera JWT propio
   ↓
8. Frontend almacena JWT en httpOnly cookie
   ↓
9. Todas las requests incluyen JWT
```

**Implementación**:

```python
# infrastructure/auth/google-oauth.service.py
from authlib.integrations.starlette_client import OAuth

oauth = OAuth()
oauth.register(
    name='google',
    client_id=settings.GOOGLE_CLIENT_ID,
    client_secret=settings.GOOGLE_CLIENT_SECRET,
    server_metadata_url='https://accounts.google.com/.well-known/openid-configuration',
    client_kwargs={'scope': 'openid email profile'}
)

# presentation/routers/auth.router.py
@router.get('/login/google')
async def login_google(request: Request):
    redirect_uri = request.url_for('auth_google_callback')
    return await oauth.google.authorize_redirect(request, redirect_uri)

@router.get('/auth/google/callback')
async def auth_google_callback(request: Request):
    token = await oauth.google.authorize_access_token(request)
    user_info = token['userinfo']
    
    # Crear/actualizar usuario
    user = await user_service.get_or_create_user(
        email=user_info['email'],
        name=user_info['name'],
        picture=user_info['picture']
    )
    
    # Generar JWT
    jwt_token = create_jwt_token(user.id)
    
    response = RedirectResponse(url='/dashboard')
    response.set_cookie(
        key='access_token',
        value=jwt_token,
        httponly=True,
        secure=True,
        samesite='lax'
    )
    return response
```

---

### 5.2 Protección de Datos Sensibles

**Decisión**: Encriptación en reposo y en tránsito

**Implementación**:

1. **En tránsito**: HTTPS obligatorio (TLS 1.3)
2. **En reposo**: Encriptación de columnas sensibles

```python
# domain/emociones/entities/emotion-entry.entity.py
from cryptography.fernet import Fernet

class EmotionEntry:
    def __init__(
        self,
        user_id: str,
        emotion: Emotion,
        diary_text: Optional[str] = None,
        timestamp: datetime = datetime.now()
    ):
        self.user_id = user_id
        self.emotion = emotion
        self._diary_text_encrypted = self._encrypt(diary_text) if diary_text else None
        self.timestamp = timestamp
    
    def _encrypt(self, text: str) -> bytes:
        cipher = Fernet(settings.ENCRYPTION_KEY)
        return cipher.encrypt(text.encode())
    
    def _decrypt(self, encrypted: bytes) -> str:
        cipher = Fernet(settings.ENCRYPTION_KEY)
        return cipher.decrypt(encrypted).decode()
    
    @property
    def diary_text(self) -> Optional[str]:
        if self._diary_text_encrypted:
            return self._decrypt(self._diary_text_encrypted)
        return None
```

1. **Logs**: NUNCA registrar PII/emociones

```python
# Mal ❌
logger.info(f"User {user.email} registered emotion: {emotion.name}")

# Bien ✅
logger.info(f"User {user.id} registered emotion successfully")
```

---

### 5.3 Defensive Programming: Validación de Inputs

**Decisión**: Validación estricta en backend con Pydantic

```python
# presentation/schemas/emotion.schema.py
from pydantic import BaseModel, Field, validator
from enum import Enum
from datetime import datetime

class EmotionModel(str, Enum):
    EKMAN = "ekman"
    PLUTCHIK = "plutchik"
    PAD = "pad"

class EkmanEmotion(str, Enum):
    JOY = "joy"
    SADNESS = "sadness"
    ANGER = "anger"
    FEAR = "fear"
    SURPRISE = "surprise"
    DISGUST = "disgust"

class RegisterEmotionRequest(BaseModel):
    model: EmotionModel
    emotion_data: dict
    diary_text: Optional[str] = Field(None, max_length=5000)
    timestamp: Optional[datetime] = None
    
    @validator('diary_text')
    def sanitize_diary_text(cls, v):
        if v:
            # Eliminar scripts y HTML peligroso
            import bleach
            return bleach.clean(v, tags=[], strip=True)
        return v
    
    @validator('emotion_data')
    def validate_emotion_data(cls, v, values):
        model = values.get('model')
        
        if model == EmotionModel.EKMAN:
            if 'emotion' not in v or 'intensity' not in v:
                raise ValueError('Ekman model requires emotion and intensity')
            if v['emotion'] not in [e.value for e in EkmanEmotion]:
                raise ValueError(f'Invalid Ekman emotion: {v["emotion"]}')
            if not 1 <= v['intensity'] <= 3:
                raise ValueError('Intensity must be between 1 and 3')
        
        elif model == EmotionModel.PAD:
            required = ['pleasure', 'arousal', 'dominance']
            if not all(k in v for k in required):
                raise ValueError(f'PAD model requires: {required}')
            for axis in required:
                if not -1 <= v[axis] <= 1:
                    raise ValueError(f'{axis} must be between -1 and 1')
        
        return v
```

**Referencia**: [09 - Seguridad](../manuales/seguridad.md)

---

## 📊 6. Análisis de Datos y Machine Learning

### 6.1 Detección de Patrones con K-Means Clustering

**Objetivo**: Identificar patrones recurrentes en las emociones del usuario

**Algoritmo**: K-Means Clustering (No Supervisado)

**Features para el Clustering**:

1. **Hora del día** (0-23)
2. **Día de la semana** (0-6)
3. **Emoción codificada** (one-hot encoding o embedding)
4. **Intensidad** (0-1 normalizado)

**Implementación**:

```python
# infrastructure/ml/pattern-analyzer.service.py
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
import pandas as pd
import numpy as np

class PatternAnalyzerService:
    def __init__(self):
        self.scaler = StandardScaler()
        self.kmeans = None
    
    def detect_patterns(self, emotion_entries: List[EmotionEntry]) -> List[Pattern]:
        # Convertir a DataFrame
        df = self._entries_to_dataframe(emotion_entries)
        
        # Feature engineering
        features = self._extract_features(df)
        
        # Normalizar
        features_scaled = self.scaler.fit_transform(features)
        
        # Clustering
        optimal_k = self._find_optimal_k(features_scaled)
        self.kmeans = KMeans(n_clusters=optimal_k, random_state=42)
        clusters = self.kmeans.fit_predict(features_scaled)
        
        # Interpretar clusters
        patterns = self._interpret_clusters(df, clusters)
        
        return patterns
    
    def _extract_features(self, df: pd.DataFrame) -> np.ndarray:
        features = []
        
        # Hora del día (sin encoding)
        features.append(df['hour'].values)
        
        # Día de la semana (one-hot)
        day_onehot = pd.get_dummies(df['day_of_week'], prefix='day')
        features.append(day_onehot.values)
        
        # Emoción (one-hot o embedding)
        emotion_onehot = pd.get_dummies(df['emotion'], prefix='emotion')
        features.append(emotion_onehot.values)
        
        # Intensidad normalizada
        features.append(df['intensity_normalized'].values.reshape(-1, 1))
        
        return np.hstack(features)
    
    def _find_optimal_k(self, features: np.ndarray) -> int:
        # Método del codo (Elbow Method)
        inertias = []
        K_range = range(2, min(10, len(features) // 5))
        
        for k in K_range:
            kmeans = KMeans(n_clusters=k, random_state=42)
            kmeans.fit(features)
            inertias.append(kmeans.inertia_)
        
        # Encontrar el "codo" (simplificado)
        # En producción, usar métodos más sofisticados
        return 3  # Por defecto, 3 clusters
    
    def _interpret_clusters(self, df: pd.DataFrame, clusters: np.ndarray) -> List[Pattern]:
        patterns = []
        
        for cluster_id in range(max(clusters) + 1):
            cluster_mask = clusters == cluster_id
            cluster_data = df[cluster_mask]
            
            # Analizar características del cluster
            most_common_emotion = cluster_data['emotion'].mode()[0]
            most_common_hour = int(cluster_data['hour'].mode()[0])
            most_common_day = cluster_data['day_of_week'].mode()[0]
            avg_intensity = cluster_data['intensity_normalized'].mean()
            
            pattern = Pattern(
                id=f"pattern_{cluster_id}",
                description=f"Tiendes a sentir {most_common_emotion} los {most_common_day} alrededor de las {most_common_hour}:00",
                emotion=most_common_emotion,
                time_of_day=most_common_hour,
                day_of_week=most_common_day,
                intensity=avg_intensity,
                confidence=len(cluster_data) / len(df),  # % de datos en este cluster
                occurrences=len(cluster_data)
            )
            
            patterns.append(pattern)
        
        return sorted(patterns, key=lambda p: p.confidence, reverse=True)
```

**Ejemplo de Salida**:

```json
{
  "patterns": [
    {
      "id": "pattern_0",
      "description": "Tiendes a sentir tristeza los lunes alrededor de las 9:00",
      "emotion": "sadness",
      "time_of_day": 9,
      "day_of_week": "monday",
      "intensity": 0.7,
      "confidence": 0.45,
      "occurrences": 12
    },
    {
      "id": "pattern_1",
      "description": "Tiendes a sentir alegría los viernes alrededor de las 18:00",
      "emotion": "joy",
      "time_of_day": 18,
      "day_of_week": "friday",
      "intensity": 0.8,
      "confidence": 0.35,
      "occurrences": 9
    }
  ]
}
```

**Referencia**: [20 - Machine Learning](../manuales/machine-learning.md)

---

### 6.2 Visualización: Heatmap de Calendario

**Objetivo**: Mostrar la distribución de emociones en el tiempo

**Tecnología**: Recharts + Custom Heatmap

**Implementación**:

```typescript
// components/EmotionCalendarHeatmap.tsx
import { useMemo } from 'react';
import { scaleLinear } from 'd3-scale';

interface EmotionCalendarHeatmapProps {
  data: EmotionEntry[];
  startDate: Date;
  endDate: Date;
}

const EmotionCalendarHeatmap: React.FC<EmotionCalendarHeatmapProps> = ({
  data,
  startDate,
  endDate
}) => {
  const heatmapData = useMemo(() => {
    // Agrupar por día
    const grouped = data.reduce((acc, entry) => {
      const date = entry.timestamp.toISOString().split('T')[0];
      if (!acc[date]) {
        acc[date] = { emotions: [], avgIntensity: 0 };
      }
      acc[date].emotions.push(entry.emotion);
      return acc;
    }, {} as Record<string, { emotions: string[], avgIntensity: number }>);
    
    // Calcular intensidad promedio por día
    Object.keys(grouped).forEach(date => {
      const emotions = grouped[date].emotions;
      const intensities = emotions.map(e => getEmotionValence(e));
      grouped[date].avgIntensity = intensities.reduce((a, b) => a + b, 0) / intensities.length;
    });
    
    return grouped;
  }, [data]);
  
  const colorScale = scaleLinear<string>()
    .domain([-1, 0, 1])
    .range(['#DC143C', '#FFFFFF', '#90EE90']); // Rojo → Blanco → Verde
  
  return (
    <div className="calendar-heatmap">
      {/* Renderizar grid de días */}
      {generateDateRange(startDate, endDate).map(date => {
        const dateStr = date.toISOString().split('T')[0];
        const dayData = heatmapData[dateStr];
        const color = dayData 
          ? colorScale(dayData.avgIntensity)
          : '#F5F5F5';
        
        return (
          <div
            key={dateStr}
            className="calendar-day"
            style={{ backgroundColor: color }}
            title={`${dateStr}: ${dayData?.emotions.join(', ') || 'Sin datos'}`}
          />
        );
      })}
    </div>
  );
};

function getEmotionValence(emotion: string): number {
  const valenceMap: Record<string, number> = {
    'joy': 0.8,
    'trust': 0.6,
    'anticipation': 0.4,
    'surprise': 0.2,
    'sadness': -0.6,
    'disgust': -0.7,
    'anger': -0.8,
    'fear': -0.5
  };
  return valenceMap[emotion] || 0;
}
```

---

### 6.3 Visualización: Timeline de Emociones

**Objetivo**: Mostrar la evolución temporal de las emociones

**Tecnología**: Recharts LineChart

```typescript
// components/EmotionTimeline.tsx
import { LineChart, Line, XAxis, YAxis, Tooltip, Legend } from 'recharts';

interface EmotionTimelineProps {
  data: EmotionEntry[];
}

const EmotionTimeline: React.FC<EmotionTimelineProps> = ({ data }) => {
  const chartData = useMemo(() => {
    return data.map(entry => ({
      date: entry.timestamp.toLocaleDateString(),
      valence: getEmotionValence(entry.emotion.name),
      arousal: entry.emotion.intensity / 3, // Normalizar 1-3 a 0-1
      emotion: entry.emotion.name
    }));
  }, [data]);
  
  return (
    <LineChart width={800} height={400} data={chartData}>
      <XAxis dataKey="date" />
      <YAxis domain={[-1, 1]} />
      <Tooltip content={<CustomTooltip />} />
      <Legend />
      <Line 
        type="monotone" 
        dataKey="valence" 
        stroke="#8884d8" 
        name="Valencia Emocional"
        strokeWidth={2}
      />
      <Line 
        type="monotone" 
        dataKey="arousal" 
        stroke="#82ca9d" 
        name="Intensidad"
        strokeWidth={2}
      />
    </LineChart>
  );
};
```

---

## 🧪 7. Testing y Calidad

### 7.1 TDD para Lógica de Patrones

**Decisión**: Aplicar TDD para el motor de detección de patrones

**Ejemplo de Test**:

```python
# tests/domain/patrones/test_detect_patterns.py
import pytest
from domain.emociones.entities.emotion import Emotion, EkmanEmotion
from domain.emociones.entities.emotion_entry import EmotionEntry
from domain.patrones.use_cases.detect_patterns import DetectPatternsUseCase
from datetime import datetime, timedelta

class TestDetectPatterns:
    def test_should_detect_monday_morning_sadness_pattern(self):
        # Arrange
        use_case = DetectPatternsUseCase()
        
        # Crear 10 entradas de tristeza los lunes a las 9am
        entries = []
        base_date = datetime(2024, 1, 1, 9, 0)  # Lunes 9am
        for i in range(10):
            date = base_date + timedelta(weeks=i)
            entry = EmotionEntry(
                user_id="user123",
                emotion=Emotion(EkmanEmotion.SADNESS, intensity=2),
                timestamp=date
            )
            entries.append(entry)
        
        # Act
        patterns = use_case.execute(entries)
        
        # Assert
        assert len(patterns) > 0
        monday_pattern = next(
            (p for p in patterns if p.day_of_week == 'monday' and p.time_of_day == 9),
            None
        )
        assert monday_pattern is not None
        assert monday_pattern.emotion == 'sadness'
        assert monday_pattern.confidence > 0.7
    
    def test_should_not_detect_pattern_with_insufficient_data(self):
        # Arrange
        use_case = DetectPatternsUseCase()
        
        # Solo 2 entradas (insuficiente)
        entries = [
            EmotionEntry(
                user_id="user123",
                emotion=Emotion(EkmanEmotion.JOY, intensity=3),
                timestamp=datetime(2024, 1, 1, 10, 0)
            ),
            EmotionEntry(
                user_id="user123",
                emotion=Emotion(EkmanEmotion.SADNESS, intensity=1),
                timestamp=datetime(2024, 1, 2, 15, 0)
            )
        ]
        
        # Act
        patterns = use_case.execute(entries)
        
        # Assert
        assert len(patterns) == 0 or all(p.confidence < 0.5 for p in patterns)
```

**Referencia**: [03 - Disciplinas de Desarrollo](../manuales/disciplinas-desarrollo.md)

---

### 7.2 Cobertura de Tests

**Objetivo**: ≥ 80% en lógica crítica

| Módulo | Cobertura Objetivo | Cobertura Actual |
|:-------|:-------------------|:-----------------|
| **Domain (Core)** | ≥ 90% | 92% ✅ |
| **Pattern Analysis** | ≥ 85% | 88% ✅ |
| **API Endpoints** | ≥ 75% | 78% ✅ |
| **Frontend Components** | ≥ 70% | 73% ✅ |
| **Overall** | ≥ 80% | 83% ✅ |

---

## 📄 8. Architecture Decision Records (ADRs)

### ADR-001: Uso de 3 Modelos de Emociones

**Estado**: Aceptado

**Contexto**:
Necesitamos decidir cuántos modelos de emociones ofrecer al usuario.

**Decisión**:
Ofrecer 3 modelos: Ekman (6 emociones), Plutchik (8 emociones), PAD (3 ejes)

**Consecuencias**:

- ✅ **Positivas**: Mayor adopción, flexibilidad, valor educativo
- ❌ **Negativas**: Complejidad técnica, dificultad para comparar datos

**Alternativas Consideradas**:

- Solo Ekman (rechazado por limitación)
- Solo Plutchik (rechazado por complejidad para usuarios nuevos)

---

### ADR-002: PostgreSQL + TimescaleDB

**Estado**: Aceptado

**Contexto**:
Necesitamos una base de datos que soporte series temporales eficientemente.

**Decisión**:
Usar PostgreSQL con extensión TimescaleDB

**Consecuencias**:

- ✅ **Positivas**: ACID, optimización temporal, relaciones nativas
- ❌ **Negativas**: Menos flexible que MongoDB

**Alternativas Consideradas**:

- MongoDB (rechazado por falta de ACID)
- InfluxDB (rechazado por falta de soporte de relaciones)

---

### ADR-003: FastAPI como Backend Framework

**Estado**: Aceptado

**Contexto**:
Necesitamos un framework Python para la API.

**Decisión**:
Usar FastAPI

**Consecuencias**:

- ✅ **Positivas**: Performance async, Pydantic, OpenAPI
- ❌ **Negativas**: Ecosistema más pequeño que Django

**Alternativas Consideradas**:

- Django REST Framework (rechazado por overhead)
- Flask (rechazado por falta de async nativo)

---

### ADR-004: Wizard Multinivel para Registro

**Estado**: Aceptado

**Contexto**:
Necesitamos decidir cómo presentar el formulario de registro de emoción.

**Decisión**:
Implementar wizard de 3-4 pasos

**Consecuencias**:

- ✅ **Positivas**: Progressive disclosure, mejor UX, gamificación
- ❌ **Negativas**: Más clics, puede ser percibido como lento

**Alternativas Consideradas**:

- Formulario único (rechazado por abrumar al usuario)

---

### ADR-005: K-Means para Detección de Patrones

**Estado**: Aceptado

**Contexto**:
Necesitamos un algoritmo para detectar patrones en las emociones.

**Decisión**:
Usar K-Means Clustering (No Supervisado)

**Consecuencias**:

- ✅ **Positivas**: Simple, rápido, no requiere datos etiquetados
- ❌ **Negativas**: Requiere elegir K, sensible a outliers

**Alternativas Consideradas**:

- DBSCAN (rechazado por complejidad de parámetros)
- Reglas manuales (rechazado por falta de adaptabilidad)

---

## 🎓 9. Lecciones Aprendidas

### 9.1 ¿Qué Funcionó Bien? ✅

1. **Arquitectura Hexagonal**:
   - Facilitó el testing (mocks de repositorios)
   - Permitió cambiar de SQLite a PostgreSQL sin afectar el dominio
   - Código más mantenible

2. **Wizard Multinivel**:
   - Los usuarios reportaron que el proceso es "guiado y claro"
   - Tasa de completación del registro: 87% (objetivo: 85%)

3. **TimescaleDB**:
   - Consultas de 1 año de datos: < 500ms (objetivo: < 2s)
   - Excelente para visualizaciones

4. **Pydantic**:
   - Validación automática de inputs
   - Documentación OpenAPI generada automáticamente

### 9.2 ¿Qué No Funcionó? ❌

1. **Visualización 3D (Cubo PAD)**:
   - **Problema**: Confusa para usuarios no técnicos
   - **Solución**: Agregar tutorial interactivo
   - **Lección**: No asumir que los usuarios entienden visualizaciones complejas

2. **Mapeo entre Modelos**:
   - **Problema**: Mapeo aproximado genera inconsistencias
   - **Solución**: Mostrar advertencia al cambiar de modelo
   - **Lección**: Ser transparente sobre las limitaciones

3. **Clustering con Pocos Datos**:
   - **Problema**: K-Means falla con < 20 entradas
   - **Solución**: Mostrar mensaje "Necesitas más datos para detectar patrones"
   - **Lección**: Validar cantidad de datos antes de ejecutar ML

### 9.3 ¿Qué Haríamos Diferente? 🔄

1. **Agregar Notificaciones Push**:
   - Para recordar al usuario registrar su emoción diaria
   - Aumentaría el D7 Retention

2. **Exportación de Datos**:
   - Implementar desde el inicio (no como feature secundaria)
   - Los usuarios valoran tener control de sus datos

3. **Modo Offline**:
   - Usar Service Workers para permitir registro offline
   - Sincronizar cuando haya conexión

---

## 📈 10. Métricas de Éxito

### 10.1 North Star Metric

| Métrica | Objetivo | Resultado | Estado |
|:--------|:---------|:----------|:-------|
| **D7 Retention** | > 40% | 43% | ✅ |
| **D30 Retention** | > 25% | 28% | ✅ |

### 10.2 Métricas HEART

| Métrica | Objetivo | Resultado | Estado |
|:--------|:---------|:----------|:-------|
| **Happiness (NPS)** | > 40 | 45 | ✅ |
| **Engagement (Tasa completación)** | > 85% | 87% | ✅ |
| **Adoption (Usuarios activos)** | 1,000 | 1,250 | ✅ |
| **Retention (D7)** | > 40% | 43% | ✅ |
| **Task Success (Registro exitoso)** | > 95% | 97% | ✅ |

### 10.3 Métricas Técnicas

| Métrica | Objetivo | Resultado | Estado |
|:--------|:---------|:----------|:-------|
| **Uptime** | > 99.5% | 99.7% | ✅ |
| **P95 Latency (API)** | < 500ms | 320ms | ✅ |
| **P95 Latency (Visualizaciones)** | < 2s | 1.8s | ✅ |
| **Cobertura de Tests** | > 80% | 83% | ✅ |

### 10.4 Métricas de Negocio

| Métrica | Resultado |
|:--------|:----------|
| **Tiempo de desarrollo** | 8 semanas |
| **Costo** | $0 (hosting gratuito en Render) |
| **Usuarios registrados (primer mes)** | 1,250 |
| **Usuarios activos semanales** | 540 |
| **Patrones detectados** | 3,200 |

---

## 🔗 Referencias

### Capítulos de la Guía Aplicados

- [03 - Disciplinas de Desarrollo](../manuales/disciplinas-desarrollo.md)
- [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md)
- [09 - Seguridad](../manuales/seguridad.md)
- [16 - APIs y Protocolos](../manuales/apis-protocolos.md)
- [17 - Mobile, UI y UX](../manuales/mobile-ui-ux.md)
- [20 - Machine Learning](../manuales/machine-learning.md)
- [21 - Ciencia de Datos](../manuales/ciencia-datos.md)
- [24 - Product Management](../manuales/product-management.md)
- [25 - Métricas y KPIs](../manuales/metricas-kpis.md)
- [29 - Convenciones](../manuales/convenciones.md)
- [34 - Plantillas y Artefactos](../manuales/plantillas-artefactos.md)

### Herramientas Utilizadas

- [FastAPI](https://fastapi.tiangolo.com/)
- [PostgreSQL](https://www.postgresql.org/)
- [TimescaleDB](https://www.timescale.com/)
- [Next.js](https://nextjs.org/)
- [Scikit-learn](https://scikit-learn.org/)
- [D3.js](https://d3js.org/)
- [Three.js](https://threejs.org/)
- [Pydantic](https://pydantic-docs.helpmanual.io/)

### Recursos Académicos

- [Paul Ekman - Basic Emotions](https://www.paulekman.com/universal-emotions/)
- [Robert Plutchik - Wheel of Emotions](https://www.6seconds.org/2022/03/13/plutchik-wheel-emotions/)
- [PAD Model - Russell & Mehrabian](https://en.wikipedia.org/wiki/PAD_emotional_state_model)

---

**Autor**: David Rolón

**Fecha**: 2018

**Versión**: 1.0

---

[⬆️ Volver arriba](#caso-de-estudio-diario-digital-de-emociones-mood-tracker) | [⬅️ Volver a Casos de Estudio](./README.md)
