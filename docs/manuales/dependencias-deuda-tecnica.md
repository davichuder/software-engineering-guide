# Gestión de Dependencias y Deuda Técnica

> Estrategias para gestionar dependencias, upgrades y deuda técnica de forma sistemática.

---

## 📦 Dependency Management

### Semantic Versioning

**Formato:** `MAJOR.MINOR.PATCH` (ej: `2.5.3`)

| Cambio | Incrementar | Ejemplo |
| :------- | :------------ | :-------- |
| **Breaking change** | MAJOR | `2.5.3` → `3.0.0` |
| **Nueva feature (backward compatible)** | MINOR | `2.5.3` → `2.6.0` |
| **Bug fix (backward compatible)** | PATCH | `2.5.3` → `2.5.4` |

---

### Cuándo Upgradear

| Tipo de Update | Urgencia | Criterio |
| :--------------- | :--------- | :--------- |
| **Security patch** | 🔴 Inmediato | CVE crítico, exploit público |
| **Major version** | 🟠 Planificado | Breaking changes, requiere testing extensivo |
| **Minor version** | 🟡 Mensual | Nuevas features, bajo riesgo |
| **Patch version** | 🟢 Semanal | Bug fixes, muy bajo riesgo |

---

### Lock Files

**Propósito:** Garantizar builds reproducibles.

| Lenguaje | Lock File | Package Manager |
| :--------- | :---------- | :---------------- |
| **JavaScript** | `package-lock.json` / `yarn.lock` / `pnpm-lock.yaml` | npm / yarn / pnpm |
| **Python** | `poetry.lock` / `Pipfile.lock` | poetry / pipenv |
| **Java** | `gradle.lockfile` | Gradle |
| **Ruby** | `Gemfile.lock` | Bundler |

### Best practice

- ✅ Commitear lock files a git
- ✅ Usar lock files en CI/CD
- ❌ Editar lock files manualmente

---

### Deprecation Policy

**Proceso:**

1. **Announce** (v1.0): Deprecar feature, documentar alternativa
2. **Warn** (v1.1-v1.9): Logs de warning cuando se usa feature deprecated
3. **Remove** (v2.0): Remover feature en próximo major version

**Ejemplo:**

```python
# v1.0: Announce deprecation
import warnings

def old_function():
    warnings.warn(
        "old_function is deprecated, use new_function instead",
        DeprecationWarning,
        stacklevel=2
    )
    # ... implementation

# v2.0: Remove
# old_function eliminada completamente
```

---

## 💳 Technical Debt Tracking

### Qué es Technical Debt

**Definición:** Costo de trabajo adicional causado por elegir una solución rápida en lugar de la mejor solución.

**Tipos:**

| Tipo | Descripción | Ejemplo |
| :----- | :------------ | :-------- |
| **Deliberate** | Consciente, para time-to-market | "Hardcodeamos esto para lanzar rápido" |
| **Accidental** | Inconsciente, por falta de conocimiento | "No sabíamos que existía este pattern" |
| **Bit rot** | Código que era bueno pero quedó obsoleto | "Usamos jQuery, ahora hay React" |

---

### Cómo Identificar

| Señal | Descripción |
| :------ | :------------ |
| **Código duplicado** | Mismo código en múltiples lugares |
| **Funciones largas** | >50 líneas, múltiples responsabilidades |
| **Tests frágiles** | Tests que fallan sin razón aparente |
| **Deployment lento** | >30 min para deployar |
| **Bugs recurrentes** | Mismo tipo de bug en diferentes lugares |
| **Onboarding lento** | Nuevos devs tardan >2 semanas en ser productivos |

---

### Cómo Cuantificar

**Fórmula simple:**

```text
Technical Debt = (Tiempo para implementar feature con deuda) - (Tiempo si no hubiera deuda)
```

**Ejemplo:**

- Feature nueva toma 5 días
- Si el código estuviera refactorizado, tomaría 2 días
- **Technical Debt = 3 días**

---

### Priorización de Deuda Técnica

**Matriz de Priorización:**

| Impacto \ Esfuerzo | Bajo Esfuerzo | Alto Esfuerzo |
| :------------------- | :-------------- | :-------------- |
| **Alto Impacto** | 🔴 **P0: Hacer YA** | 🟠 **P1: Planificar** |
| **Bajo Impacto** | 🟡 **P2: Backlog** | 🟢 **P3: No hacer** |

**Ejemplo:**

- **P0**: Refactorizar módulo de pagos (alto impacto, bajo esfuerzo)
- **P1**: Migrar de monolito a microservicios (alto impacto, alto esfuerzo)
- **P2**: Renombrar variables (bajo impacto, bajo esfuerzo)
- **P3**: Reescribir todo en Rust (bajo impacto, alto esfuerzo)

---

### Estrategia de Asignación de Tiempo

No basta con listar la deuda, hay que pagarla.

#### 1. La Regla del 20% (Boy Scout Rule constante)

- Dedicar 20% del tiempo de desarrollo a pagar deuda técnica.
- **Sprint de 2 semanas:** 8 días features, 2 días deuda técnica.
- **Viernes de Calidad:** Usar viernes por la tarde para refactors pequeños.

#### 2. Deuda en el Roadmap

- **P0 (Crítica):** Se trata como un **Bug Bloqueante**. Entra en el sprint actual o siguiente obligatoriamente.
- **P1 (Alta):** Se planifica como una **Feature**. Compite con nuevas funcionalidades en el Q-Planning.
- **P2 (Media):** Se ataca con el "20% tax" o en semanas de "Cool-down" entre proyectos.

#### 3. Negociación con Negocio

- Hablar en términos de **Riesgo** y **Velocidad**, no de "código limpio".
- *"Si refactorizamos X ahora (2 días), la feature Y tomará 3 días en vez de 7."*

---

## 🔄 Refactoring Strategies

### Strangler Pattern

**Qué es:** Reemplazar gradualmente sistema legacy con nuevo sistema.

**Proceso:**

1. Crear nuevo sistema en paralelo
2. Routing dual (legacy + nuevo)
3. Migrar features una a una
4. Deprecar legacy cuando todo esté migrado

### Ejemplo

```text
[Request] → [Router]
                ├─→ [Legacy System] (80% tráfico)
                └─→ [New System]    (20% tráfico)

Gradualmente:
[Request] → [Router]
                ├─→ [Legacy System] (20% tráfico)
                └─→ [New System]    (80% tráfico)

Finalmente:
[Request] → [New System] (100% tráfico)
```

---

### Branch by Abstraction

**Qué es:** Refactorizar sin feature branches largos.

**Proceso:**

1. Crear abstracción sobre código legacy
2. Implementar nueva versión detrás de abstracción
3. Switchear gradualmente de legacy a nuevo
4. Remover legacy

**Ejemplo:**

```python
# 1. Crear abstracción
class PaymentGateway(ABC):
    @abstractmethod
    def process_payment(self, amount): pass

class LegacyPaymentGateway(PaymentGateway):
    def process_payment(self, amount):
        # Legacy implementation
        pass

class NewPaymentGateway(PaymentGateway):
    def process_payment(self, amount):
        # New implementation
        pass

# 2. Feature flag para switchear
if feature_flags.is_enabled("new_payment_gateway"):
    gateway = NewPaymentGateway()
else:
    gateway = LegacyPaymentGateway()

gateway.process_payment(100)
```

---

### Feature Toggles

**Qué es:** Activar/desactivar features sin deployar.

**Tipos:**

| Tipo | Duración | Uso |
| :----- | :--------- | :---- |
| **Release toggle** | Corto plazo | Deploy features incompletas, activar cuando estén listas |
| **Experiment toggle** | Medio plazo | A/B testing |
| **Ops toggle** | Largo plazo | Circuit breakers, kill switches |
| **Permission toggle** | Permanente | Features por rol/plan |

### Best practices

- ✅ Remover toggles cuando no se usan
- ✅ Documentar qué hace cada toggle
- ❌ Tener >10 toggles activos (complejidad)

---

## ⚠️ Breaking Changes

### Cómo Introducir Breaking Changes

**Proceso:**

1. **Announce** (3-6 meses antes): Comunicar breaking change
2. **Deprecate** (1-3 meses antes): Marcar como deprecated, proveer alternativa
3. **Migrate** (1 mes antes): Proveer migration guide
4. **Remove** (major version): Remover feature deprecated

---

### Migration Guide Template

`````markdown
# Guía de Migración: [Feature X] → [Feature Y]
 
 ## Cambios de Ruptura (Breaking Changes)
 - [Cambio 1]
 - [Cambio 2]
 
 ## Antes (v1.x)
 ```[lenguaje]
 // Código viejo
 ```
 
 ## Después (v2.x)
 
 ```[lenguaje]
 // Código nuevo
 ```
 
 ## Migración Paso a Paso
 
 ### 1. Actualizar dependencias
 
 ```bash
 npm install package@2.0.0
 ```
 
 ### 2. Reemplazar llamadas deprecadas
 
 [Instrucciones]
 
 ### 3. Testear
 
 [Cómo verificar que la migración funcionó]
 
 ## Migración Automática (si existe)
 
 ```bash
 npx @package/migrate
 ```
 
 ## Soporte
 
 - **Soporte versión deprecada:** Hasta YYYY-MM-DD
 - **Preguntas:** [Slack channel / GitHub Discussions]

`````

---

## 📋 Artefactos

### Dependency Upgrade Checklist

```markdown
# Dependency Upgrade Checklist

## Pre-Upgrade
- [ ] Leer changelog de la nueva versión
- [ ] Identificar breaking changes
- [ ] Verificar compatibilidad con otras dependencias
- [ ] Crear branch de upgrade

## Upgrade
- [ ] Actualizar versión en package.json / requirements.txt / etc
- [ ] Actualizar lock file
- [ ] Ejecutar tests
- [ ] Revisar warnings de deprecation
- [ ] Actualizar código si hay breaking changes

## Testing
- [ ] Tests unitarios pasan
- [ ] Tests de integración pasan
- [ ] Tests E2E pasan
- [ ] Testing manual en staging

## Security
- [ ] Escanear vulnerabilidades (npm audit / safety)
- [ ] Verificar que no hay CVEs conocidos

## Documentation
- [ ] Actualizar README si hay cambios en setup
- [ ] Actualizar CHANGELOG
- [ ] Documentar breaking changes (si aplica)

## Deployment
- [ ] Deploy a staging
- [ ] Smoke tests en staging
- [ ] Deploy a producción
- [ ] Monitoring post-deployment
```

---

### Technical Debt Register

```markdown
# Technical Debt Register

| ID | Área | Descripción | Impacto | Esfuerzo | Prioridad | Owner | Status |
| :--- | :----- | :------------ | :-------- | :--------- | :---------- | :------ | :------- |
| TD-001 | Payments | Código duplicado en validación | Alto | Bajo | P0 | @alice | In Progress |
| TD-002 | Auth | Tests frágiles | Medio | Medio | P1 | @bob | Backlog |
| TD-003 | UI | jQuery → React | Alto | Alto | P1 | @charlie | Planned |
| TD-004 | DB | Queries N+1 en reportes | Alto | Bajo | P0 | @dave | Done |

## TD-001: Código Duplicado en Validación de Pagos

**Descripción:**
Lógica de validación de tarjetas de crédito duplicada en 3 lugares:
- `PaymentController`
- `SubscriptionController`
- `RefundController`

**Impacto:**
- Bugs: Si arreglamos bug en un lugar, hay que arreglarlo en 3
- Mantenibilidad: Difícil agregar nuevas validaciones

**Esfuerzo estimado:** 2 días

**Solución propuesta:**
Extraer a `CreditCardValidator` service reutilizable

**Prioridad:** P0 (alto impacto, bajo esfuerzo)

**Owner:** @alice

**Status:** In Progress (50% completado)
```

---

### Breaking Change Communication Template

````markdown
# Anuncio de Breaking Change: [Feature X]
 
 **Versiones afectadas:** v2.0.0+
 **Fecha deprecación:** YYYY-MM-DD
 **Fecha remoción:** YYYY-MM-DD (6 meses después)
 
 ## Qué Cambia
 [Descripción clara del cambio]
 
 ## Por Qué
 [Razón del breaking change]
 
 ## Impacto
 **Quién es afectado:**
 - [Grupo 1]
 - [Grupo 2]
 
 **Qué se rompe:**
 - [Funcionalidad 1]
 - [Funcionalidad 2]
 
 ## Ruta de Migración
 
 ### Opción 1: Migración Automática (Recomendada)
 ```bash
 npx @package/migrate
 ```
 
 ### Opción 2: Migración Manual
 
 [Guía paso a paso]
 
 ## Cronología
 
 - **YYYY-MM-DD**: Anuncio de deprecación
 - **YYYY-MM-DD**: Logs de warning agregados
 - **YYYY-MM-DD**: Feature removida en v2.0.0
 
 ## Soporte
 
 - **Ayuda migración:** [Slack channel]
 - **Preguntas:** [GitHub Discussions]
 - **Soporte extendido:** Contactar ventas para soporte v1.x extendido

````

---

## 📚 Recursos

- [Semantic Versioning](https://semver.org/)
- [Strangler Fig Pattern - Martin Fowler](https://martinfowler.com/bliki/StranglerFigApplication.html)
- [Feature Toggles - Pete Hodgson](https://martinfowler.com/articles/feature-toggles.html)
- [Managing Technical Debt - Martin Fowler](https://martinfowler.com/bliki/TechnicalDebt.html)
