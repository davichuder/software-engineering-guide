# Gestión de Calidad

> Prácticas y herramientas para mantener código limpio, seguro y mantenible a través de validaciones automatizadas.

---

## 🎯 Calidad de Software

**Qué:** Conjunto de características que determinan si el software cumple requisitos y es mantenible.

**Por qué:** Reduce bugs, facilita evolución, acelera desarrollo a largo plazo.

**Quién:** Todo el equipo, especialmente developers y QA.

**Costo:** 20% del tiempo en configurar y mantener, previene costosas correcciones posteriores.

---

## 📊 Code Coverage

**Qué:** Porcentaje de código ejecutado por tests.

| Métrica | Qué | Target | Herramienta |
| :-------- | :----- | :------- | :------------ |
| **Line Coverage** | % líneas ejecutadas | ≥80% lógica crítica | [JaCoCo](https://www.eclemma.org/jacoco/), [Coverage.py](https://coverage.readthedocs.io/) |
| **Branch Coverage** | % ramas if/else ejecutadas | ≥70% | [Istanbul](https://istanbul.js.org/) |
| **Function Coverage** | % funciones llamadas | 100% funciones públicas | Coverage tools |
| **Mutation Coverage** | % mutantes detectados | ≥70% | [Stryker](https://stryker-mutator.io/), [mutmut](https://github.com/boxed/mutmut) |

**Anti-patrón:** 100% coverage sin tests significativos.

---

## 🔍 Static Analysis

**Qué:** Analizar código sin ejecutarlo.

**Por qué:** Detectar bugs, vulnerabilidades, code smells tempranamente.

| Herramienta | Qué | Cuándo |
| :------------ | :----- | :----- |
| [SonarQube](https://www.sonarsource.com/products/sonarqube/) | Plataforma calidad multi-lenguaje | CI/CD, pre-merge |
| [ESLint](https://eslint.org/) | Linter JavaScript/TypeScript | Pre-commit, IDE |
| [Pylint](https://pylint.pycqa.org/) | Linter Python | Pre-commit, CI |
| [Checkstyle](https://checkstyle.sourceforge.io/) | Linter Java | CI, IDE |
| [SpotBugs](https://spotbugs.github.io/) | Bug detector Java | CI |
| [Bandit](https://bandit.readthedocs.io/) | Security linter Python | CI |

---

## ✨ Code Formatting

**Qué:** Formateo automático consistente.

**Por qué:** Elimina debates de estilo, diffs limpios, legibilidad.

| Lenguaje | Herramienta | Config |
| :--------- | :------------ | :------- |
| **JavaScript/TS** | [Prettier](https://prettier.io/) | `.prettierrc` |
| **Python** | [Black](https://black.readthedocs.io/), [Ruff](https://docs.astral.sh/ruff/) | `pyproject.toml` |
| **Java** | [google-java-format](https://github.com/google/google-java-format) | Maven/Gradle plugin |
| **Go** | `gofmt` (built-in) | - |
| **Rust** | `rustfmt` (built-in) | - |

**Setup:** Pre-commit hooks con [Husky](https://typicode.github.io/husky/) o [pre-commit](https://pre-commit.com/).

---

## 🔐 Security Scanning

| Tipo | Qué | Herramienta |
| :----- | :----- | :------------ |
| **SAST** | Static Application Security Testing | [SonarQube](https://www.sonarsource.com/), [Checkmarx](https://checkmarx.com/) |
| **Dependency Scan** | Vulnerabilidades en librerías | [Snyk](https://snyk.io/), [Dependabot](https://github.com/dependabot) |
| **Secret Detection** | Credenciales en código | [GitGuardian](https://www.gitguardian.com/), [TruffleHog](https://github.com/trufflesecurity/trufflehog) |
| **Container Scan** | Vulnerabilidades en imágenes | [Trivy](https://aquasecurity.github.io/trivy/), [Clair](https://github.com/quay/clair) |

---

## 👥 Code Review

**Qué:** Revisión de código por pares antes de merge.

**Por qué:** Detecta bugs, mejora diseño, comparte conocimiento.

### Checklist

- [ ] Código cumple requisitos
- [ ] Tests incluidos y pasando
- [ ] Sin código comentado
- [ ] Sin console.log / print debug
- [ ] Nombres descriptivos
- [ ] Sin magic numbers
- [ ] Documentación actualizada
- [ ] Sin vulnerabilidades obvias
- [ ] Performance aceptable
- [ ] Cambios tienen sentido

### Best Practices

| Práctica | Por qué |
| :--------- | :---- |
| **PRs pequeños** | < 400 líneas, fácil revisar |
| **Descripción clara** | Qué, por qué, cómo testear |
| **Automatizar lo automatizable** | Linters, tests, no manual |
| **Ser constructivo** | Sugerir mejoras, no criticar |
| **Responder rápido** | < 24 horas |

---

## 🔄 CI Validation

**Qué:** Validaciones automáticas en CI/CD.

### Pipeline Típico

```yaml
1. Checkout code
2. Install dependencies
3. Lint (ESLint, Pylint)
4. Format check (Prettier, Black)
5. Unit tests
6. Integration tests
7. Code coverage (fail if <80%)
8. Security scan
9. Build
10. Deploy (staging)
```

**Herramientas:** [GitHub Actions](https://github.com/features/actions), [GitLab CI](https://docs.gitlab.com/ee/ci/), [CircleCI](https://circleci.com/)

---

## 📏 Complexity Metrics

| Métrica | Qué | Target | Herramienta |
| :-------- | :----- | :------- | :------------ |
| **Cyclomatic Complexity** | Número de caminos independientes | < 10 por función | SonarQube, ESLint |
| **Cognitive Complexity** | Dificultad para entender | < 15 | SonarQube |
| **LOC** | Lines of Code | < 200 por función como umbral máximo | Linters |
| **Nesting Depth** | Niveles de indentación | < 4 | Linters |

---

## 🎨 Design Quality

| Aspecto | Qué | Cómo medir |
| :-------- | :----- | :----------- |
| **Cohesión** | Qué tan relacionados están elementos | Alta cohesión = bueno |
| **Acoplamiento** | Dependencias entre módulos | Bajo acoplamiento = bueno |
| **Code Smells** | Indicadores de mal diseño | SonarQube, manual |
| **Technical Debt** | Costo de soluciones subóptimas | SonarQube Debt Ratio |

---

## 📊 Métricas de Calidad

| Métrica | Fórmula | Target |
| :-------- | :-------- | :------- |
| **Defect Density** | Bugs / KLOC | < 1 |
| **Test Success Rate** | Tests passing / Total tests | 100% |
| **Code Coverage** | Lines covered / Total lines | ≥80% |
| **Technical Debt Ratio** | Remediation cost / Development cost | < 5% |

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Ignorar warnings** | Acumulación de problemas | Tratar warnings como errores |
| **Tests sin asserts** | Falsa sensación de seguridad | Validar comportamiento real |
| **Coverage por coverage** | Tests inútiles | Tests significativos |
| **Skip CI checks** | Merges sin validar | CI obligatorio |
| **Code review superficial** | LGTM sin leer | Checklist, tiempo dedicado |

---

## 📚 Recursos

- [Clean Code - Robert Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Refactoring - Martin Fowler](https://refactoring.com/)
- [SonarQube Docs](https://docs.sonarqube.org/)
- [Google Engineering Practices](https://google.github.io/eng-practices/)
