# Ciencia de Datos

> Extraer conocimiento y valor de datos mediante estadística, visualización y análisis exploratorio.

---

## 📊 Ciencia de Datos

**Qué:** Disciplina que combina estadística, programación y conocimiento del dominio para extraer insights de datos.

**Por qué:** Tomar decisiones data-driven, descubrir patrones, validar hipótesis.

**Quién:** Data Scientists, Data Analysts, Business Intelligence.

**Esfuerzo:** ROI variable según caso, requiere datos de calidad y stakeholder buy-in.

---

## 🔄 Workflow Data Science

```text
1. Problem Framing (¿Qué queremos saber?)
   ↓
2. Data Collection (¿Dónde están los datos?)
   ↓
3. Data Cleaning (¿Son datos limpios?)
   ↓
4. EDA (¿Qué nos dicen?)
   ↓
5. Analysis/Modeling (¿Cómo responder la pregunta?)
   ↓
6. Communication (¿Cómo presentar insights?)
   ↓
7. Action (¿Qué decisión tomar?)
```

---

## 🧹 Data Cleaning (Limpieza)

**Qué:** Preparar datos crudos para análisis.

**Por qué:** "Garbage in, garbage out" - datos sucios = análisis inválido.

| Problema | Qué | Solución | Herramientas |
| :--------- | :----- | :--------- | :------------- |
| **Valores faltantes** | NaN, NULL, vacíos | Imputar (media, mediana), eliminar, flag | [pandas](https://pandas.pydata.org/) `fillna()`, `dropna()` |
| **Duplicados** | Registros repetidos | Eliminar con lógica | `drop_duplicates()` |
| **Outliers** | Valores extremos | Investigar, eliminar o transformar | IQR, Z-score, visualización |
| **Inconsistencias** | "USA" vs "United States" | Estandarizar | Regex, `replace()` |
| **Tipos incorrectos** | Fechas como string | Convertir tipos | `pd.to_datetime()`, `astype()` |
| **Formato** | Espacios, mayúsculas | Normalizar | `str.strip()`, `str.lower()` |

**Herramientas:** [pandas](https://pandas.pydata.org/), [Polars](https://pola.rs/), [pyjanitor](https://pyjanitor-devs.github.io/pyjanitor/)

---

## 🔍 EDA (Exploratory Data Analysis)

**Qué:** Entender datos mediante estadística y visualización.

**Por qué:** Encontrar patrones, anomalías, formular hipótesis.

### Análisis Univariado

| Tipo Variable | Métricas | Visualización |
| :-------------- | :--------- | :-------------- |
| **Numérica** | Mean, median, std, min, max, percentiles | Histogram, boxplot, density plot |
| **Categórica** | Frecuencias, moda | Bar chart, pie chart |

### Análisis Bivariado

| Combinación | Análisis | Visualización |
| :------------ | :--------- | :-------------- |
| **Num vs Num** | Correlación (Pearson, Spearman) | Scatter plot, heatmap |
| **Cat vs Num** | Comparar distribuciones | Boxplot, violin plot |
| **Cat vs Cat** | Tablas de contingencia, chi-squared | Heatmap, stacked bars |

### Análisis Multivariado

| Técnica | Qué | Herramienta |
| :-------- | :----- | :------------ |
| **PCA** | Reducción dimensionalidad | scikit-learn |
| **t-SNE** | Visualizar high-dim data | scikit-learn |
| **Correlation Matrix** | Relaciones entre variables | seaborn heatmap |

---

## 📈 Visualización

**Qué:** Representar datos gráficamente.

**Por qué:** "Un gráfico vale más que mil tablas".

| Tipo | Cuándo | Herramienta |
| :----- | :----- | :------------ |
| **Estática** | Reportes, papers | [Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/) |
| **Interactiva** | Dashboards, exploración | [Plotly](https://plotly.com/python/), [Altair](https://altair-viz.github.io/) |
| **Dashboards** | Apps analíticas | [Dash](https://plotly.com/dash/), [Streamlit](https://streamlit.io/) |
| **BI Tools** | Business users | [Tableau](https://www.tableau.com/), [Power BI](https://powerbi.microsoft.com/), [Looker](https://cloud.google.com/looker) |

### Tipos de Gráficos

| Gráfico | Cuándo | Ejemplo |
| :-------- | :----- | :-------- |
| **Line** | Series temporales | Ventas por mes |
| **Bar** | Comparar categorías | Ventas por región |
| **Scatter** | Relación 2 variables | Precio vs tamaño |
| **Histogram** | Distribución | Distribución edades |
| **Boxplot** | Distribución + outliers | Salarios por departamento |
| **Heatmap** | Correlaciones, matrices | Matriz de correlación |
| **Pie** | Proporciones (evitar) | Market share |

---

## 📊 Estadística

### Descriptiva

| Métrica | Qué | Cuándo |
| :-------- | :----- | :------- |
| **Media** | Promedio | Distribución normal |
| **Mediana** | Valor medio | Outliers presentes |
| **Moda** | Más frecuente | Variables categóricas |
| **Std Dev** | Dispersión | Cuantificar variabilidad |
| **Percentiles** | Posición en distribución | Benchmarking |

### Inferencial

| Concepto | Qué | Herramienta |
| :--------- | :----- | :------------ |
| **Hypothesis Testing** | Validar suposiciones | t-test, chi-squared |
| **p-value** | Probabilidad resultado por azar | <0.05 = significativo |
| **Confidence Intervals** | Rango valores probables | Bootstrap, t-distribution |
| **A/B Testing** | Comparar variantes | scipy.stats |

---

## 🔬 Reproducibilidad

**Qué:** Capacidad de replicar análisis.

**Por qué:** Ciencia requiere verificabilidad.

| Aspecto | Cómo | Herramientas |
| :-------- | :---- | :------------- |
| **Versionado datos** | Trackear cambios en datasets | [DVC](https://dvc.org/), [Git LFS](https://git-lfs.github.com/) |
| **Versionado código** | Git para notebooks y scripts | Git, GitHub |
| **Environments** | Aislar dependencias | [conda](https://docs.conda.io/), [venv](https://docs.python.org/3/library/venv.html), [Docker](https://www.docker.com/) |
| **Notebooks parametrizados** | Ejecutar con distintos params | [Papermill](https://github.com/nteract/papermill) |
| **Seeds** | Reproducir aleatoriedad | `np.random.seed(42)` |
| **Documentation** | Documentar decisiones | Markdown, docstrings |

---

## 🧮 Herramientas Core

| Herramienta | Qué | Cuándo |
| :------------ | :----- | :----- |
| [pandas](https://pandas.pydata.org/) | Manipulación tabular | Default para análisis |
| [NumPy](https://numpy.org/) | Cálculo numérico | Operaciones matriciales |
| [Polars](https://pola.rs/) | Pandas más rápido | Datasets grandes (>1GB) |
| [Dask](https://www.dask.org/) | Parallel computing | Datos que no caben en RAM |
| [Jupyter](https://jupyter.org/) | Notebooks interactivos | Exploración, prototipado |
| [VS Code](https://code.visualstudio.com/) | IDE con notebook support | Desarrollo productivo |

---

## 📊 Tipos de Análisis

| Tipo | Qué | Pregunta | Técnica |
| :----- | :----- | :--------- | :-------- |
| **Descriptivo** | ¿Qué pasó? | Métricas históricas | Aggregations, visualización |
| **Diagnóstico** | ¿Por qué pasó? | Causas | Correlaciones, comparaciones |
| **Predictivo** | ¿Qué pasará? | Forecast | Machine Learning, time series |
| **Prescriptivo** | ¿Qué hacer? | Recomendaciones | Optimization, simulación |

---

## 🔄 Data Quality

| Dimensión | Qué | Cómo validar |
| :---------- | :----- | :------------- |
| **Completitud (Completeness)** | Sin valores faltantes | `df.isnull().sum()` |
| **Unicidad (Uniqueness)** | Sin duplicados | `df.duplicated().sum()` |
| **Consistencia (Consistency)** | Valores válidos | Regex, value ranges |
| **Exactitud (Accuracy)** | Datos correctos | Validar con fuentes |
| **Actualidad (Timeliness)** | Datos actuales | Timestamps |

**Herramientas:** [Great Expectations](https://greatexpectations.io/), [Pandera](https://pandera.readthedocs.io/)

---

## 🎯 Métricas de Negocio

| Métrica | Qué | Fórmula |
| :-------- | :----- | :-------- |
| **Churn Rate** | % clientes que abandonan | Churned / Total × 100 |
| **CAC** | Customer Acquisition Cost | Marketing Spend / New Customers |
| **LTV** | Lifetime Value | Avg Revenue per User × Avg Lifetime |
| **Conversion Rate** | % que completan acción | Conversions / Visitors × 100 |
| **AOV** | Average Order Value | Revenue / Orders |

---

## 🚫 Errores Comunes

| Error | Problema | Solución |
| :------ | :--------- | :--------- |
| **Correlation = Causation** | Confundir relación con causa | Experimentos, domain knowledge |
| **P-hacking** | Buscar hasta encontrar p<0.05 | Hipótesis a priori, correction |
| **Confirmation Bias** | Buscar solo evidencia que confirme | Buscar evidencia contradictoria |
| **Simpson's Paradox** | Tendencia se invierte al agregar | Estratificar análisis |
| **Survivorship Bias** | Solo analizar sobrevivientes | Incluir todos los casos |

---

## 📚 Recursos

- [Python for Data Analysis - Wes McKinney](https://wesmckinney.com/book/)
- [Storytelling with Data - Cole Nussbaumer](https://www.storytellingwithdata.com/)
- [R for Data Science](https://r4ds.hadley.nz/)
- [Kaggle Datasets](https://www.kaggle.com/datasets)
