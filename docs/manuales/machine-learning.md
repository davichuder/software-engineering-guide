# Machine Learning y Deep Learning

> Sistemas que aprenden de datos para hacer predicciones, clasificaciones y generación de contenido.

---

## 🤖 Machine Learning

**Qué:** Algoritmos que mejoran con experiencia sin ser programados explícitamente.

**Por qué:** Automatizar decisiones, encontrar patrones, personalización.

**Quién:** Data Scientists, ML Engineers, Research Engineers.

**Esfuerzo:** Alta inversión inicial (datos, entrenamiento), ROI variable según caso de uso.

---

## 📊 Tipos de Aprendizaje

| Tipo | Qué | Cuándo | Algoritmos |
|:-----|:-----|:-----|:-----------|
| **Supervisado** | Aprender de datos etiquetados | Clasificación, regresión | Linear Regression, Random Forest, XGBoost, SVM |
| **No Supervisado** | Encontrar patrones sin etiquetas | Clustering, reducción dimensionalidad | K-Means, DBSCAN, PCA, t-SNE |
| **Semi-Supervisado** | Pocos labels + muchos unlabeled | Etiquetado costoso | Self-training, co-training |
| **Reinforcement Learning** | Aprender por reward/penalty | Gaming, robótica, trading | Q-Learning, DQN, PPO, A3C |

---

## 🧠 Algoritmos Clásicos (ML)

| Algoritmo | Qué | Cuándo | Pros/Contras |
|:----------|:-----|:-----|:----------|
| **Linear Regression** | Predecir valor continuo | Relación lineal | ✅ Simple, interpretable; ❌ Solo lineal |
| **Logistic Regression** | Clasificación binaria | Baseline classification | ✅ Rápido, probabilístico; ❌ Solo linealmente separable |
| **Decision Trees** | Árbol de decisiones | Interpretabilidad | ✅ Fácil explicar; ❌ Overfitting |
| **Random Forest** | Ensemble de árboles | Clasificación/regresión robusta | ✅ Reduce overfitting; ❌ Menos interpretable |
| **XGBoost** | Gradient boosting optimizado | Competencias Kaggle | ✅ SOTA en tabular; ❌ Tuning complejo |
| **SVM** | Support Vector Machines | Clasificación con kernel | ✅ Efectivo high-dim; ❌ Lento en big data |
| **K-Means** | Clustering por centroides | Segmentación clientes | ✅ Simple; ❌ Requiere K predefinido |
| **PCA** | Reducción dimensionalidad | Visualización, preprocessing | ✅ Elimina colinealidad; ❌ Pierde interpretabilidad |

**Herramientas:** [scikit-learn](https://scikit-learn.org/), [XGBoost](https://xgboost.readthedocs.io/), [LightGBM](https://lightgbm.readthedocs.io/)

---

## 🧠 Deep Learning

**Qué:** Redes neuronales con múltiples capas.

**Por qué:** Aprende representaciones complejas, SOTA en visión, NLP, audio.

| Arquitectura | Qué | Cuándo | Casos de Uso |
|:-------------|:-----|:-----|:----------|
| **CNN** (Convolutional) | Redes para datos espaciales | Imágenes, video | Clasificación imágenes, detección objetos |
| **RNN** (Recurrent) | Redes para secuencias | Series temporales, texto | Predicción series, sentiment analysis |
| **LSTM** (Long Short-Term Memory) | RNN con memoria long-term | Secuencias largas | Traducción, generación texto |
| **Transformer** | Attention mechanism | NLP moderno | BERT, GPT, traducción |
| **GAN** (Generative Adversarial) | Generador vs Discriminador | Generar imágenes realistas | Deepfakes, data augmentation |
| **Autoencoder** | Encoder-Decoder | Reducción dimensionalidad, denoising | Compresión, anomalías |
| **Diffusion Models** | Generación iterativa | Generación imagen SOTA | DALL-E, Stable Diffusion |

**Frameworks:** [PyTorch](https://pytorch.org/), [TensorFlow](https://www.tensorflow.org/), [Keras](https://keras.io/), [JAX](https://github.com/google/jax)

---

## 🗣️ NLP (Natural Language Processing)

| Tarea | Qué | Modelos |
|:------|:-----|:--------|
| **Clasificación texto** | Categorizar documentos | BERT, RoBERTa, DistilBERT |
| **NER** (Named Entity Recognition) | Extraer entidades | spaCy, Flair |
| **Sentiment Analysis** | Detectar sentimiento | Fine-tuned BERT |
| **Traducción** | Traducir entre idiomas | mT5, MarianMT |
| **Question Answering** | Responder preguntas | BERT-QA, T5 |
| **Text Generation** | Generar texto coherente | GPT-4, Claude, LLaMA |
| **Embeddings** | Vectorizar texto | Word2Vec, GloVe, BERT embeddings |

**Herramientas:** [Hugging Face](https://huggingface.co/), [spaCy](https://spacy.io/), [LangChain](https://www.langchain.com/)

---

## 👁️ Computer Vision

| Tarea | Qué | Modelos |
|:------|:-----|:--------|
| **Clasificación** | Etiquetar imagen | ResNet, EfficientNet, Vision Transformer |
| **Detección objetos** | Ubicar objetos en imagen | YOLO, Faster R-CNN |
| **Segmentación** | Clasificar cada pixel | U-Net, Mask R-CNN |
| **Pose Estimation** | Detectar posición humana | OpenPose, MediaPipe |
| **OCR** | Extraer texto de imagen | Tesseract, EasyOCR |
| **Face Recognition** | Identificar personas | FaceNet, ArcFace |

**Herramientas:** [OpenCV](https://opencv.org/), [YOLO](https://github.com/ultralytics/ultralytics), [MediaPipe](https://mediapipe.dev/)

---

## 🔄 MLOps

**Qué:** DevOps para Machine Learning (automatizar pipeline ML).

**Por qué:** Reproducibilidad, deployment continuo, monitoreo modelos.

| Fase | Qué | Herramientas |
|:-----|:-----|:-------------|
| **Experiment Tracking** | Registrar entrenamientos | [MLflow](https://mlflow.org/), [Weights & Biases](https://wandb.ai/) |
| **Feature Store** | Centralizar features | [Feast](https://feast.dev/), [Tecton](https://www.tecton.ai/) |
| **Model Registry** | Versionar modelos | MLflow, [DVC](https://dvc.org/) |
| **Training** | Pipeline entrenamiento | [Kubeflow](https://www.kubeflow.org/), [SageMaker](https://aws.amazon.com/sagemaker/) |
| **Serving** | Deploy modelos | [TorchServe](https://pytorch.org/serve/), [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving) |
| **Monitoring** | Detectar drift | [Evidently](https://www.evidentlyai.com/), [WhyLabs](https://whylabs.ai/) |

---

## 📊 Pipeline ML

```text
1. Problem Definition
   ↓
2. Data Collection
   ↓
3. Data Cleaning & EDA
   ↓
4. Feature Engineering
   ↓
5. Model Training
   ↓
6. Evaluation (test set)
   ↓
7. Hyperparameter Tuning
   ↓
8. Deployment
   ↓
9. Monitoring & Retraining
```

---

## 🎯 Evaluación de Modelos

### Clasificación

| Métrica | Qué | Cuándo |
|:--------|:-----|:-----|
| **Accuracy** | % correctos | Clases balanceadas |
| **Precision** | % de positivos correctos | Minimizar falsos positivos |
| **Recall** | % de positivos encontrados | Minimizar falsos negativos |
| **F1-Score** | Media armónica precision/recall | Balance |
| **ROC-AUC** | Área bajo curva ROC | Evaluar probabilidades |
| **Confusion Matrix** | Visualizar errores | Entender errores |

### Regresión

| Métrica | Qué | Cuándo |
|:--------|:-----|:-----|
| **MAE** | Mean Absolute Error | Interpretar error promedio |
| **MSE** | Mean Squared Error | Penalizar errores grandes |
| **RMSE** | Root MSE | Misma escala que target |
| **R²** | Varianza explicada | Comparar modelos |

---

## 🔧 Feature Engineering

| Técnica | Qué | Ejemplo |
|:--------|:-----|:--------|
| **Encoding** | Convertir categóricas | One-hot, label encoding |
| **Scaling** | Normalizar rangos | StandardScaler, MinMaxScaler |
| **Binning** | Discretizar continuas | Edad → grupos etarios |
| **Interactions** | Combinar features | precio * cantidad |
| **Time Features** | Extraer de fechas | día_semana, mes, hora |
| **Text Features** | De texto a números | TF-IDF, embeddings |

---

## ⚙️ Optimización Hiperparámetros

| Método | Qué | Cuándo |
|:-------|:-----|:-----|
| **Grid Search** | Probar todas combinaciones | Pocos hiperparámetros |
| **Random Search** | Sampling aleatorio | Más hiperparámetros |
| **Bayesian Optimization** | Optimización inteligente | Entrenamientos costosos |
| **Optuna** | AutoML hyperparameter tuning | Automatizar búsqueda |

**Herramientas:** [Optuna](https://optuna.org/), [Ray Tune](https://docs.ray.io/en/latest/tune/index.html), [Hyperopt](http://hyperopt.github.io/hyperopt/)

---

## 🧪 Validación

| Técnica | Qué | Cuándo |
|:--------|:-----|:-----|
| **Train/Test Split** | 20 o 30 | Dataset suficientemente grande |
| **K-Fold CV** | K subsets, entrenar K veces | Datasets pequeños |
| **Stratified K-Fold** | Mantener proporción clases | Clases desbalanceadas |
| **Time Series Split** | No mezclar temporal | Series temporales |

---

## 🚫 Problemas Comunes

| Problema | Causa | Solución |
|:---------|:------|:---------|
| **Overfitting** | Modelo memoriza training | Regularization, más datos, dropout |
| **Underfitting** | Modelo muy simple | Más features, modelo más complejo |
| **Fuga de Datos (Data Leakage)** | Info de test en training | Validar splits, feature engineering post-split |
| **Imbalanced Classes** | 99% clase A, 1% clase B | Oversampling (SMOTE), class weights |
| **Concept Drift** | Distribución cambia en prod | Retraining periódico, monitoring |

---

## 🔐 Ética y Fairness

| Aspecto | Qué | Cómo mitigar |
|:--------|:-----|:-------------|
| **Bias** | Modelo discrimina grupos | Auditar datasets, fairness metrics |
| **Privacy** | Datos sensibles | Differential privacy, federated learning |
| **Explainability** | Black-box decisions | SHAP, LIME, feature importance |
| **Transparency** | Comunicar limitaciones | Documentar asunciones, model cards |

**Herramientas:** [SHAP](https://shap.readthedocs.io/), [LIME](https://github.com/marcotcr/lime), [Fairlearn](https://fairlearn.org/)

---

## 📚 Recursos

- [fast.ai](https://www.fast.ai/)
- [Deep Learning Book - Goodfellow](https://www.deeplearningbook.org/)
- [Kaggle Learn](https://www.kaggle.com/learn)
- [Papers with Code](https://paperswithcode.com/)
- [Hugging Face Course](https://huggingface.co/learn)
