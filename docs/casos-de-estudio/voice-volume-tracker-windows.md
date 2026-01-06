# Caso de Estudio: Voice Volume Tracker para Windows

> **Proyecto**: Detector de Volumen de Voz con Identificación de Hablante
>
> **Stack**: C# + .NET 8, WPF, NAudio, ML.NET/ONNX, Windows Service
>
> **Duración**: 12 semanas
>
> **Equipo**: 1 desarrollador + 1 ML engineer

---

## 📋 Resumen Ejecutivo

Este caso de estudio documenta el desarrollo de una aplicación nativa de Windows que monitorea el volumen de voz del usuario en tiempo real, identifica su voz específica usando Machine Learning, y proporciona feedback visual cuando excede umbrales configurables según el momento del día.

### Tecnologías Principales

- **Frontend**: WPF (Windows Presentation Foundation) + C# 12
- **Backend Service**: Windows Service + .NET 8
- **Audio Processing**: NAudio + MathNet.Numerics
- **Machine Learning**: ML.NET + ONNX Runtime (SpeechBrain embeddings)
- **IPC**: Named Pipes + SignalR
- **Persistencia**: SQLite + JSON config
- **Alertas**: WinUI 3 Notifications + DirectX Overlay

### Métricas de Éxito

- ✅ **North Star Metric**: Tasa de Corrección de Volumen > 70%
- ✅ **Latencia de Detección**: < 200ms desde que se excede el umbral
- ✅ **Precisión de Identificación**: > 95% (solo alertar cuando habla el usuario)
- ✅ **Tasa de Falsos Positivos**: < 5%
- ✅ **Consumo de CPU**: < 3% en idle, < 8% durante habla
- ✅ **Consumo de RAM**: < 150 MB

---

## 🎯 1. PRD (Product Requirements Document)

### 1.1 Contexto y Job To Be Done (JTBD)

**Cuando** trabajo o convivo en un espacio compartido (oficina, hogar),  
**Lo contrato para** monitorear mi volumen de voz y recibir feedback discreto en tiempo real,  
**Para así** mantener el respeto por el entorno, mejorar mi etiqueta de comunicación, y evitar molestar a otros.

### 1.2 Requisitos Funcionales

| ID | Requisito | Prioridad | Complejidad |
|:---|:----------|:----------|:------------|
| RF-01 | Monitoreo continuo de micrófono en segundo plano | 🔴 Alta | Alta |
| RF-02 | Cálculo de volumen en decibeles (dBFS) en tiempo real | 🔴 Alta | Media |
| RF-03 | Identificación de voz del usuario (Speaker Verification) | 🔴 Alta | Muy Alta |
| RF-04 | Configuración de umbrales de volumen (Día/Noche) | 🔴 Alta | Media |
| RF-05 | Modo Día y Modo Noche con transición automática | 🟠 Media | Media |
| RF-06 | Alerta visual cuando se excede el umbral | 🔴 Alta | Alta |
| RF-07 | Notificación Toast de Windows | 🟠 Media | Baja |
| RF-08 | Overlay de bordes rojos en pantalla | 🟠 Media | Alta |
| RF-09 | Fase de enrollment (registro de voz del usuario) | 🔴 Alta | Alta |
| RF-10 | Configuración de horarios Día/Noche personalizados | 🟢 Baja | Baja |
| RF-11 | Historial de alertas y estadísticas | 🟢 Baja | Media |
| RF-12 | Exportación de datos (CSV) | 🟢 Baja | Baja |

### 1.3 Requisitos No Funcionales

| ID | Requisito | Métrica | Criticidad |
|:---|:----------|:--------|:-----------|
| RNF-01 | **Latencia**: Detección y alerta | < 200ms | 🔴 Crítica |
| RNF-02 | **Performance CPU**: Uso en idle | < 3% | 🔴 Crítica |
| RNF-03 | **Performance CPU**: Uso durante habla | < 8% | 🟠 Alta |
| RNF-04 | **Performance RAM**: Consumo total | < 150 MB | 🟠 Alta |
| RNF-05 | **Precisión ML**: Identificación correcta | > 95% | 🔴 Crítica |
| RNF-06 | **Falsos Positivos**: Alertas incorrectas | < 5% | 🔴 Crítica |
| RNF-07 | **Disponibilidad**: Uptime del servicio | > 99.9% | 🟠 Alta |
| RNF-08 | **Seguridad**: No almacenar audio crudo | 100% | 🔴 Crítica |
| RNF-09 | **Privacidad**: Encriptación de embeddings | 100% | 🔴 Crítica |
| RNF-10 | **Startup**: Tiempo de inicio del servicio | < 3s | 🟢 Media |

### 1.4 Requisitos de Seguridad y Privacidad

| Requisito | Justificación | Implementación |
|:----------|:--------------|:---------------|
| **No almacenar audio crudo** | El audio es PII sensible | Solo guardar embeddings vectoriales |
| **Encriptación de embeddings** | Proteger identidad vocal | AES-256 en reposo |
| **Logs sin PII** | Cumplimiento GDPR | Solo registrar timestamps y niveles de dB |
| **Permisos de micrófono** | Seguridad de Windows | Solicitar permisos explícitos |
| **Least Privilege** | Minimizar superficie de ataque | Servicio con permisos mínimos |

---

## 🏗️ 2. Decisiones de Arquitectura

### 2.1 ¿Por qué C# + .NET sobre Python/Electron?

**Decisión**: Usar C# + .NET 8 nativo

**Alternativas Consideradas**:

- Python + PyQt/Tkinter
- Electron + Node.js
- C++ nativo
- C# + .NET 8

**Justificación**:

| Criterio | Python | Electron | C++ | C# + .NET | Ganador |
|:---------|:-------|:---------|:----|:----------|:--------|
| **Performance** | ❌ Lento | ❌ Alto overhead | ✅ Excelente | ✅ Muy bueno | C++/C# |
| **Acceso bajo nivel (audio)** | ⚠️ Limitado | ❌ Difícil | ✅ Total | ✅ Bueno (P/Invoke) | C++/C# |
| **Windows Service** | ❌ Difícil | ❌ No nativo | ✅ Nativo | ✅ Nativo | C++/C# |
| **Ecosistema ML** | ✅ Excelente | ⚠️ Limitado | ⚠️ Complejo | ✅ ML.NET + ONNX | Python/C# |
| **Curva de aprendizaje** | ✅ Baja | ✅ Baja | ❌ Alta | ⚠️ Media | Python/Electron |
| **Consumo de RAM** | ✅ Bajo | ❌ Alto (>200MB) | ✅ Muy bajo | ✅ Bajo | Python/C++/C# |
| **UI nativa Windows** | ❌ No | ❌ No | ✅ Sí | ✅ WPF/WinUI | C++/C# |

**Resultado**: C# + .NET gana 5 a 3

**Contexto Específico**:

- Necesitamos **performance** para procesamiento de audio en tiempo real
- Necesitamos **acceso de bajo nivel** al micrófono de Windows
- Necesitamos crear un **Windows Service** robusto
- C# ofrece **ML.NET + ONNX** para inferencia eficiente
- WPF proporciona UI nativa y moderna

**Trade-offs**:

- ✅ **Pro**: Excelente performance, acceso nativo a Windows APIs, bajo consumo
- ❌ **Contra**: Curva de aprendizaje de C# y .NET, menos flexible que Python para ML

---

### 2.2 ¿Por qué NAudio para el manejo de Audio?

**Decisión**: Usar la librería open-source `NAudio`.

**Justificación**:

- **Madurez**: Es el estándar de facto en .NET para audio desde hace >10 años.
- **Bajo Nivel**: Permite acceso directo a **WASAPI** (Windows Audio Session API), lo cual es crítico para reducir la latencia a milisegundos.
- **Flexibilidad**: Soporta captura de audio en buffers de bytes que podemos convertir a floats para el modelo de ML sin overhead innecesario.

**Trade-offs**:

- ✅ **Pro**: Control total sobre el dispositivo de entrada y el sample rate (16kHz requerido por ML).
- ❌ **Contra**: La documentación oficial es escasa; requiere entender conceptos de DSP (Digital Signal Processing).

---

### 2.3 Arquitectura Hexagonal (Ports & Adapters)

**Decisión**: Implementar Arquitectura Hexagonal con separación clara entre UI, Servicio y Core

**Estructura del Proyecto**:

```text
VoiceVolumeTracker/
├── VoiceTracker.Domain/              # CORE (Lógica de Negocio)
│   ├── Entities/
│   │   ├── VoiceProfile.cs           # Perfil de voz del usuario
│   │   ├── VolumeReading.cs          # Lectura de volumen
│   │   ├── AlertEvent.cs             # Evento de alerta
│   │   └── AppConfig.cs              # Configuración
│   ├── ValueObjects/
│   │   ├── Decibel.cs                # Value object para dB
│   │   ├── VoiceEmbedding.cs         # Embedding de voz
│   │   └── TimeRange.cs              # Rango horario (Día/Noche)
│   ├── Repositories/
│   │   ├── IVoiceProfileRepository.cs
│   │   ├── IVolumeHistoryRepository.cs
│   │   └── IConfigRepository.cs
│   ├── Services/
│   │   ├── IAudioProcessor.cs        # Interface para procesamiento
│   │   ├── ISpeakerVerifier.cs       # Interface para ML
│   │   └── IAlertService.cs          # Interface para alertas
│   └── UseCases/
│       ├── EnrollVoiceUseCase.cs     # Registrar voz del usuario
│       ├── MonitorVolumeUseCase.cs   # Monitorear volumen
│       └── ConfigureThresholdsUseCase.cs
│
├── VoiceTracker.Infrastructure/      # ADAPTERS (Implementaciones)
│   ├── Audio/
│   │   ├── NAudioProcessor.cs        # Procesamiento con NAudio
│   │   ├── AudioCapture.cs           # Captura de micrófono
│   │   └── DecibelCalculator.cs      # Cálculo de dBFS
│   ├── ML/
│   │   ├── ONNXSpeakerVerifier.cs    # Verificación con ONNX
│   │   ├── EmbeddingExtractor.cs     # Extracción de embeddings
│   │   └── Models/
│   │       └── speechbrain_ecapa.onnx
│   ├── Persistence/
│   │   ├── SQLiteVoiceProfileRepository.cs
│   │   ├── SQLiteVolumeHistoryRepository.cs
│   │   └── JsonConfigRepository.cs
│   ├── Alerts/
│   │   ├── ToastNotificationService.cs
│   │   └── ScreenOverlayService.cs   # DirectX overlay
│   └── IPC/
│       └── NamedPipeServer.cs        # Comunicación con UI
│
├── VoiceTracker.Service/             # Windows Service
│   ├── VoiceMonitorService.cs        # Servicio principal
│   ├── FiniteStateMachine.cs         # FSM Día/Noche
│   └── BackgroundWorker.cs           # Worker en segundo plano
│
├── VoiceTracker.UI/                  # WPF Application
│   ├── Views/
│   │   ├── MainWindow.xaml           # Ventana principal
│   │   ├── EnrollmentWindow.xaml     # Wizard de enrollment
│   │   ├── ConfigWindow.xaml         # Configuración
│   │   └── StatsWindow.xaml          # Estadísticas
│   ├── ViewModels/
│   │   ├── MainViewModel.cs
│   │   ├── EnrollmentViewModel.cs
│   │   └── ConfigViewModel.cs
│   └── Services/
│       └── NamedPipeClient.cs        # Cliente IPC
│
└── VoiceTracker.Tests/
    ├── Unit/
    ├── Integration/
    └── Performance/
```

**Beneficios**:

- ✅ **Separación de responsabilidades**: UI, Servicio y Core independientes
- ✅ **Testabilidad**: Core sin dependencias de infraestructura
- ✅ **Mantenibilidad**: Cambiar NAudio por otra librería no afecta el Core
- ✅ **Escalabilidad**: Fácil agregar nuevos adaptadores (ej: alertas por email)

---

### 2.4 Finite State Machine (FSM) para Modos Día/Noche

**Decisión**: Implementar FSM para gestionar estados y transiciones

**Estados**:

1. **Idle** (Silencio detectado)
2. **Listening** (Detectando audio)
3. **Speaking** (Usuario hablando)
4. **AlertActive** (Alerta mostrada)
5. **DayMode** (Modo Día)
6. **NightMode** (Modo Noche)

**Transiciones**:

```text
Idle → Listening: Audio detectado
Listening → Speaking: Voz del usuario identificada
Speaking → AlertActive: Volumen > Umbral
AlertActive → Speaking: Volumen < Umbral
Speaking → Idle: Silencio detectado

DayMode ↔ NightMode: Cambio de horario configurado
```

**Implementación**:

```csharp
// VoiceTracker.Service/FiniteStateMachine.cs
public enum SystemState
{
    Idle,
    Listening,
    Speaking,
    AlertActive
}

public enum TimeMode
{
    Day,
    Night
}

public class VoiceTrackerFSM
{
    private SystemState _currentState = SystemState.Idle;
    private TimeMode _currentMode = TimeMode.Day;
    
    private readonly AppConfig _config;
    private readonly IAlertService _alertService;
    
    public SystemState CurrentState => _currentState;
    public TimeMode CurrentMode => _currentMode;
    
    public void ProcessAudioFrame(AudioFrame frame, bool isUserVoice, Decibel volume)
    {
        // Actualizar modo según hora
        UpdateTimeMode();
        
        // Transiciones de estado
        switch (_currentState)
        {
            case SystemState.Idle:
                if (frame.HasAudio)
                    TransitionTo(SystemState.Listening);
                break;
                
            case SystemState.Listening:
                if (isUserVoice)
                    TransitionTo(SystemState.Speaking);
                else if (!frame.HasAudio)
                    TransitionTo(SystemState.Idle);
                break;
                
            case SystemState.Speaking:
                var threshold = GetCurrentThreshold();
                if (volume > threshold)
                    TransitionTo(SystemState.AlertActive);
                else if (!isUserVoice)
                    TransitionTo(SystemState.Listening);
                break;
                
            case SystemState.AlertActive:
                var currentThreshold = GetCurrentThreshold();
                if (volume <= currentThreshold)
                    TransitionTo(SystemState.Speaking);
                break;
        }
    }
    
    private void TransitionTo(SystemState newState)
    {
        var oldState = _currentState;
        _currentState = newState;
        
        OnStateChanged(oldState, newState);
    }
    
    private void OnStateChanged(SystemState from, SystemState to)
    {
        if (to == SystemState.AlertActive)
        {
            _alertService.ShowAlert(GetCurrentThreshold());
        }
        else if (from == SystemState.AlertActive && to != SystemState.AlertActive)
        {
            _alertService.HideAlert();
        }
    }
    
    private void UpdateTimeMode()
    {
        var currentHour = DateTime.Now.Hour;
        var newMode = currentHour >= _config.NightModeStartHour || 
                      currentHour < _config.DayModeStartHour
            ? TimeMode.Night
            : TimeMode.Day;
            
        if (newMode != _currentMode)
        {
            _currentMode = newMode;
            OnTimeModeChanged(newMode);
        }
    }
    
    private Decibel GetCurrentThreshold()
    {
        return _currentMode == TimeMode.Day
            ? _config.DayThreshold
            : _config.NightThreshold;
    }
}
```

**Referencia**: [Arquitectura y Patrones](../manuales/arquitectura-patrones.md)

---

## 🎤 3. Procesamiento de Audio en Tiempo Real

### 3.1 Captura de Micrófono con NAudio

**Decisión**: Usar NAudio para captura de audio de bajo nivel

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/Audio/AudioCapture.cs
using NAudio.Wave;

public class AudioCapture : IAudioCapture
{
    private WaveInEvent _waveIn;
    private readonly int _sampleRate = 16000; // 16 kHz
    private readonly int _channels = 1; // Mono
    private readonly int _bitsPerSample = 16;
    
    public event EventHandler<AudioFrameEventArgs> AudioFrameReady;
    
    public void Start()
    {
        _waveIn = new WaveInEvent
        {
            WaveFormat = new WaveFormat(_sampleRate, _bitsPerSample, _channels),
            BufferMilliseconds = 100 // 100ms buffer
        };
        
        _waveIn.DataAvailable += OnDataAvailable;
        _waveIn.StartRecording();
    }
    
    private void OnDataAvailable(object sender, WaveInEventArgs e)
    {
        // Convertir bytes a floats
        var samples = new float[e.BytesRecorded / 2]; // bit = 2 bytes
        for (int i = 0; i < samples.Length; i++)
        {
            short sample = BitConverter.ToInt16(e.Buffer, i * 2);
            samples[i] = sample / 32768f; // Normalizar a [-1, 1]
        }
        
        var frame = new AudioFrame
        {
            Samples = samples,
            SampleRate = _sampleRate,
            Timestamp = DateTime.UtcNow
        };
        
        AudioFrameReady?.Invoke(this, new AudioFrameEventArgs(frame));
    }
    
    public void Stop()
    {
        _waveIn?.StopRecording();
        _waveIn?.Dispose();
    }
}
```

---

### 3.2 Cálculo de Decibeles (dBFS)

**Decisión**: Calcular dBFS (decibels relative to Full Scale)

**Fórmula**:

```text
RMS = sqrt(mean(samples^2))
dBFS = 20 * log10(RMS)
```

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/Audio/DecibelCalculator.cs
using System;
using System.Linq;

public class DecibelCalculator
{
    public Decibel Calculate(float[] samples)
    {
        if (samples == null || samples.Length == 0)
            return Decibel.Silence;
        
        // Calcular RMS (Root Mean Square)
        var sumOfSquares = samples.Select(s => s * s).Sum();
        var rms = Math.Sqrt(sumOfSquares / samples.Length);
        
        // Evitar log(0)
        if (rms < 1e-10)
            return Decibel.Silence;
        
        // Calcular dBFS
        var dbfs = 20 * Math.Log10(rms);
        
        return new Decibel(dbfs);
    }
    
    public bool HasVoiceActivity(float[] samples, double threshold = -40.0)
    {
        var db = Calculate(samples);
        return db.Value > threshold;
    }
}

// VoiceTracker.Domain/ValueObjects/Decibel.cs
public record Decibel
{
    public double Value { get; init; }
    
    public static Decibel Silence => new Decibel(-96.0); // Silencio digital
    
    public Decibel(double value)
    {
        if (value > 0)
            throw new ArgumentException("dBFS must be <= 0");
        Value = value;
    }
    
    public static bool operator >(Decibel a, Decibel b) => a.Value > b.Value;
    public static bool operator <(Decibel a, Decibel b) => a.Value < b.Value;
}
```

---

### 3.3 Voice Activity Detection (VAD)

**Decisión**: Implementar VAD para filtrar silencio y ruido de fondo

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/Audio/VoiceActivityDetector.cs
public class VoiceActivityDetector
{
    private readonly double _energyThreshold = -40.0; // dBFS
    private readonly double _zeroCrossingThreshold = 0.3;
    
    public bool DetectVoice(float[] samples)
    {
        // Criterio 1: Energía suficiente
        var energy = CalculateEnergy(samples);
        if (energy < _energyThreshold)
            return false;
        
        // Criterio 2: Zero-Crossing Rate (detecta habla vs ruido)
        var zcr = CalculateZeroCrossingRate(samples);
        if (zcr < _zeroCrossingThreshold)
            return false;
        
        return true;
    }
    
    private double CalculateEnergy(float[] samples)
    {
        var calculator = new DecibelCalculator();
        return calculator.Calculate(samples).Value;
    }
    
    private double CalculateZeroCrossingRate(float[] samples)
    {
        int crossings = 0;
        for (int i = 1; i < samples.Length; i++)
        {
            if ((samples[i] >= 0 && samples[i - 1] < 0) ||
                (samples[i] < 0 && samples[i - 1] >= 0))
            {
                crossings++;
            }
        }
        return (double)crossings / samples.Length;
    }
}
```

---

**Referencia**: [Optimización de Performance](../manuales/performance.md)

---

[⬆️ Volver arriba](#caso-de-estudio-voice-volume-tracker-para-windows) | [➡️ Parte 2](./voice-volume-tracker-parte2.md) | [🏠 Volver a Casos de Estudio](./README.md)
