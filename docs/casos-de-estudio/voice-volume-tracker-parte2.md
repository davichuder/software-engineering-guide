# Voice Volume Tracker - Parte 2: ML, Alertas y Testing

> Continuación del [Caso de Estudio Principal](./voice-volume-tracker-windows.md)

---

## 🤖 4. Machine Learning: Identificación de Voz (Speaker Verification)

### 4.1 ¿Por qué Speaker Verification?

**Problema**: Necesitamos diferenciar la voz del usuario de otras voces en el entorno

**Solución**: Speaker Verification usando embeddings de voz

**Alternativas Consideradas**:

- Umbral de volumen simple (rechazado: alerta con cualquier voz)
- Filtro por dirección del micrófono (rechazado: no funciona con omnidireccionales)
- Speaker Verification con ML (seleccionado)

---

### 4.2 Modelo: SpeechBrain ECAPA-TDNN

**Decisión**: Usar modelo pre-entrenado SpeechBrain ECAPA-TDNN

**Características**:

- **Arquitectura**: ECAPA-TDNN (Emphasized Channel Attention, Propagation and Aggregation Time Delay Neural Network)
- **Embedding size**: 192 dimensiones
- **Precisión**: > 98% en VoxCeleb dataset
- **Latencia**: < 50ms en CPU moderno

**Conversión a ONNX**:

```python
# scripts/convert_to_onnx.py
import torch
from speechbrain.pretrained import EncoderClassifier

# Cargar modelo pre-entrenado
classifier = EncoderClassifier.from_hparams(
    source="speechbrain/spkrec-ecapa-voxceleb",
    savedir="pretrained_models/spkrec-ecapa-voxceleb"
)

# Crear input dummy
dummy_input = torch.randn(1, 16000)  # 1 segundo de audio a 16kHz

# Exportar a ONNX
torch.onnx.export(
    classifier.mods.embedding_model,
    dummy_input,
    "speechbrain_ecapa.onnx",
    input_names=['audio'],
    output_names=['embedding'],
    dynamic_axes={
        'audio': {0: 'batch', 1: 'time'},
        'embedding': {0: 'batch'}
    }
)
```

---

### 4.3 Fase de Enrollment (Registro de Voz)

**Objetivo**: Capturar embeddings de la voz del usuario para crear un perfil

**Proceso**:

```text
1. Usuario abre wizard de enrollment
   ↓
2. Lee 5 frases diferentes (15 segundos cada una)
   ↓
3. Sistema extrae embedding de cada frase
   ↓
4. Promedia los 5 embeddings → Embedding de referencia
   ↓
5. Guarda embedding encriptado en DB
```

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/ML/EmbeddingExtractor.cs
using Microsoft.ML.OnnxRuntime;
using Microsoft.ML.OnnxRuntime.Tensors;

public class EmbeddingExtractor
{
    private readonly InferenceSession _session;
    private readonly int _sampleRate = 16000;
    
    public EmbeddingExtractor(string modelPath)
    {
        _session = new InferenceSession(modelPath);
    }
    
    public float[] Extract(float[] audioSamples)
    {
        // Preparar input tensor
        var inputTensor = new DenseTensor<float>(
            audioSamples,
            new[] { 1, audioSamples.Length }
        );
        
        var inputs = new List<NamedOnnxValue>
        {
            NamedOnnxValue.CreateFromTensor("audio", inputTensor)
        };
        
        // Ejecutar inferencia
        using var results = _session.Run(inputs);
        var embedding = results.First(r => r.Name == "embedding")
            .AsEnumerable<float>()
            .ToArray();
        
        // Normalizar embedding (L2 norm)
        return NormalizeEmbedding(embedding);
    }
    
    private float[] NormalizeEmbedding(float[] embedding)
    {
        var norm = Math.Sqrt(embedding.Sum(x => x * x));
        return embedding.Select(x => x / (float)norm).ToArray();
    }
}

// VoiceTracker.Domain/UseCases/EnrollVoiceUseCase.cs
public class EnrollVoiceUseCase
{
    private readonly IEmbeddingExtractor _extractor;
    private readonly IVoiceProfileRepository _repository;
    
    public async Task<VoiceProfile> EnrollAsync(List<float[]> audioSamples)
    {
        if (audioSamples.Count < 3)
            throw new ArgumentException("Need at least 3 samples for enrollment");
        
        // Extraer embeddings de cada muestra
        var embeddings = new List<float[]>();
        foreach (var sample in audioSamples)
        {
            var embedding = _extractor.Extract(sample);
            embeddings.Add(embedding);
        }
        
        // Promediar embeddings
        var referenceEmbedding = AverageEmbeddings(embeddings);
        
        // Crear perfil
        var profile = new VoiceProfile
        {
            Id = Guid.NewGuid(),
            ReferenceEmbedding = new VoiceEmbedding(referenceEmbedding),
            EnrollmentDate = DateTime.UtcNow,
            SampleCount = audioSamples.Count
        };
        
        await _repository.SaveAsync(profile);
        return profile;
    }
    
    private float[] AverageEmbeddings(List<float[]> embeddings)
    {
        var embeddingSize = embeddings[0].Length;
        var averaged = new float[embeddingSize];
        
        for (int i = 0; i < embeddingSize; i++)
        {
            averaged[i] = embeddings.Average(e => e[i]);
        }
        
        return averaged;
    }
}
```

---

### 4.4 Verificación en Tiempo Real

**Objetivo**: Comparar embedding actual con embedding de referencia

**Métrica**: Similitud de coseno

```text
similarity = (A · B) / (||A|| * ||B||)
```

**Umbral**: similarity > 0.75 → Es el usuario

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/ML/ONNXSpeakerVerifier.cs
public class ONNXSpeakerVerifier : ISpeakerVerifier
{
    private readonly EmbeddingExtractor _extractor;
    private readonly IVoiceProfileRepository _repository;
    private readonly double _similarityThreshold = 0.75;
    
    private VoiceProfile _currentProfile;
    
    public async Task LoadProfileAsync()
    {
        _currentProfile = await _repository.GetCurrentProfileAsync();
        if (_currentProfile == null)
            throw new InvalidOperationException("No voice profile found. Please enroll first.");
    }
    
    public bool IsUserSpeaking(float[] audioSamples)
    {
        if (_currentProfile == null)
            return false;
        
        // Extraer embedding del audio actual
        var currentEmbedding = _extractor.Extract(audioSamples);
        
        // Calcular similitud de coseno
        var similarity = CosineSimilarity(
            currentEmbedding,
            _currentProfile.ReferenceEmbedding.Values
        );
        
        return similarity >= _similarityThreshold;
    }
    
    private double CosineSimilarity(float[] a, float[] b)
    {
        if (a.Length != b.Length)
            throw new ArgumentException("Embeddings must have same length");
        
        double dotProduct = 0;
        double normA = 0;
        double normB = 0;
        
        for (int i = 0; i < a.Length; i++)
        {
            dotProduct += a[i] * b[i];
            normA += a[i] * a[i];
            normB += b[i] * b[i];
        }
        
        return dotProduct / (Math.Sqrt(normA) * Math.Sqrt(normB));
    }
}
```

---

### 4.5 Optimización de Performance

**Problema**: Inferencia ML puede ser costosa en CPU

**Soluciones**:

1. **Batching**: Procesar múltiples frames juntos
2. **Caching**: No re-calcular si el audio es similar al anterior
3. **Throttling**: Verificar cada 500ms en lugar de cada frame

```csharp
// VoiceTracker.Infrastructure/ML/OptimizedSpeakerVerifier.cs
public class OptimizedSpeakerVerifier : ISpeakerVerifier
{
    private readonly ONNXSpeakerVerifier _verifier;
    private readonly TimeSpan _verificationInterval = TimeSpan.FromMilliseconds(500);
    
    private DateTime _lastVerification = DateTime.MinValue;
    private bool _lastResult = false;
    
    public bool IsUserSpeaking(float[] audioSamples)
    {
        var now = DateTime.UtcNow;
        
        // Throttling: solo verificar cada 500ms
        if (now - _lastVerification < _verificationInterval)
            return _lastResult;
        
        _lastResult = _verifier.IsUserSpeaking(audioSamples);
        _lastVerification = now;
        
        return _lastResult;
    }
}
```

**Referencia**: [20 - Machine Learning](../manuales/machine-learning.md)

---

## 🚨 5. Sistema de Alertas Visuales

### 5.1 Notificaciones Toast de Windows

**Decisión**: Usar WinUI 3 Notifications para alertas discretas

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/Alerts/ToastNotificationService.cs
using Microsoft.Toolkit.Uwp.Notifications;

public class ToastNotificationService : IAlertService
{
    public void ShowAlert(Decibel currentVolume, Decibel threshold)
    {
        new ToastContentBuilder()
            .AddText("🔊 Volumen Alto Detectado")
            .AddText($"Tu volumen: {currentVolume.Value:F1} dB")
            .AddText($"Límite: {threshold.Value:F1} dB")
            .AddButton(new ToastButton()
                .SetContent("Entendido")
                .AddArgument("action", "dismiss"))
            .Show();
    }
    
    public void HideAlert()
    {
        ToastNotificationManagerCompat.History.Clear();
    }
}
```

---

### 5.2 Overlay de Bordes Rojos (DirectX)

**Decisión**: Crear overlay transparente con bordes rojos usando DirectX

**Características**:

- Ventana transparente fullscreen
- Siempre en primer plano (topmost)
- No bloquea interacción con otras ventanas
- Animación de fade in/out

**Implementación**:

```csharp
// VoiceTracker.Infrastructure/Alerts/ScreenOverlayService.cs
using System.Windows;
using System.Windows.Media;
using System.Windows.Media.Animation;

public class ScreenOverlayWindow : Window
{
    private readonly int _borderThickness = 10;
    
    public ScreenOverlayWindow()
    {
        // Configurar ventana
        WindowStyle = WindowStyle.None;
        AllowsTransparency = true;
        Background = Brushes.Transparent;
        Topmost = true;
        ShowInTaskbar = false;
        
        // Fullscreen
        Left = 0;
        Top = 0;
        Width = SystemParameters.PrimaryScreenWidth;
        Height = SystemParameters.PrimaryScreenHeight;
        
        // Permitir clicks a través de la ventana
        var hwnd = new WindowInteropHelper(this).Handle;
        SetWindowExTransparent(hwnd);
    }
    
    protected override void OnRender(DrawingContext dc)
    {
        base.OnRender(dc);
        
        // Dibujar bordes rojos
        var redBrush = new SolidColorBrush(Colors.Red);
        var rect = new Rect(0, 0, Width, Height);
        var pen = new Pen(redBrush, _borderThickness);
        
        dc.DrawRectangle(Brushes.Transparent, pen, rect);
    }
    
    public void ShowWithAnimation()
    {
        Opacity = 0;
        Show();
        
        var animation = new DoubleAnimation
        {
            From = 0,
            To = 1,
            Duration = TimeSpan.FromMilliseconds(300),
            EasingFunction = new QuadraticEase()
        };
        
        BeginAnimation(OpacityProperty, animation);
    }
    
    public void HideWithAnimation()
    {
        var animation = new DoubleAnimation
        {
            From = 1,
            To = 0,
            Duration = TimeSpan.FromMilliseconds(300),
            EasingFunction = new QuadraticEase()
        };
        
        animation.Completed += (s, e) => Hide();
        BeginAnimation(OpacityProperty, animation);
    }
    
    [DllImport("user32.dll")]
    private static extern int SetWindowLong(IntPtr hWnd, int nIndex, int dwNewLong);
    
    private void SetWindowExTransparent(IntPtr hwnd)
    {
        const int GWL_EXSTYLE = -20;
        const int WS_EX_TRANSPARENT = 0x00000020;
        const int WS_EX_LAYERED = 0x00080000;
        
        SetWindowLong(hwnd, GWL_EXSTYLE, WS_EX_LAYERED | WS_EX_TRANSPARENT);
    }
}

public class ScreenOverlayService : IAlertService
{
    private ScreenOverlayWindow _overlay;
    
    public void ShowAlert(Decibel currentVolume, Decibel threshold)
    {
        if (_overlay == null)
        {
            _overlay = new ScreenOverlayWindow();
        }
        
        _overlay.ShowWithAnimation();
    }
    
    public void HideAlert()
    {
        _overlay?.HideWithAnimation();
    }
}
```

---

### 5.3 Niveles de Severidad

**Decisión**: 3 niveles de alerta según cuánto se excede el umbral

| Nivel | Exceso | Color | Acción |
| :------ | :------- | :------ | :------- |
| **Advertencia** | 0-5 dB | Amarillo | Toast notification |
| **Moderado** | 5-10 dB | Naranja | Toast + Bordes finos |
| **Severo** | > 10 dB | Rojo | Toast + Bordes gruesos + Vibración |

```csharp
// VoiceTracker.Domain/ValueObjects/AlertLevel.cs
public enum AlertLevel
{
    None,
    Warning,
    Moderate,
    Severe
}

public static class AlertLevelExtensions
{
    public static AlertLevel GetLevel(Decibel current, Decibel threshold)
    {
        var excess = current.Value - threshold.Value;
        
        if (excess <= 0) return AlertLevel.None;
        if (excess <= 5) return AlertLevel.Warning;
        if (excess <= 10) return AlertLevel.Moderate;
        return AlertLevel.Severe;
    }
}
```

---

## 🧪 6. Testing Exhaustivo

### 6.1 Unit Tests para Cálculo de Decibeles

```csharp
// VoiceTracker.Tests/Unit/DecibelCalculatorTests.cs
using Xunit;
using FluentAssertions;

public class DecibelCalculatorTests
{
    private readonly DecibelCalculator _calculator = new();
    
    [Fact]
    public void Calculate_WithSilence_ReturnsSilenceLevel()
    {
        // Arrange
        var samples = new float[1000]; // All zeros
        
        // Act
        var result = _calculator.Calculate(samples);
        
        // Assert
        result.Should().Be(Decibel.Silence);
    }
    
    [Theory]
    [InlineData(0.1, -20.0)]  // Aproximadamente
    [InlineData(0.5, -6.0)]
    [InlineData(1.0, 0.0)]
    public void Calculate_WithKnownRMS_ReturnsExpectedDB(double rms, double expectedDb)
    {
        // Arrange
        var samples = Enumerable.Repeat((float)rms, 1000).ToArray();
        
        // Act
        var result = _calculator.Calculate(samples);
        
        // Assert
        result.Value.Should().BeApproximately(expectedDb, 0.5);
    }
}
```

---

### 6.2 Property-Based Testing para Audio

**Objetivo**: Generar miles de inputs aleatorios y verificar propiedades invariantes

```csharp
// VoiceTracker.Tests/Property/AudioProcessingProperties.cs
using FsCheck;
using FsCheck.Xunit;

public class AudioProcessingProperties
{
    [Property]
    public Property DecibelCalculation_ShouldNeverExceedZero()
    {
        return Prop.ForAll<float[]>(samples =>
        {
            if (samples == null || samples.Length == 0)
                return true;
            
            var calculator = new DecibelCalculator();
            var result = calculator.Calculate(samples);
            
            return result.Value <= 0;
        });
    }
    
    [Property]
    public Property VoiceActivityDetection_ShouldBeConsistent()
    {
        return Prop.ForAll<float[]>(samples =>
        {
            if (samples == null || samples.Length < 100)
                return true;
            
            var vad = new VoiceActivityDetector();
            var result1 = vad.DetectVoice(samples);
            var result2 = vad.DetectVoice(samples);
            
            return result1 == result2; // Debe ser determinístico
        });
    }
}
```

---

### 6.3 Integration Tests para FSM

```csharp
// VoiceTracker.Tests/Integration/FSMIntegrationTests.cs
public class FSMIntegrationTests
{
    [Fact]
    public async Task FSM_ShouldTransitionCorrectly_WhenVolumeExceedsThreshold()
    {
        // Arrange
        var config = new AppConfig
        {
            DayThreshold = new Decibel(-20),
            NightThreshold = new Decibel(-30)
        };
        
        var alertService = new Mock<IAlertService>();
        var fsm = new VoiceTrackerFSM(config, alertService.Object);
        
        // Act
        var frame = new AudioFrame
        {
            Samples = GenerateLoudAudio(), // -15 dB
            HasAudio = true
        };
        
        fsm.ProcessAudioFrame(frame, isUserVoice: true, new Decibel(-15));
        
        // Assert
        fsm.CurrentState.Should().Be(SystemState.AlertActive);
        alertService.Verify(a => a.ShowAlert(It.IsAny<Decibel>()), Times.Once);
    }
    
    private float[] GenerateLoudAudio()
    {
        // Generar audio con RMS = 0.2 → -14 dBFS
        return Enumerable.Repeat(0.2f, 1600).ToArray();
    }
}
```

---

### 6.4 Performance Profiling

**Objetivo**: Asegurar que el consumo de CPU/RAM esté dentro de los límites

```csharp
// VoiceTracker.Tests/Performance/PerformanceTests.cs
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

[MemoryDiagnoser]
public class AudioProcessingBenchmarks
{
    private float[] _audioSamples;
    private DecibelCalculator _calculator;
    private EmbeddingExtractor _extractor;
    
    [GlobalSetup]
    public void Setup()
    {
        _audioSamples = GenerateRandomAudio(16000); // 1 segundo
        _calculator = new DecibelCalculator();
        _extractor = new EmbeddingExtractor("speechbrain_ecapa.onnx");
    }
    
    [Benchmark]
    public Decibel CalculateDecibels()
    {
        return _calculator.Calculate(_audioSamples);
    }
    
    [Benchmark]
    public float[] ExtractEmbedding()
    {
        return _extractor.Extract(_audioSamples);
    }
    
    private float[] GenerateRandomAudio(int length)
    {
        var random = new Random(42);
        return Enumerable.Range(0, length)
            .Select(_ => (float)(random.NextDouble() * 2 - 1))
            .ToArray();
    }
}

// Ejecutar: dotnet run -c Release --project VoiceTracker.Tests
```

**Resultados Esperados**:

```text
| Method             | Mean      | Allocated |
| ------------------- | ----------: | ----------: |
| CalculateDecibels  | 15.2 μs   | 32 B      |
| ExtractEmbedding   | 45.3 ms   | 2.4 KB    |
```

**Referencia**: [04 - Testing](../manuales/testing.md)

---

[⬆️ Volver arriba](#voice-volume-tracker-parte-2-ml-alertas-y-testing) | [⬅️ Parte 1](./voice-volume-tracker-windows.md) | [➡️ Parte 3](./voice-volume-tracker-parte3.md) | [🏠 Casos de Estudio](./README.md)
