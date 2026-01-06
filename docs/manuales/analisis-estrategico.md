# Análisis Estratégico y de Negocio

> Frameworks para analizar mercados, competencia, clientes y tomar decisiones estratégicas informadas.

---

## 🎯 Análisis Estratégico

**Qué:** Herramientas para evaluar situación actual, entorno y definir estrategia.

**Por qué:** Decisiones basadas en análisis sistemático > intuición.

**Quién:** Product Managers, founders, strategy teams, senior leadership.

**Cuándo:** Planning anual/trimestral, lanzar producto, pivotar, expansión.

**Esfuerzo:** 1-4 semanas de análisis, decisiones que afectan años.

---

## 🔍 FODA / SWOT

**Qué:** Análisis de Fortalezas, Oportunidades, Debilidades y Amenazas.

**Por qué:** Vista 360° de situación interna y externa.

**Cuándo:** Inicio de proyecto, planning estratégico, decisiones importantes.

**Cómo:** Workshop 2-3 horas con stakeholders, matriz 2x2.

| | **Positivo** | **Negativo** |
|:---|:-------------|:-------------|
| **Interno** | **Fortalezas** (Strengths); ¿Qué hacemos bien?; ¿Qué recursos únicos tenemos? | **Debilidades** (Weaknesses); ¿Qué podemos mejorar?; ¿Qué nos falta? |
| **Externo** | **Oportunidades** (Opportunities); ¿Qué tendencias favorecen?; ¿Qué gaps de mercado? | **Amenazas** (Threats); ¿Qué competencia?; ¿Qué riesgos externos? |

**Ejemplo - Startup SaaS B2B:**

**Fortalezas:**

- Equipo técnico senior
- Velocidad de desarrollo
- Producto innovador

**Debilidades:**

- Sin brand recognition
- Presupuesto marketing limitado
- Solo 2 clientes actuales

**Oportunidades:**

- Mercado creciendo 40% anual
- Competidores con UX pobre
- Remote work aumenta demanda

**Amenazas:**

- Competidor grande puede copiar
- Recesión reduce presupuestos B2B
- Regulaciones nuevas posibles

**⚠️ Sesgos Cognitivos a Evitar:**

- **[Confirmation Bias](./sesgos-falacias.md#sesgos-de-decision-y-juicio)**: Tendencia a buscar solo información que confirme nuestras creencias. En FODA, esto puede llevar a sobrestimar fortalezas e ignorar debilidades reales. **Mitigación**: Buscar activamente evidencia que contradiga tus suposiciones.

- **[Status Quo Bias](./sesgos-falacias.md#sesgos-de-decision-y-juicio)**: Preferencia por mantener el estado actual. Puede impedir identificar oportunidades de cambio necesarias. **Mitigación**: Preguntarse "si empezara de cero hoy, ¿haría esto igual?"

**Siguiente paso:** Usar CAME para convertir insights en acciones.

---

## 🎯 CAME

**Qué:** Acciones estratégicas derivadas del FODA.

**Por qué:** FODA sin acción = papel. CAME convierte análisis en plan.

**Cuándo:** Inmediatamente después de FODA.

| FODA | CAME | Descripción | Acción |
|:-----|:-----|:------------|:-------|
| **Fortalezas** | **Mantener** (Corregir) | Seguir invirtiendo, defender ventaja | Mantener velocidad: contratar más devs |
| **Oportunidades** | **Explotar** (Afrontar) | Capitalizar, actuar rápido | Lanzar campaña en nicho remote work |
| **Debilidades** | **Corregir** (Mantener) | Mejorar, invertir recursos | Contratar CMO, aumentar budget marketing |
| **Amenazas** | **Afrontar** (Explotar) | Mitigar riesgo, plan contingencia | Diversificar: añadir tier pequeñas empresas |

---

## 🌍 PESTEL

**Qué:** Análisis de factores macro-ambientales: Político, Económico, Social, Tecnológico, Ecológico, Legal.

**Por qué:** Entender contexto externo que no controlamos pero nos afecta.

**Cuándo:** Estrategia long-term, expansión internacional, nuevos mercados.

**Cómo:** Investigar cada factor, identificar impacto en negocio.

| Factor | Qué | Preguntas | Ejemplo Tech |
|:-------|:-----|:----------|:-------------|
| **Político** | Estabilidad, políticas, gobierno | ¿Cambios en regulación tech?; ¿Incentivos fiscales? | Subsidios I+D, restricciones export chips |
| **Económico** | Inflación, tasas, PBI, empleo | ¿Recesión?; ¿Poder adquisitivo? | Startup funding afectado por tasas altas |
| **Social** | Demografía, cultura, tendencias | ¿Adopción tecnológica?; ¿Valores? | Remote work normalizado post-COVID |
| **Tecnológico** | Innovación, automatización, I+D | ¿Nuevas tecnologías?; ¿Obsolescencia? | AI generativa democratiza creación contenido |
| **Ecológico** | Sostenibilidad, clima, recursos | ¿Regulaciones verdes?; ¿Consumidores eco-conscious? | Data centers eficientes energéticamente |
| **Legal** | Leyes, IP, privacidad | ¿GDPR?; ¿Copyright AI? | Regulación IA en UE, privacidad datos |

**Ejemplo - SaaS expandiendo a UE:**

- **Legal:** GDPR compliance obligatorio → invertir en infra UE
- **Tecnológico:** Alta adopción cloud → oportunidad
- **Económico:** Euro fuerte → pricing en EUR atractivo

---

## ⚔️ 5 Fuerzas de Porter

**Qué:** Análisis de competitividad de industria.

**Por qué:** Entender atractivo de mercado y dinámica competitiva.

**Cuándo:** Entrar nuevo mercado, evaluar rentabilidad industria.

**Las 5 Fuerzas:**

```text
         [Amenaza Nuevos Entrantes]
                    ↓
[Poder Proveedores] → [Rivalidad Competidores] ← [Poder Compradores]
                    ↑
         [Amenaza Sustitutos]
```

| Fuerza | Qué | Alta = ⚠️ | Baja = ✅ |
|:-------|:-----|:----------|:---------|
| **Rivalidad entre competidores** | Intensidad competencia actual | Muchos competidores similares, guerra precios | Pocos competidores, diferenciación clara |
| **Amenaza nuevos entrantes** | Facilidad para nuevos competidores | Bajas barreras entrada, capital bajo | Altas barreras (regulación, capital, tech) |
| **Poder de negociación proveedores** | Influencia de proveedores | Pocos proveedores, altos costos cambio | Muchos proveedores, fácil cambiar |
| **Poder de negociación compradores** | Influencia de clientes | Pocos clientes grandes, fácil cambiar | Muchos clientes pequeños, altos switching costs |
| **Amenaza productos sustitutos** | Alternativas al producto | Sustitutos buenos y baratos | Pocos sustitutos, baja performance |

**Ejemplo - Cloud Storage:**

- **Rivalidad:** Alta (AWS, Google, Azure, Dropbox)
- **Nuevos entrantes:** Media (capital alto pero tech disponible)
- **Poder proveedores:** Baja (múltiples datacenter providers)
- **Poder compradores:** Alta (empresas grandes negocian descuentos)
- **Sustitutos:** Media (on-prem, file servers legacy)

**Conclusión:** Industria competitiva, márgenes presionados → diferenciarse o especializarse.

---

## 💎 VRIO

**Qué:** Framework para evaluar ventaja competitiva sostenible: Valor, Rareza, Imitabilidad, Organización.

**Por qué:** Identificar qué recursos/capacidades son ventaja real.

**Cuándo:** Evaluar fortalezas, decisiones de inversión.

**Preguntas:**

| Pregunta | Resultado |
|:---------|:----------|
| ¿Agrega **Valor**? | No → Desventaja competitiva |
| ¿Es **Raro**? | No → Paridad competitiva |
| ¿Es **Inimitable** (difícil copiar)? | No → Ventaja temporal |
| ¿Está **Organizado** para capturar valor? | Sí → **Ventaja sostenible** ✅ |

**Ejemplo - Tesla:**

| Recurso | Valor | Raro | Inimitable | Organizado | Resultado |
|:--------|:------|:-----|:-----------|:-----------|:----------|
| Marca | ✅ | ✅ | ✅ (reputación construida años) | ✅ | Ventaja sostenible |
| Baterías propias | ✅ | ✅ | ⚠️ (competidores invierten I+D) | ✅ | Ventaja temporal |
| Software OTA | ✅ | ⚠️ (otros adoptan) | ❌ | ✅ | Ventaja temporal |

---

## 👤 Buyer Persona

**Qué:** Representación semi-ficticia del cliente ideal basada en investigación.

**Por qué:** Entender profundamente al cliente para mejor producto/marketing.

**Cuándo:** Definir producto, crear contenido, segmentar mercado.

**Cómo:** Entrevistas, encuestas, analytics. Crear 2-4 personas clave.

**Template:**

```text
Nombre: Laura, la Lead Developer

Demografía:
- 32 años, mujer
- Ingeniería de Software, 8 años experiencia
- Salario: $120k
- Ubicación: Remoto (Argentina)

Psicografía:
- Valora: eficiencia, calidad, aprendizaje
- Frustraciones: reuniones innecesarias, legacy code
- Objetivos: crecer técnicamente, work-life balance

Comportamiento:
- Lee: Dev.to, Hacker News
- Herramientas: VS Code, GitHub, Linear
- Influencers: Kent C. Dodds, Dan Abramov

Necesidades:
- Herramientas que aumenten productividad
- Documentación clara
- Community support

Quote: "Si no puedo configurarlo en 5 minutos, busco alternativa."
```

**Uso:** Validar features ("¿Laura usaría esto?"), escribir copy, priorizar roadmap.

---

## 🎯 ICP (Ideal Customer Profile)

**Qué:** Descripción de la compañía perfecta para tu producto B2B.

**Por qué:** Foco en leads de alto potencial, mejor fit product-market.

**Cuándo:** B2B sales, marketing campaigns, qualify leads.

**Diferencia con Persona:** ICP = empresa, Persona = individuo dentro de empresa.

**Criterios:**

| Categoría | Ejemplos |
|:----------|:---------|
| **Firmográficos** | Industria (SaaS, Fintech); Tamaño (500 empleados); Revenue ($5M-$50M); Ubicación (LATAM, USA) |
| **Tecnográficos** | Stack tech (React, AWS); Herramientas (Salesforce, Slack); Madurez tech (cloud-native) |
| **Comportamiento** | Budget disponible ($50k+/año); Ciclo compra (3-6 meses); Decision makers (CTO, VP Eng) |
| **Pain Points** | Escalabilidad limitada; Seguridad preocupante; Time-to-market lento |

**Ejemplo ICP - Herramienta DevOps:**

- **Industria:** SaaS, E-commerce
- **Tamaño:** 11000 empleados
- **Stack:** Kubernetes, microservicios
- **Madurez:** Usando CI/CD básico, quieren optimizar
- **Budget:** >$100k/año en tooling
- **Pain:** Deploys lentos, rollbacks manuales

---

## 📅 Diagrama de Gantt

**Qué:** Gráfico de barras que muestra cronograma de proyecto.

**Por qué:** Visualizar timeline, dependencias, hitos.

**Cuándo:** Planning de proyecto, comunicar timeline a stakeholders.

**Componentes:**

- Eje X: Tiempo (días, semanas, meses)
- Eje Y: Tareas
- Barras: Duración de cada tarea
- Flechas: Dependencias

**Ejemplo - Lanzamiento MVP:**

```text
Tarea                  | Sem1 | Sem2 | Sem3 | Sem4 |
-----------------------|------|------|------|------|
Diseño UX              | ███  |      |      |      |
Desarrollo Backend     |   ████████  |      |      |
Desarrollo Frontend    |      |  █████████  |      |
Testing                |      |      |  ██  |      |
Deploy                 |      |      |      | █    |
```

**Herramientas:** [Microsoft Project](https://www.microsoft.com/en-us/microsoft-365/project), [TeamGantt](https://www.teamgantt.com/), [GanttProject](https://www.ganttproject.biz/)

---

## 🚫 Errores Comunes

| Error | Problema | Solución |
|:------|:---------|:---------|
| **Análisis sin acción** | Documentos que nadie usa | Derivar OKRs y plan concreto |
| **Assumptions sin validar** | Decisiones basadas en guesses | Customer interviews, MVP |
| **Personas genéricas** | "Todas las empresas" | Segmentar, priorizar ICP |
| **FODA superficial** | Solo lo obvio | Research profundo, data |
| **Ignorar amenazas** | Optimismo sesgado | War gaming, escenarios pesimistas |
| **No priorizar acciones** | Todo es igual | OKRs, PBI |
| **Causación = Causalidad** | Correlación no implica causa | Experimentos, domain knowledge, Testing A/B |

---

## 📚 Recursos

- [Blue Ocean Strategy - Kim & Mauborgne](https://www.blueoceanstrategy.com/)
- [Positioning - Al Ries & Jack Trout](https://www.amazon.com/Positioning-Battle-Your-Al-Ries/dp/0071373586)
- [Good Strategy Bad Strategy - Richard Rumelt](https://www.amazon.com/Good-Strategy-Bad-Difference-Matters/dp/0307886239)
- [Harvard Business Review](https://hbr.org/)
