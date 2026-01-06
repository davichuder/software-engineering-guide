# Ética y Gobernanza de IA

> Ética, sesgos, fairness, explicabilidad y gobernanza en sistemas de IA.

---

## ⚖️ Introducción

**Qué:** Principios y prácticas para construir sistemas de IA éticos, justos y transparentes.

**Por qué:** IA sin ética puede amplificar sesgos, discriminar y causar daño a escala. Regulaciones (GDPR, AI Act) exigen responsabilidad.

**Cómo:** Detectar y mitigar bias, medir fairness, explicar decisiones, proteger privacy, documentar modelos.

### Principios Fundamentales

| Principio | Explicación |
|:----------|:------------|
| **Fairness** | El modelo no debe discriminar por raza, género, edad, etc. |
| **Transparency** | Las decisiones del modelo deben ser explicables |
| **Privacy** | Proteger datos personales y sensibles |
| **Accountability** | Responsabilidad clara por decisiones del modelo |
| **Safety** | El modelo no debe causar daño |

---

## 🎯 Bias en Machine Learning

### Tipos de Bias

| Tipo | Descripción | Ejemplo |
|:-----|:------------|:--------|
| **Historical Bias** | Bias en datos históricos refleja discriminación pasada | Dataset de contrataciones con más hombres en tech → modelo aprende a preferir hombres |
| **Representation Bias** | Grupos subrepresentados en datos de entrenamiento | Dataset de reconocimiento facial con 90% personas blancas → peor performance en personas de color |
| **Measurement Bias** | Features usadas como proxies para atributos protegidos | Usar código postal como proxy para raza/ingresos |
| **Aggregation Bias** | Modelo único para grupos diversos con necesidades diferentes | Modelo de diagnóstico médico entrenado solo en adultos, usado en niños |
| **Evaluation Bias** | Benchmark no representa población real | Evaluar modelo de NLP solo en inglés formal, usar en slang |

 ---

## 👮‍♂️ Gestión de Riesgo y Auditoría Humana

### Clasificación de Riesgo (EU AI Act)

 | Nivel | Descripción | Ejemplos | Requisito de Auditoría |
 |:------|:------------|:---------|:-----------------------|
 | **Riesgo Inaceptable** | Amenaza a seguridad o derechos | Social scoring, manipulación subliminal | 🛑 **Prohibido** |
 | **Alto Riesgo** | Infraestructura crítica, empleo, servicios esenciales | Auth, Crypto, Hiring, Crédito | 👮 **Auditoría Humana Obligatoria** |
 | **Riesgo Limitado** | Chatbots, deepfakes | Customer service, generación de contenido | ⚠️ **Transparencia** (avisar que es IA) |
 | **Riesgo Mínimo** | Filtros de spam, juegos | Videojuegos, filtros de correo | ✅ **Libre** (sin requisitos extra) |

### Protocolo de Auditoría Humana (Human-in-the-loop)

 Para componentes de **Alto Riesgo** (ej: Autenticación, Cifrado, Pagos):

 1. **Prohibición de Commit Directo:** IA no puede commitear a `main` sin review humano.
 2. **Review de Seguridad:** Experto humano debe validar línea por línea (no "LGTM" rápido).
 3. **Sandboxing:** Código generado corre en entorno aislado primero.
 4. **Firma Digital:** El humano firma que revisó y asume responsabilidad (Accountability).

 > [!IMPORTANT]
 > **Nunca delegues decisiones arquitectónicas críticas o de seguridad a la IA.** La IA es un copiloto, el humano es el capitán.

---

### Detección de Bias

#### 1. Análisis de Datos

```python
import pandas as pd

# Verificar balance de clases por grupo protegido
df.groupby(['protected_attribute', 'target']).size()

# Ejemplo: Dataset de crédito
#                    approved
# gender  age_group          
# female  <30           1200
#         50         3400
#         >50           2100
# male    <30           1800
#         50         4200
#         >50           2500

# ⚠️ Warning: Mujeres <30 subrepresentadas
```

---

#### 2. Fairness Metrics (ver sección siguiente)

---

#### 3. Feature Importance por Grupo

```python
from sklearn.inspection import permutation_importance

# Calcular feature importance por grupo
for group in df['protected_attribute'].unique():
    group_data = df[df['protected_attribute'] == group]
    importance = permutation_importance(model, group_data[features], group_data['target'])
    print(f"Group {group}: {importance.importances_mean}")

# ⚠️ Si features diferentes son importantes para diferentes grupos → posible bias
```

---

### Mitigación de Bias

| Técnica | Cuándo Usar | Ejemplo |
|:--------|:------------|:--------|
| **Pre-processing** | Antes de entrenar modelo | Rebalancear dataset (oversampling, undersampling, SMOTE) |
| **In-processing** | Durante entrenamiento | Agregar fairness constraints (adversarial debiasing, reweighting) |
| **Post-processing** | Después de entrenar modelo | Ajustar thresholds por grupo para igualar métricas de fairness |

---

#### Ejemplo: Rebalanceo con SMOTE

```python
from imblearn.over_sampling import SMOTE

# Rebalancear dataset por grupo protegido
smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X, y)

# Verificar balance
pd.Series(y_resampled).value_counts()
```

---

#### Ejemplo: Fairness Constraints

```python
from aif360.algorithms.inprocessing import AdversarialDebiasing

# Entrenar modelo con fairness constraint
debiased_model = AdversarialDebiasing(
    privileged_groups=[{'gender': 1}],  # Hombres
    unprivileged_groups=[{'gender': 0}],  # Mujeres
    scope_name='debiased_classifier',
    debias=True
)
debiased_model.fit(dataset)
```

---

## 📊 Fairness Metrics

### Demographic Parity

**Definición:** Probabilidad de outcome positivo debe ser igual para todos los grupos.

**Fórmula:**

```text
P(Y_pred=1 | A=0) = P(Y_pred=1 | A=1)
```

**Ejemplo:**

```python
# Calcular demographic parity
def demographic_parity(y_pred, protected_attr):
    groups = protected_attr.unique()
    rates = {}
    for group in groups:
        mask = protected_attr == group
        rates[group] = y_pred[mask].mean()
    return rates

# Ejemplo: Aprobación de crédito
# Group 0 (mujeres): 0.65
# Group 1 (hombres): 0.70
# Diferencia: 0.05 → ⚠️ Posible bias
```

**Cuándo usar:**

- Cuando queremos igualdad de oportunidades (ej: contratación, admisiones)

**Limitación:**

- Puede forzar igualdad de outcomes incluso si hay diferencias legítimas en inputs

---

### Equalized Odds

**Definición:** True positive rate y false positive rate deben ser iguales para todos los grupos.

**Fórmula:**

```text
P(Y_pred=1 | Y_true=1, A=0) = P(Y_pred=1 | Y_true=1, A=1)  # TPR
P(Y_pred=1 | Y_true=0, A=0) = P(Y_pred=1 | Y_true=0, A=1)  # FPR
```

**Ejemplo:**

```python
from sklearn.metrics import confusion_matrix

def equalized_odds(y_true, y_pred, protected_attr):
    groups = protected_attr.unique()
    metrics = {}
    for group in groups:
        mask = protected_attr == group
        tn, fp, fn, tp = confusion_matrix(y_true[mask], y_pred[mask]).ravel()
        tpr = tp / (tp + fn)
        fpr = fp / (fp + tn)
        metrics[group] = {'TPR': tpr, 'FPR': fpr}
    return metrics

# Ejemplo: Predicción de reincidencia
# Group 0: TPR=0.75, FPR=0.20
# Group 1: TPR=0.80, FPR=0.25
# ⚠️ Diferencias en TPR y FPR → posible bias
```

**Cuándo usar:**

- Cuando queremos igualdad en accuracy para todos los grupos (ej: diagnóstico médico, predicción de riesgo)

---

### Calibration

**Definición:** Entre individuos con misma probabilidad predicha, la fracción de outcomes positivos debe ser igual para todos los grupos.

**Fórmula:**

```text
P(Y_true=1 | Y_pred=p, A=0) = P(Y_true=1 | Y_pred=p, A=1)
```

**Cuándo usar:**

- Cuando las probabilidades predichas se usan para tomar decisiones (ej: scoring de crédito)

---

### Trade-offs entre Fairness Metrics

> [!WARNING]
> Es matemáticamente imposible satisfacer todas las fairness metrics simultáneamente (excepto en casos triviales).

**Ejemplo:**

- Demographic parity vs Equalized odds: Si hay diferencias reales en base rates entre grupos, no se pueden satisfacer ambas.

**Solución:**

- Elegir la métrica más apropiada para el contexto de negocio y regulaciones aplicables.

---

## 🔍 Explicabilidad (XAI)

### ¿Por Qué Explicabilidad?

| Razón | Ejemplo |
|:------|:--------|
| **Regulaciones** | GDPR "derecho a explicación" |
| **Trust** | Usuarios confían más en decisiones explicables |
| **Debugging** | Detectar errores en modelo |
| **Fairness** | Verificar que modelo no usa features discriminatorias |

---

### SHAP (SHapley Additive exPlanations)

**Qué hace:** Explica contribución de cada feature a la predicción.

**Ejemplo:**

```python
import shap

# Entrenar modelo
model = XGBClassifier()
model.fit(X_train, y_train)

# Calcular SHAP values
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)

# Visualizar
shap.summary_plot(shap_values, X_test)
```

**Interpretación:**

- Features con SHAP values positivos → aumentan probabilidad de clase positiva
- Features con SHAP values negativos → disminuyen probabilidad

---

### LIME (Local Interpretable Model-agnostic Explanations)

**Qué hace:** Explica predicciones individuales aproximando modelo complejo con modelo simple local.

**Ejemplo:**

```python
from lime.lime_tabular import LimeTabularExplainer

# Crear explainer
explainer = LimeTabularExplainer(
    X_train.values,
    feature_names=X_train.columns,
    class_names=['rejected', 'approved'],
    mode='classification'
)

# Explicar predicción individual
exp = explainer.explain_instance(
    X_test.iloc[0].values,
    model.predict_proba
)
exp.show_in_notebook()
```

---

### Feature Importance

**Qué hace:** Ranking de features por importancia global.

**Ejemplo:**

```python
from sklearn.inspection import permutation_importance

# Calcular permutation importance
result = permutation_importance(model, X_test, y_test, n_repeats=10)

# Visualizar
import matplotlib.pyplot as plt
sorted_idx = result.importances_mean.argsort()
plt.barh(X_test.columns[sorted_idx], result.importances_mean[sorted_idx])
plt.xlabel("Permutation Importance")
```

---

## 🔐 Privacy

### Differential Privacy

**Qué es:** Garantía matemática de que agregar/remover un individuo del dataset no cambia significativamente el output.

**Ejemplo:**

```python
from diffprivlib.models import LogisticRegression

# Entrenar modelo con differential privacy
model = LogisticRegression(epsilon=1.0)  # Privacy budget
model.fit(X_train, y_train)

# Modelo entrenado sin ver datos individuales directamente
```

**Trade-off:**

- ✅ Privacy garantizada matemáticamente
- ❌ Accuracy reducida (más privacy → menos accuracy)

---

### Federated Learning

**Qué es:** Entrenar modelo sin centralizar datos (modelo viaja a los datos, no al revés).

**Ejemplo:**

```python
# Pseudocódigo
# Servidor central
global_model = initialize_model()

for round in range(num_rounds):
    # Enviar modelo a clientes
    for client in clients:
        local_model = client.train(global_model, local_data)
        send_to_server(local_model.weights)
    
    # Agregar pesos locales
    global_model.weights = average(all_local_weights)
```

**Ventajas:**

- ✅ Datos nunca salen del dispositivo del usuario
- ✅ Compliance con GDPR, HIPAA

**Desafíos:**

- ❌ Heterogeneidad de datos entre clientes
- ❌ Comunicación costosa

---

### Data Anonymization

**Técnicas:**

| Técnica | Descripción | Ejemplo |
|:--------|:------------|:--------|
| **K-anonymity** | Cada registro es indistinguible de al menos k-1 otros | Generalizar edad (25 → 30) |
| **L-diversity** | Cada grupo de k registros tiene al menos L valores distintos en atributos sensibles | Grupo de 5 personas con 3 diagnósticos diferentes |
| **T-closeness** | Distribución de atributos sensibles en cada grupo es similar a distribución global | Distribución de salarios en grupo similar a distribución general |

---

## 📋 Gobernanza

### Model Cards

**Qué es:** Documentación estandarizada de modelos ML.

**Contenido:**

- **Model Details**: Tipo, arquitectura, versión
- **Intended Use**: Para qué fue diseñado, limitaciones
- **Factors**: Variables que afectan performance (demografía, contexto)
- **Metrics**: Accuracy, fairness metrics
- **Training Data**: Descripción, fuentes, bias conocidos
- **Ethical Considerations**: Riesgos, mitigaciones

**Ejemplo:**

```markdown
# Model Card: Credit Approval Model

## Model Details
- Type: XGBoost Classifier
- Version: 2.1.0
- Trained: 2015

## Intended Use
- Primary use: Approve/reject credit applications
- Out-of-scope: Not for use in countries with different regulations

## Factors
- Evaluated across gender, age, income brackets
- Performance varies by age group (lower accuracy for <25)

## Metrics
- Overall Accuracy: 0.87
- Demographic Parity Difference: 0.03
- Equalized Odds Difference: 0.05

## Training Data
- Source: Internal credit applications 202023
- Size: 500K applications
- Known bias: Underrepresentation of applicants <25

## Ethical Considerations
- Risk: May perpetuate historical bias in credit approval
- Mitigation: Applied SMOTE rebalancing, fairness constraints
```

---

### Datasheets for Datasets

**Qué es:** Documentación estandarizada de datasets.

**Contenido:**

- **Motivation**: Por qué se creó el dataset
- **Composition**: Qué contiene, cuántos ejemplos, missing data
- **Collection Process**: Cómo se recolectaron los datos
- **Preprocessing**: Qué transformaciones se aplicaron
- **Uses**: Para qué se puede/no se puede usar
- **Distribution**: Cómo se distribuye, licencia
- **Maintenance**: Quién mantiene, cómo reportar errores

---

### AI Ethics Boards

**Qué es:** Comité que revisa proyectos de IA para identificar riesgos éticos.

**Composición:**

- Técnicos (ML engineers, data scientists)
- Expertos en ética
- Legal/compliance
- Representantes de grupos afectados

**Proceso:**

1. **Submission**: Equipo envía propuesta de proyecto de IA
2. **Review**: Ethics board evalúa riesgos (bias, privacy, safety)
3. **Decision**: Aprobar, aprobar con condiciones, rechazar
4. **Monitoring**: Seguimiento post-deployment

---

## 📋 Artefactos

### Bias Detection Checklist

- [ ] **Representatividad**: ¿Todos los grupos están representados proporcionalmente?
- [ ] **Balance**: ¿Las clases están balanceadas por grupo protegido?
- [ ] **Proxies**: ¿Hay features que son proxies para atributos protegidos? (ej: código postal → raza)
- [ ] **Historical bias**: ¿Los datos históricos reflejan discriminación pasada?
- [ ] **Measurement bias**: ¿Las features se miden de forma consistente para todos los grupos?
- [ ] **Evaluation**: ¿El benchmark representa la población real?

---

### Model Card Template

```markdown
# Model Card: [Model Name]

## Model Details
- **Type**: [Classification/Regression/etc]
- **Architecture**: [XGBoost/Neural Network/etc]
- **Version**: [1.0.0]
- **Trained**: [YYYY-MM-DD]
- **Owner**: [Team/Person]

## Intended Use
- **Primary use**: [Description]
- **Out-of-scope uses**: [What NOT to use for]
- **Users**: [Who should use this model]

## Factors
- **Groups**: [Demographic groups evaluated]
- **Instrumentation**: [How data was collected]
- **Environment**: [Deployment environment]

## Metrics
- **Overall Accuracy**: [0.XX]
- **Precision/Recall**: [0.XX / 0.XX]
- **Fairness Metrics**:
  - Demographic Parity Difference: [0.XX]
  - Equalized Odds Difference: [0.XX]

## Training Data
- **Source**: [Where data came from]
- **Size**: [N examples]
- **Timeframe**: [YYYY-MM to YYYY-MM]
- **Known Bias**: [Describe bias]

## Ethical Considerations
- **Risks**: [Potential harms]
- **Mitigations**: [What was done to reduce risks]
- **Limitations**: [Known limitations]

## Caveats and Recommendations
- [Caveat 1]
- [Caveat 2]
```

---

### Ethical Review Template

```markdown
# Ethical Review: [Project Name]

## Project Overview
- **Description**: [What the AI system does]
- **Stakeholders**: [Who is affected]
- **Impact**: [Expected impact]

## Risk Assessment

### Bias Risk
- **Risk Level**: [Low/Medium/High]
- **Description**: [Potential bias]
- **Mitigation**: [How to address]

### Privacy Risk
- **Risk Level**: [Low/Medium/High]
- **Description**: [Privacy concerns]
- **Mitigation**: [How to protect privacy]

### Safety Risk
- **Risk Level**: [Low/Medium/High]
- **Description**: [Potential harm]
- **Mitigation**: [Safety measures]

### Fairness Risk
- **Risk Level**: [Low/Medium/High]
- **Description**: [Fairness concerns]
- **Mitigation**: [Fairness interventions]

## Decision
- [ ] Approved
- [ ] Approved with conditions: [List conditions]
- [ ] Rejected: [Reason]

## Monitoring Plan
- **Metrics to track**: [Fairness metrics, performance metrics]
- **Frequency**: [Weekly/Monthly]
- **Responsible**: [Team/Person]
```

---

## 📚 Recursos

- [AI Fairness 360 (IBM)](https://aif360.mybluemix.net/)
- [Fairlearn (Microsoft)](https://fairlearn.org/)
- [Model Cards Paper (Google)](https://arxiv.org/abs/1810.03993)
- [Datasheets for Datasets (Microsoft)](https://arxiv.org/abs/1803.09010)
- [GDPR Right to Explanation](https://gdpr-info.eu/)
