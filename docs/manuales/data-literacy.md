# Data Literacy

> Democratización de datos y data literacy para empoderar a equipos no técnicos.

---

## 📊 Fundamentos de Data Literacy

### Qué es Data Literacy

**Definición:** Capacidad de leer, trabajar con, analizar y comunicar con datos.

**Componentes:**

1. **Leer datos**: Interpretar gráficos, tablas, métricas
2. **Trabajar con datos**: Filtrar, agrupar, calcular
3. **Analizar datos**: Identificar patterns, correlaciones, outliers
4. **Comunicar con datos**: Presentar insights de forma clara

---

### Conceptos Clave

#### Correlación vs Causalidad

**Correlación:** Dos variables cambian juntas.
**Causalidad:** Una variable causa el cambio en otra.

**Ejemplo:**

- **Correlación**: Ventas de helado ↑ cuando ahogamientos ↑
- **NO causalidad**: Helado no causa ahogamientos
- **Variable oculta**: Temperatura (verano → más helado Y más gente en playas)

**Regla de oro:** Correlación ≠ Causalidad

---

#### Tipos de Métricas

| Tipo | Descripción | Ejemplo |
|:-----|:------------|:--------|
| **Counter** | Siempre incrementa | Total de ventas, usuarios registrados |
| **Gauge** | Puede subir o bajar | Usuarios activos ahora, temperatura |
| **Rate** | Cambio por unidad de tiempo | Ventas/día, requests/segundo |
| **Ratio** | Proporción entre dos valores | Conversion rate, churn rate |

---

#### Percentiles

**Qué son:** Valor por debajo del cual cae un % de observaciones.

| Percentil | Significado |
|:----------|:------------|
| **p50 (mediana)** | 50% de valores están por debajo |
| **p95** | 95% de valores están por debajo (detecta outliers) |
| **p99** | 99% de valores están por debajo (tail latency) |

**Por qué p99 > promedio:**

- Promedio puede ser engañoso (outliers lo afectan mucho)
- p99 muestra experiencia del 1% peor (usuarios frustrados)

**Ejemplo:**

```text
Latencies: [10ms, 12ms, 15ms, 18ms, 20ms, 25ms, 30ms, 50ms, 100ms, 500ms]

Promedio: 78ms
p50: 22.5ms
p95: 100ms
p99: 500ms

→ Promedio no representa bien la experiencia (mayoría ve <25ms)
→ p99 muestra que 1% de usuarios ve 500ms (problema!)
```

---

## 🔧 Analítica Autoservicio

### Herramientas

| Tool | Pros | Cons | Cuándo Usar |
|:-----|:-----|:-----|:------------|
| **Looker** | Modeling layer (LookML), embeddable | Curva de aprendizaje | Empresas, data teams |
| **Tableau** | Visualizaciones poderosas, drag-and-drop | Costo alto | Business analysts |
| **Metabase** | Open-source, fácil de usar | Menos features | Startups, equipos pequeños |
| **Superset** | Open-source, flexible | Requiere setup | Data teams técnicos |

---

### Best Practices

#### 1. Definir Métricas Clave

**Ejemplo:**

````markdown
# Métrica: Monthly Active Users (MAU)

**Definición:** Usuarios únicos que hicieron login en los últimos 30 días

**SQL:**
```sql
SELECT COUNT(DISTINCT user_id)
FROM user_events
WHERE event_type = 'login'
  AND event_date >= CURRENT_DATE - INTERVAL '30 days'
```

**Owner:** Product team
**Actualización:** Diaria
**Alerta:** Si MAU cae >10% week-over-week

````

---

#### 2. Data Governance

**Quién puede acceder a qué:**

| Rol | Acceso | Ejemplo |
|:----|:-------|:--------|
| **Executives** | Dashboards agregados | Revenue, MAU, churn |
| **Product Managers** | Métricas de producto | Feature adoption, NPS |
| **Engineers** | Logs, traces, performance | Latency, error rate |
| **Sales** | CRM data | Leads, pipeline, deals |

**Principio:** Least privilege (solo acceso necesario)

---

## 📖 Narrativa con Datos

### Estructura

```text

1. Hook
   ↓
2. Context
   ↓
3. Insight
   ↓
4. Action (Acción)

```

---

### Ejemplo: Presentar Insights a Stakeholders

**❌ MAL (solo números):**

```text

"Conversion rate es 2.5%"

```

**✅ BIEN (storytelling):**

```text

**Hook:** Estamos perdiendo $50K/mes en revenue.

**Context:** Nuestro conversion rate es 2.5%, mientras que el benchmark de la industria es 4%.

**Insight:** Identificamos que el 60% de usuarios abandonan en el paso de pago.
Análisis cualitativo (user interviews) reveló que el formulario es confuso.

**Action (Acción):** Propongo rediseñar el checkout. Estimamos que subir conversion a 3.5%
generaría $200K/mes adicionales. ROI en 2 meses.

```

---

### Visualizaciones Efectivas

#### Elegir el Gráfico Correcto

| Objetivo | Gráfico | Ejemplo |
|:---------|:--------|:--------|
| **Comparar valores** | Bar chart | Ventas por producto |
| **Mostrar tendencia** | Line chart | Revenue over time |
| **Mostrar proporción** | Pie chart / Donut | Market share |
| **Mostrar distribución** | Histogram | Latency distribution |
| **Mostrar correlación** | Scatter plot | Price vs demand |

---

#### Principios de Diseño

| Principio | Descripción | Ejemplo |
|:----------|:------------|:--------|
| **Clarity** | Fácil de entender | Ejes etiquetados, leyenda clara |
| **Actionability** | Insight → acción | "Conversion bajó 10% → investigar checkout" |
| **Context** | Comparar con baseline | "Revenue: $100K (vs $90K last month)" |
| **Honesty** | No manipular ejes | Eje Y empieza en 0 (no en 95 para exagerar diferencias) |

---

## ✅ Calidad de Datos

### Dimensiones de Calidad

| Dimensión | Descripción | Ejemplo |
|:----------|:------------|:--------|
| **Completeness** | ¿Hay datos faltantes? | 95% de registros tienen email |
| **Accuracy** | ¿Datos son correctos? | Fechas futuras son inválidas |
| **Consistency** | ¿Datos son consistentes? | Mismo user_id en todas las tablas |
| **Timeliness** | ¿Datos son actuales? | Dashboard actualizado cada hora |

---

### Data Profiling

**Qué es:** Analizar dataset para entender su estructura y calidad.

**Ejemplo (Python):**

```python
import pandas as pd

df = pd.read_csv('users.csv')

# Profiling básico
print(df.info())  # Tipos, nulls
print(df.describe())  # Estadísticas

# Detectar nulls
print(df.isnull().sum())

# Detectar duplicados
print(df.duplicated().sum())

# Detectar outliers (IQR method)
Q1 = df['age'].quantile(0.25)
Q3 = df['age'].quantile(0.75)
IQR = Q3 - Q1
outliers = df[(df['age'] < Q1 - 1.5*IQR) | (df['age'] > Q3 + 1.5*IQR)]
print(f"Outliers: {len(outliers)}")
```

---

### Validación de Datos

**Ejemplo (Great Expectations):**

```python
import great_expectations as ge

# Cargar dataset
df = ge.read_csv('users.csv')

# Definir expectations
df.expect_column_values_to_not_be_null('user_id')
df.expect_column_values_to_be_unique('email')
df.expect_column_values_to_be_between('age', min_value=0, max_value=120)
df.expect_column_values_to_be_in_set('country', ['US', 'UK', 'CA'])

# Validar
results = df.validate()
print(results)
```

---

## 📋 Artefactos

### Data Literacy Guide (para no técnicos)

```markdown
# Data Literacy Guide

## Cómo Leer un Dashboard

### 1. Identificar Métricas Clave
- **Verde**: Métrica está bien
- **Amarillo**: Métrica en warning
- **Rojo**: Métrica crítica

### 2. Entender Contexto
- **Comparar con período anterior**: "Revenue: $100K (+10% vs last month)"
- **Comparar con objetivo**: "MAU: 50K (objetivo: 60K)"

### 3. Identificar Trends
- **↑ Subiendo**: Bueno para revenue, malo para churn
- **↓ Bajando**: Malo para revenue, bueno para churn
- **→ Estable**: Puede ser bueno o malo dependiendo del contexto

## Conceptos Clave

### Promedio vs Mediana
- **Promedio**: Suma / cantidad (afectado por outliers)
- **Mediana**: Valor del medio (robusto a outliers)

**Ejemplo:**
Salarios: [$50K, $55K, $60K, $65K, $500K]
- Promedio: $146K (engañoso!)
- Mediana: $60K (más representativo)

### Correlación vs Causalidad
- **Correlación**: Dos cosas cambian juntas
- **Causalidad**: Una causa la otra

**Regla:** Correlación ≠ Causalidad

## Preguntas para Hacer

Cuando veas un insight:
1. **¿Qué significa esto?** (entender la métrica)
2. **¿Por qué pasó?** (buscar causas)
3. **¿Qué deberíamos hacer?** (acción)
4. **¿Cómo medimos si funcionó?** (validación)
```

---

### Dashboard Design Checklist

```markdown
# Dashboard Design Checklist

## Clarity
- [ ] Título claro describe qué muestra el dashboard
- [ ] Métricas tienen labels descriptivos
- [ ] Ejes están etiquetados (con unidades)
- [ ] Colores tienen significado consistente (verde=bueno, rojo=malo)

## Actionability
- [ ] Dashboard responde una pregunta específica
- [ ] Insights → acciones claras
- [ ] Alertas configuradas para valores críticos

## Context
- [ ] Métricas comparadas con baseline (período anterior, objetivo)
- [ ] Trends visibles (no solo snapshot)
- [ ] Anotaciones para eventos importantes (deployments, campañas)

## Performance
- [ ] Dashboard carga en <5 segundos
- [ ] Queries optimizadas (índices, agregaciones)
- [ ] Refresh automático (no manual)

## Audience
- [ ] Nivel de detalle apropiado para audiencia
- [ ] Jerga técnica explicada (o evitada)
- [ ] Visualizaciones apropiadas (bar chart vs line chart)
```

---

### Data Quality Checklist

```markdown
# Data Quality Checklist

## Completeness
- [ ] % de nulls por columna <5%
- [ ] Campos críticos no tienen nulls (user_id, timestamp)
- [ ] Registros tienen todas las columnas esperadas

## Accuracy
- [ ] Valores están en rangos esperados (edad 0-120, no -5)
- [ ] Fechas son válidas (no futuras, no antes de 1900)
- [ ] Formatos son consistentes (emails válidos, phone numbers)

## Consistency
- [ ] Mismos IDs en diferentes tablas
- [ ] Enums usan valores consistentes (no "US" y "USA")
- [ ] Aggregations coinciden (sum(sales) == total_sales)

## Timeliness
- [ ] Datos actualizados según SLA (hourly, daily)
- [ ] Lag entre evento y disponibilidad <X horas
- [ ] Timestamps reflejan tiempo real del evento

## Validation
- [ ] Data profiling ejecutado
- [ ] Expectations definidas (Great Expectations, dbt tests)
- [ ] Alertas configuradas para data quality issues
```

---

## 📚 Recursos

- [Data Literacy Project](https://thedataliteracyproject.org/)
- [Storytelling with Data - Cole Nussbaumer Knaflic](https://www.storytellingwithdata.com/)
- [Great Expectations](https://greatexpectations.io/)
- [Metabase](https://www.metabase.com/)
