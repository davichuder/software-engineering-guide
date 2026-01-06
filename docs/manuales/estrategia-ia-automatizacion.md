# Estrategia de IA y Automatización

> Estrategia de adopción de IA en workflows reales: cuándo usar, límites, integración en procesos.

---

## 🤖 Introducción

**Qué:** Estrategia para integrar IA en workflows de desarrollo sin perder control ni calidad.

**Por qué:** IA bien usada amplifica productividad 10x. Mal usada genera código frágil, inseguro y difícil de mantener.

**Cómo:** Definir casos de uso, límites claros, validación rigurosa e integración en procesos existentes.

### Filosofía: IA como Amplificador, No Reemplazo

- **IA amplifica**: Un senior con IA es 10x más productivo. Un junior con IA sigue siendo junior.
- **Garbage in, garbage out**: Prompts vagos → código mediocre
- **Validación crítica**: NUNCA confiar ciegamente en código generado por IA
- **Conceptos > Código**: IA genera código, pero tú debés entender qué hace y por qué

---

## 🎯 Casos de Uso Prácticos

### Code Review Automatizado

**Qué hace:**

- Detectar code smells (duplicación, complejidad ciclomática alta)
- Identificar vulnerabilidades (SQL injection, XSS, secrets expuestos)
- Sugerir refactorings (extract method, rename variable)

**Herramientas:**

- **SonarQube** + IA: Análisis estático con sugerencias contextuales
- **GitHub Copilot** en PRs: Sugerencias de mejoras
- **Custom agents**: Agentes especializados en tu stack

**Integración:**

```yaml
# .github/workflows/ai-code-review.yml
- name: AI Code Review
  run: |
    # Analizar diff del PR
    git diff origin/main...HEAD > changes.diff
    # Enviar a agente de code review
    ai-agent code-reviewer --input changes.diff --output review.md
```

**Límites:**

- ✅ Detectar patterns obvios (duplicación, complejidad)
- ❌ Entender contexto de negocio (por qué se tomó una decisión)

---

### Generación de Tests

**Qué hace:**

- Generar unit tests para funciones puras
- Generar test cases (happy path, edge cases, error handling)
- Generar mocks y fixtures

**Ejemplo de prompt:**

```text
Genera unit tests con pytest para esta función:

[código]

Incluye:
- Happy path
- Edge cases (input vacío, null, valores extremos)
- Error handling (excepciones esperadas)
- Mocks para dependencias externas
```

**Validación:**

- ✅ Ejecutar tests generados y verificar que pasen
- ✅ Revisar coverage (debe ser >80%)
- ❌ Aceptar tests que solo testean implementación (no comportamiento)

---

### Documentación Auto-Generada

**Qué hace:**

- Generar docstrings a partir de código
- Generar README a partir de estructura de proyecto
- Generar API documentation (OpenAPI) a partir de código

**Ejemplo:**

```python
# Antes (sin docstring)
def calculate_discount(price, customer_type):
    if customer_type == "premium":
        return price * 0.8
    return price * 0.9

# Después (con IA)
def calculate_discount(price: float, customer_type: str) -> float:
    """
    Calculate discounted price based on customer type.
    
    Args:
        price: Original price before discount
        customer_type: Type of customer ("premium" or "regular")
    
    Returns:
        Discounted price
    
    Raises:
        ValueError: If price is negative or customer_type is invalid
    
    Examples:
        >>> calculate_discount(100, "premium")
        80.0
        >>> calculate_discount(100, "regular")
        90.0
    """
    if price < 0:
        raise ValueError("Price cannot be negative")
    if customer_type not in ["premium", "regular"]:
        raise ValueError(f"Invalid customer type: {customer_type}")
    
    if customer_type == "premium":
        return price * 0.8
    return price * 0.9
```

---

### Refactoring Asistido

**Qué hace:**

- Detectar código duplicado y sugerir extracción
- Renombrar variables/funciones con nombres más descriptivos
- Aplicar design patterns (Strategy, Factory, etc.)

**Ejemplo de prompt:**

```text
Refactoriza este código aplicando el patrón Strategy para eliminar el switch statement:

[código]

Requisitos:
- Mantener misma interfaz pública
- Agregar tests para cada estrategia
- Documentar el patrón aplicado
```

---

### Migración de Código

**Qué hace:**

- Migrar de un lenguaje a otro (JS → TS, Python 2 → 3)
- Migrar de un framework a otro (AngularJS → Angular, Class components → Hooks)
- Actualizar a nuevas APIs (deprecated → current)

**Validación crítica:**

- 🔴 **NUNCA** migrar sin tests existentes
- 🔴 **SIEMPRE** revisar código migrado línea por línea
- 🔴 **EJECUTAR** tests antes y después de migración

---

## 🚫 Límites de la IA

### ❌ Qué NO Delegar a IA

| Tarea | Por qué NO |
| :------ | :----------- |
| **Decisiones arquitectónicas críticas** | IA no entiende trade-offs de negocio, escalabilidad futura, constraints organizacionales |
| **Compliance y regulaciones** | IA puede generar código que viola GDPR, HIPAA, SOC2 sin saberlo |
| **Ethical decisions** | IA no tiene moral ni contexto social (ej: features que pueden ser usadas para discriminación) |
| **Security-critical code** | IA puede generar código con vulnerabilidades sutiles (timing attacks, race conditions) |
| **Performance-critical code** | IA no optimiza para latency/throughput sin contexto específico |

---

### ⚠️ Qué Delegar CON Validación Rigurosa

| Tarea | Validación Requerida |
| :------ | :--------------------- |
| **Boilerplate code** | Revisar que siga convenciones del proyecto |
| **Tests unitarios** | Ejecutar tests, revisar coverage, verificar que testean comportamiento (no implementación) |
| **Documentación** | Revisar precisión técnica, claridad, ejemplos correctos |
| **Refactoring** | Ejecutar tests, revisar que no cambie comportamiento |

---

## 📝 Prompt Engineering Avanzado

### Chain-of-Thought (CoT)

Hacer que la IA "piense en voz alta" antes de generar código.

**Ejemplo:**

```text
Antes de generar código, explica paso a paso:
1. Qué problema estamos resolviendo
2. Qué alternativas consideraste
3. Por qué elegiste esta solución
4. Qué trade-offs tiene

Luego genera el código.
```

**Resultado:** Código más pensado, con mejor contexto.

---

### Few-Shot Learning

Dar ejemplos de lo que querés antes de pedir la tarea.

**Ejemplo:**

```text
Genera unit tests siguiendo este estilo:

# Ejemplo 1:
def test_calculate_discount_premium_customer():
    # Given
    price = 100
    customer_type = "premium"
    
    # When
    result = calculate_discount(price, customer_type)
    
    # Then
    assert result == 80.0

# Ejemplo 2:
def test_calculate_discount_invalid_customer_type():
    # Given
    price = 100
    customer_type = "invalid"
    
    # When / Then
    with pytest.raises(ValueError, match="Invalid customer type"):
        calculate_discount(price, customer_type)

Ahora genera tests para esta función:
[código]
```

---

### Retrieval-Augmented Generation (RAG)

Proveer contexto relevante antes de generar código.

**Ejemplo:**

```text
Contexto del proyecto:
- Stack: FastAPI + PostgreSQL + SQLAlchemy
- Convenciones: 
  - Usar Pydantic para validación
  - Usar dependency injection para DB session
  - Tests con pytest + pytest-asyncio

Genera un endpoint CRUD para la entidad Product siguiendo estas convenciones.
```

---

## ✅ Evaluación de Outputs

### AI Output Validation Rubric

| Criterio | ✅ Pasa | ❌ Falla |
| :--------- | :-------- | :--------- |
| **Correctness** | Código compila/ejecuta sin errores | Syntax errors, runtime errors |
| **Security** | No vulnerabilidades (OWASP Top 10) | SQL injection, XSS, secrets expuestos |
| **Performance** | No bottlenecks obvios (N+1 queries, loops innecesarios) | Complejidad O(n²) donde O(n) es posible |
| **Maintainability** | Código legible, nombres descriptivos, funciones pequeñas | Funciones >50 líneas, nombres crípticos |
| **Testing** | Tests incluidos, coverage >80% | Sin tests o tests que no testean comportamiento |
| **Documentation** | Docstrings claros, ejemplos correctos | Sin docs o docs incorrectos |

---

### Checklist de Validación

Antes de aceptar código generado por IA:

- [ ] **Compilar/ejecutar**: ¿El código corre sin errores?
- [ ] **Tests**: ¿Hay tests? ¿Pasan? ¿Coverage >80%?
- [ ] **Security scan**: ¿Pasó SAST/DAST? ¿No hay secrets expuestos?
- [ ] **Code review**: ¿Un humano revisó el código?
- [ ] **Performance**: ¿No hay bottlenecks obvios?
- [ ] **Convenciones**: ¿Sigue el style guide del proyecto?
- [ ] **Documentación**: ¿Está documentado? ¿Es preciso?

---

## 🔄 Integración en CI/CD

### Pre-Commit Hooks con IA

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: ai-code-review
        name: AI Code Review
        entry: ai-agent code-reviewer
        language: system
        pass_filenames: true
        
      - id: ai-test-generation
        name: AI Test Generation
        entry: ai-agent test-generator --check-coverage
        language: system
        files: \.py$
```

---

### Code Suggestions en PRs

```yaml
# .github/workflows/pr-review.yml
name: AI PR Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  ai-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: AI Code Review
        run: |
          # Obtener diff del PR
          git diff origin/${{ github.base_ref }}...HEAD > pr.diff
          
          # Enviar a agente de code review
          ai-agent code-reviewer --input pr.diff --output review.md
          
      - name: Post Review Comment
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            const review = fs.readFileSync('review.md', 'utf8');
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: review
            });
```

---

### Automated Refactoring

```yaml
# .github/workflows/auto-refactor.yml
name: Auto Refactor

on:
  schedule:
    - cron: '0 2 * * 1'  # Lunes a las 2am

jobs:
  refactor:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Detect Code Smells
        run: |
          ai-agent refactorer --detect-smells --output smells.json
          
      - name: Apply Refactorings
        run: |
          ai-agent refactorer --apply-fixes --input smells.json
          
      - name: Run Tests
        run: pytest
        
      - name: Create PR
        if: success()
        uses: peter-evans/create-pull-request@v5
        with:
          title: "🤖 Automated Refactoring"
          body: "AI-suggested refactorings. Review carefully before merging."
          branch: auto-refactor
```

---

## 📋 Artefactos

### AI Adoption Checklist

Decidir si usar IA para una tarea:

| Criterio | Usar IA | NO usar IA |
| :--------- | :-------- | :----------- |
| **Repetitividad** | Tarea repetitiva (boilerplate, tests) | Tarea única, creativa |
| **Riesgo** | Bajo riesgo (código no crítico) | Alto riesgo (security, compliance) |
| **Complejidad** | Complejidad baja-media | Complejidad alta (arquitectura) |
| **Contexto** | Contexto claro, bien definido | Contexto ambiguo, muchas variables |
| **Validación** | Fácil de validar (tests, linters) | Difícil de validar (ethical decisions) |

**Ejemplo:**

- ✅ **Usar IA**: Generar unit tests para función pura
- ❌ **NO usar IA**: Decidir arquitectura de microservices vs monolito

---

### Prompt Template Library

#### Template: Code Review

```text
Revisa este código y proporciona feedback organizado por prioridad:

[código]

Checklist:
- Correctness (lógica, edge cases)
- Security (OWASP Top 10)
- Performance (bottlenecks, complejidad)
- Maintainability (legibilidad, nombres, funciones pequeñas)
- Testing (coverage, calidad de tests)

Formato de salida:
## Critical Issues (must fix)
- [issue 1]

## Warnings (should fix)
- [issue 1]

## Suggestions (consider improving)
- [issue 1]
```

---

#### Template: Test Generation

```text
Genera unit tests para esta función:

[código]

Requisitos:
- Framework: [pytest/jest/junit]
- Incluir:
  - Happy path
  - Edge cases (input vacío, null, valores extremos)
  - Error handling (excepciones esperadas)
  - Mocks para dependencias externas
- Estilo: Given-When-Then
- Coverage objetivo: >80%
```

---

#### Template: Documentation

```text
Genera documentación para este código:

[código]

Formato:
- Docstring con descripción clara
- Args con tipos y descripciones
- Returns con tipo y descripción
- Raises con excepciones posibles
- Examples con casos de uso reales

Audiencia: [junior/senior/stakeholders]
```

---

#### Template: Refactoring

```text
Refactoriza este código:

[código]

Objetivos:
- Aplicar [patrón de diseño específico]
- Reducir complejidad ciclomática
- Eliminar duplicación
- Mejorar nombres (variables, funciones)

Constraints:
- Mantener misma interfaz pública
- No cambiar comportamiento (tests deben seguir pasando)
- Agregar tests si no existen
```

---

## 📚 Recursos

- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Anthropic Prompt Library](https://docs.anthropic.com/claude/prompt-library)
- [GitHub Copilot Best Practices](https://github.blog/20how-to-write-better-prompts-for-github-copilot/)
- [Google AI Code Review](https://ai.google/research/pubs/pub49101)
