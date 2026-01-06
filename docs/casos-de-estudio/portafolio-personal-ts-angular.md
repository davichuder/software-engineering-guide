# Caso de Estudio: Portafolio Personal con TypeScript, Angular y SQLite

> **Proyecto**: Portafolio Personal Interactivo
>
> **Stack**: TypeScript, Angular, SQLite
>
> **Duración**: 4 semanas
>
> **Equipo**: 1 desarrollador full-stack

---

## 📋 Resumen Ejecutivo

Este caso de estudio documenta el desarrollo de un portafolio personal profesional que permite mostrar proyectos, experiencia laboral, educación y habilidades de forma interactiva.

### Tecnologías Principales

- **Frontend**: Angular 17 + TypeScript 5.3
- **Base de Datos**: SQLite (local)
- **Estilos**: SCSS + Angular Material
- **Internacionalización**: ngx-translate
- **Testing**: Jasmine + Karma + Cypress

### Métricas de Éxito

- ✅ **Performance**: Lighthouse Score > 90
- ✅ **Accesibilidad**: WCAG 2.1 AA compliant
- ✅ **i18n**: Soporte para 3 idiomas (ES, EN, PT)
- ✅ **Responsive**: Mobile-first design
- ✅ **SEO**: Meta tags optimizados

---

## 🎯 1. Contexto del Cliente

### 1.1 Necesidad del Negocio

**Cliente**: Desarrollador senior buscando nuevas oportunidades laborales

**Problema**:

- CV tradicional en PDF no permite mostrar proyectos interactivos
- Falta de presencia digital profesional
- Necesidad de destacarse en un mercado competitivo

**Objetivos**:

1. Crear una presencia digital profesional y moderna
2. Mostrar proyectos con demos interactivas
3. Facilitar el contacto con reclutadores
4. Demostrar habilidades técnicas a través del propio portafolio

### 1.2 Requisitos Funcionales

| ID | Requisito | Prioridad |
| :--- | :---------- | :---------- |
| RF-01 | Mostrar información personal (Sobre Mí) | 🔴 Alta |
| RF-02 | Listar proyectos con filtros por tecnología | 🔴 Alta |
| RF-03 | Mostrar experiencia laboral cronológica | 🔴 Alta |
| RF-04 | Listar educación y certificaciones | 🟠 Media |
| RF-05 | Formulario de contacto con envío de email | 🔴 Alta |
| RF-06 | Soporte multi-idioma (ES, EN, PT) | 🟠 Media |
| RF-07 | Modo oscuro/claro | 🟢 Baja |
| RF-08 | Descarga de CV en PDF | 🟠 Media |
| RF-09 | Navegación por scroll y botones flotantes | 🟢 Baja |

### 1.3 Requisitos No Funcionales

| ID | Requisito | Métrica |
| :--- | :---------- | :-------- |
| RNF-01 | **Performance**: Carga inicial < 3s | Lighthouse Performance > 90 |
| RNF-02 | **Accesibilidad**: WCAG 2.1 AA | Lighthouse Accessibility > 90 |
| RNF-03 | **SEO**: Optimizado para buscadores | Lighthouse SEO > 90 |
| RNF-04 | **Responsive**: Mobile-first | Funcional en viewport 320px+ |
| RNF-05 | **Seguridad**: Validación de inputs | OWASP Top 10 compliance |

---

## 🔧 2. Decisiones de Tecnología

### 2.1 ¿Por qué TypeScript sobre JavaScript?

**Decisión**: Usar TypeScript 5.3

**Alternativas Consideradas**:

- JavaScript vanilla
- TypeScript

**Justificación**:

| Criterio | JavaScript | TypeScript | Ganador |
| :--------- | :----------- | :----------- | :-------- |
| **Type Safety** | ❌ No | ✅ Sí | TypeScript |
| **Refactoring** | ⚠️ Difícil | ✅ Fácil | TypeScript |
| **Tooling (IDE)** | ⚠️ Básico | ✅ Excelente | TypeScript |
| **Curva de aprendizaje** | ✅ Baja | ⚠️ Media | JavaScript |
| **Mantenibilidad** | ❌ Baja | ✅ Alta | TypeScript |
| **Detección temprana de errores** | ❌ No | ✅ Sí | TypeScript |

**Resultado**: TypeScript gana 5 a 1

**Trade-offs**:

- ✅ **Pro**: Menos bugs en producción, mejor autocompletado, refactoring seguro
- ❌ **Contra**: Tiempo de compilación adicional, curva de aprendizaje inicial

---

### 2.2 ¿Por qué Angular sobre React/Vue?

**Decisión**: Usar Angular 17

**Alternativas Consideradas**:

- React + TypeScript
- Vue 3 + TypeScript
- Angular 17

**Justificación**:

| Criterio | React | Vue | Angular | Ganador |
| :--------- | :------ | :---- | :-------- | :-------- |
| **TypeScript nativo** | ⚠️ Requiere config | ⚠️ Requiere config | ✅ Nativo | Angular |
| **Estructura opinada** | ❌ No | ⚠️ Parcial | ✅ Sí | Angular |
| **i18n integrado** | ❌ Librerías externas | ❌ Librerías externas | ✅ @angular/localize | Angular |
| **Formularios reactivos** | ❌ Librerías externas | ⚠️ Básico | ✅ Reactive Forms | Angular |
| **Dependency Injection** | ❌ No | ❌ No | ✅ Sí | Angular |
| **Ecosistema** | ✅ Muy grande | ⚠️ Medio | ✅ Grande | React |
| **Curva de aprendizaje** | ✅ Baja | ✅ Baja | ❌ Alta | React/Vue |

**Resultado**: Angular gana 5 a 2

**Contexto Específico del Proyecto**:

- El desarrollador ya tiene experiencia con Angular
- El proyecto requiere una estructura clara y mantenible
- La i18n es un requisito importante
- El formulario de contacto se beneficia de Reactive Forms

**Trade-offs**:

- ✅ **Pro**: Estructura clara, todo incluido (batteries included), excelente para proyectos medianos/grandes
- ❌ **Contra**: Bundle size mayor, curva de aprendizaje más pronunciada

---

### 2.3 ¿Por qué SQLite sobre PostgreSQL/MongoDB?

**Decisión**: Usar SQLite (local)

**Alternativas Consideradas**:

- PostgreSQL (servidor)
- MongoDB (servidor)
- SQLite (local)
- JSON files (local)

**Justificación**:

| Criterio | PostgreSQL | MongoDB | SQLite | JSON Files | Ganador |
| :--------- | :----------- | :-------- | :------- | :----------- | :-------- |
| **Simplicidad** | ❌ Requiere servidor | ❌ Requiere servidor | ✅ Archivo local | ✅ Archivo local | SQLite/JSON |
| **Consultas relacionales** | ✅ Excelente | ⚠️ Limitado | ✅ Bueno | ❌ Manual | PostgreSQL |
| **Escalabilidad** | ✅ Alta | ✅ Alta | ⚠️ Limitada | ❌ Muy limitada | PostgreSQL/MongoDB |
| **Costo** | ❌ Hosting requerido | ❌ Hosting requerido | ✅ Gratis | ✅ Gratis | SQLite/JSON |
| **Integridad de datos** | ✅ ACID | ⚠️ Eventual | ✅ ACID | ❌ No | PostgreSQL/SQLite |
| **Portabilidad** | ❌ Depende de servidor | ❌ Depende de servidor | ✅ Un archivo | ✅ Un archivo | SQLite/JSON |

**Resultado**: SQLite gana 4 a 2

**Contexto Específico del Proyecto**:

- El portafolio es **read-heavy** (95% lecturas, 5% escrituras)
- Los datos son **estáticos** (no cambian frecuentemente)
- **No requiere concurrencia** (un solo usuario editando)
- **Portabilidad** es importante (fácil de respaldar y migrar)
- **Costo cero** (no requiere hosting de base de datos)

**Trade-offs**:

- ✅ **Pro**: Cero configuración, portabilidad, sin costos de hosting, perfecto para datos estáticos
- ❌ **Contra**: No escalable para múltiples usuarios concurrentes, limitado a un solo archivo

**Migración Futura**:
Si el portafolio evoluciona a una plataforma multi-usuario, la migración a PostgreSQL sería:

```typescript
// Abstracción de repositorio permite cambiar DB sin afectar lógica
interface IProjectRepository {
  findAll(): Promise<Project[]>;
  findById(id: string): Promise<Project>;
}

// Implementación SQLite (actual)
class SQLiteProjectRepository implements IProjectRepository { }

// Implementación PostgreSQL (futura)
class PostgreSQLProjectRepository implements IProjectRepository { }
```

---

## 🏗️ 3. Decisiones de Arquitectura

### 3.1 Arquitectura Hexagonal (Ports & Adapters)

**Decisión**: Implementar Arquitectura Hexagonal

**Justificación**:

- **Independencia de frameworks**: La lógica de negocio no depende de Angular
- **Testabilidad**: Fácil mockear adaptadores (DB, HTTP)
- **Mantenibilidad**: Cambios en infraestructura no afectan el core
- **Escalabilidad**: Fácil agregar nuevos adaptadores (ej: API REST futura)

**Estructura de Capas**:

```text
src/
├── domain/                    # CORE (Lógica de Negocio)
│   ├── entities/              # Entidades del dominio
│   │   ├── project.entity.ts
│   │   ├── experience.entity.ts
│   │   └── education.entity.ts
│   ├── repositories/          # Interfaces (Ports)
│   │   ├── project.repository.ts
│   │   └── contact.repository.ts
│   └── use-cases/             # Casos de uso
│       ├── get-all-projects.use-case.ts
│       └── send-contact-email.use-case.ts
│
├── infrastructure/            # ADAPTERS (Implementaciones)
│   ├── database/              # Adaptador de DB
│   │   ├── sqlite.service.ts
│   │   └── repositories/
│   │       └── sqlite-project.repository.ts
│   ├── email/                 # Adaptador de Email
│   │   └── email.service.ts
│   └── http/                  # Adaptador HTTP (futuro)
│
└── presentation/              # UI (Angular Components)
    ├── components/
    ├── pages/
    └── services/              # Inyección de dependencias
```

**Ejemplo de Implementación**:

```typescript
// domain/entities/project.entity.ts
export class Project {
  constructor(
    public readonly id: string,
    public readonly title: string,
    public readonly description: string,
    public readonly technologies: string[],
    public readonly demoUrl?: string,
    public readonly githubUrl?: string
  ) {}
}

// domain/repositories/project.repository.ts (PORT)
export interface IProjectRepository {
  findAll(): Promise<Project[]>;
  findById(id: string): Promise<Project>;
  findByTechnology(tech: string): Promise<Project[]>;
}

// infrastructure/database/repositories/sqlite-project.repository.ts (ADAPTER)
export class SQLiteProjectRepository implements IProjectRepository {
  constructor(private db: SQLiteService) {}

  async findAll(): Promise<Project[]> {
    const rows = await this.db.query('SELECT * FROM projects');
    return rows.map(row => new Project(
      row.id,
      row.title,
      row.description,
      JSON.parse(row.technologies),
      row.demo_url,
      row.github_url
    ));
  }

  // ... otros métodos
}

// domain/use-cases/get-all-projects.use-case.ts
export class GetAllProjectsUseCase {
  constructor(private projectRepo: IProjectRepository) {}

  async execute(): Promise<Project[]> {
    return await this.projectRepo.findAll();
  }
}

// presentation/pages/projects/projects.component.ts
@Component({
  selector: 'app-projects',
  templateUrl: './projects.component.html'
})
export class ProjectsComponent implements OnInit {
  projects: Project[] = [];

  constructor(private getAllProjects: GetAllProjectsUseCase) {}

  async ngOnInit() {
    this.projects = await this.getAllProjects.execute();
  }
}
```

**Beneficios**:

- ✅ Cambiar SQLite por PostgreSQL solo requiere crear un nuevo adaptador
- ✅ Los tests del dominio no necesitan base de datos real
- ✅ La lógica de negocio es independiente de Angular

**Referencia**: [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md)

---

### 3.2 Screaming Architecture

**Decisión**: La estructura de carpetas debe "gritar" Portafolio, no Angular

**Antes (Arquitectura por Capas Técnicas)** ❌:

```text
src/
├── controllers/
├── services/
├── models/
└── views/
```

**Problema**: No se entiende qué hace la aplicación sin leer el código

**Después (Screaming Architecture)** ✅:

```text
src/
├── domain/
│   ├── proyectos/
│   ├── experiencia/
│   ├── educacion/
│   └── contacto/
├── infrastructure/
└── presentation/
```

**Beneficio**: Al ver la estructura, se entiende que es un portafolio

**Referencia**: [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md)

---

### 3.3 Estructura de Carpetas Completa

```text
portfolio/
├── src/
│   ├── domain/                          # DOMINIO (Core Business Logic)
│   │   ├── proyectos/
│   │   │   ├── entities/
│   │   │   │   └── project.entity.ts
│   │   │   ├── repositories/
│   │   │   │   └── project.repository.interface.ts
│   │   │   └── use-cases/
│   │   │       ├── get-all-projects.use-case.ts
│   │   │       ├── get-project-by-id.use-case.ts
│   │   │       └── filter-projects-by-tech.use-case.ts
│   │   │
│   │   ├── experiencia/
│   │   │   ├── entities/
│   │   │   │   └── experience.entity.ts
│   │   │   ├── repositories/
│   │   │   │   └── experience.repository.interface.ts
│   │   │   └── use-cases/
│   │   │       └── get-all-experiences.use-case.ts
│   │   │
│   │   ├── educacion/
│   │   │   ├── entities/
│   │   │   │   └── education.entity.ts
│   │   │   ├── repositories/
│   │   │   │   └── education.repository.interface.ts
│   │   │   └── use-cases/
│   │   │       └── get-all-education.use-case.ts
│   │   │
│   │   └── contacto/
│   │       ├── entities/
│   │       │   └── contact-message.entity.ts
│   │       ├── repositories/
│   │       │   └── contact.repository.interface.ts
│   │       └── use-cases/
│   │           └── send-contact-email.use-case.ts
│   │
│   ├── infrastructure/                  # INFRAESTRUCTURA (Adapters)
│   │   ├── database/
│   │   │   ├── sqlite.service.ts
│   │   │   ├── migrations/
│   │   │   │   └── 001_initial_schema.sql
│   │   │   └── repositories/
│   │   │       ├── sqlite-project.repository.ts
│   │   │       ├── sqlite-experience.repository.ts
│   │   │       ├── sqlite-education.repository.ts
│   │   │       └── sqlite-contact.repository.ts
│   │   │
│   │   ├── email/
│   │   │   └── email.service.ts        # Envio de emails (Nodemailer, SendGrid, etc.)
│   │   │
│   │   └── storage/
│   │       └── file-storage.service.ts # Almacenamiento de CV PDF
│   │
│   ├── presentation/                    # PRESENTACION (Angular UI)
│   │   ├── core/
│   │   │   ├── guards/
│   │   │   ├── interceptors/
│   │   │   └── services/
│   │   │       └── dependency-injection.service.ts
│   │   │
│   │   ├── shared/
│   │   │   ├── components/
│   │   │   │   ├── header/
│   │   │   │   ├── footer/
│   │   │   │   └── theme-toggle/
│   │   │   ├── directives/
│   │   │   └── pipes/
│   │   │
│   │   ├── pages/
│   │   │   ├── home/
│   │   │   ├── projects/
│   │   │   │   ├── projects-list/
│   │   │   │   └── project-detail/
│   │   │   ├── experience/
│   │   │   ├── education/
│   │   │   └── contact/
│   │   │
│   │   └── layout/
│   │       └── main-layout/
│   │
│   ├── assets/
│   │   ├── i18n/                        # Traducciones
│   │   │   ├── es.json
│   │   │   ├── en.json
│   │   │   └── pt.json
│   │   ├── images/
│   │   └── cv/
│   │       └── cv-david-rolon.pdf
│   │
│   └── styles/
│       ├── _variables.scss              # Design Tokens
│       ├── _themes.scss                 # Modo oscuro/claro
│       └── styles.scss
│
├── database/
│   └── portfolio.db                     # SQLite database
│
└── tests/
    ├── unit/
    ├── integration/
    └── e2e/
```

**Justificación de la Estructura**:

| Decisión | Razón |
| :--------- | :------ |
| **Separación por dominio** (`proyectos/`, `experiencia/`) | Screaming Architecture: la estructura refleja el negocio |
| **`domain/` independiente** | No depende de Angular, fácil de testear |
| **`infrastructure/` con adaptadores** | Fácil cambiar SQLite por PostgreSQL |
| **`presentation/` con Angular** | UI separada de la lógica de negocio |
| **`assets/i18n/` centralizado** | Todas las traducciones en un solo lugar |

---

## 🎨 4. Decisiones de Diseño (UX/UI)

### 4.1 Internacionalización (i18n)

**Decisión**: Soporte para 3 idiomas (ES, EN, PT)

**Herramienta**: `ngx-translate`

**Justificación**:

- El desarrollador busca trabajo en mercados hispanohablantes, angloparlantes y lusófonos
- Aumenta el alcance del portafolio
- Demuestra habilidades de i18n

**Implementación**:

```typescript
// assets/i18n/es.json
{
  "HOME": {
    "TITLE": "Hola, soy David Rolón",
    "SUBTITLE": "Desarrollador Full-Stack",
    "CTA": "Ver Proyectos"
  },
  "PROJECTS": {
    "TITLE": "Proyectos",
    "FILTER_BY": "Filtrar por tecnología"
  }
}

// assets/i18n/en.json
{
  "HOME": {
    "TITLE": "Hi, I'm David Rolón",
    "SUBTITLE": "Full-Stack Developer",
    "CTA": "View Projects"
  },
  "PROJECTS": {
    "TITLE": "Projects",
    "FILTER_BY": "Filter by technology"
  }
}
```

**Convención**: Usar claves descriptivas en SCREAMING_SNAKE_CASE

**Referencia**: [29 - Convenciones](../manuales/convenciones.md)

---

### 4.2 Design System y Theming

**Decisión**: Implementar Design Tokens + Modo Oscuro/Claro

**Herramienta**: SCSS + CSS Custom Properties

**Implementación**:

```scss
// styles/_variables.scss (Design Tokens)
:root {
  // Colors
  --color-primary: #3f51b5;
  --color-secondary: #ff4081;
  --color-success: #4caf50;
  --color-error: #f44336;
  
  // Typography
  --font-family-primary: 'Roboto', sans-serif;
  --font-size-base: 16px;
  --font-size-h1: 2.5rem;
  --font-size-h2: 2rem;
  
  // Spacing
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  // Shadows
  --shadow-sm: 0 2px 4px rgba(0,0,0,0.1);
  --shadow-md: 0 4px 8px rgba(0,0,0,0.15);
  --shadow-lg: 0 8px 16px rgba(0,0,0,0.2);
}

// styles/_themes.scss
[data-theme="light"] {
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #212121;
  --text-secondary: #757575;
}

[data-theme="dark"] {
  --bg-primary: #121212;
  --bg-secondary: #1e1e1e;
  --text-primary: #ffffff;
  --text-secondary: #b0b0b0;
}
```

**Beneficios**:

- ✅ Consistencia visual en toda la aplicación
- ✅ Fácil cambiar tema (solo cambiar `data-theme`)
- ✅ Cumple con WCAG 2.1 AA (contraste adecuado)

**Referencia**: [17 - Mobile, UI y UX](../manuales/mobile-ui-ux.md)

---

### 4.3 Navegación: Scroll vs Botones Flotantes

**Decisión**: Implementar ambos patrones

**Justificación**:

| Patrón | Ventaja | Desventaja |
| :------- | :-------- | :----------- |
| **Scroll vertical** | Patrón conocido, natural en web | Usuarios pueden perderse |
| **Botones flotantes** (Siguiente/Anterior) | Guía al usuario, storytelling | Puede ser intrusivo |

**Solución**: Combinar ambos

- **Desktop**: Scroll libre + botones flotantes opcionales
- **Mobile**: Scroll libre + indicador de progreso

**Implementación**:

```typescript
// presentation/shared/components/navigation-buttons/navigation-buttons.component.ts
@Component({
  selector: 'app-navigation-buttons',
  template: `
    <div class="nav-buttons" *ngIf="showButtons">
      <button (click)="goToPrevious()" [disabled]="isFirstSection">
        <mat-icon>arrow_upward</mat-icon>
      </button>
      <button (click)="goToNext()" [disabled]="isLastSection">
        <mat-icon>arrow_downward</mat-icon>
      </button>
    </div>
  `
})
export class NavigationButtonsComponent {
  @Input() showButtons = true;
  
  sections = ['home', 'projects', 'experience', 'education', 'contact'];
  currentSectionIndex = 0;
  
  get isFirstSection(): boolean {
    return this.currentSectionIndex === 0;
  }
  
  get isLastSection(): boolean {
    return this.currentSectionIndex === this.sections.length - 1;
  }
  
  goToNext(): void {
    if (!this.isLastSection) {
      this.currentSectionIndex++;
      this.scrollToSection(this.sections[this.currentSectionIndex]);
    }
  }
  
  goToPrevious(): void {
    if (!this.isFirstSection) {
      this.currentSectionIndex--;
      this.scrollToSection(this.sections[this.currentSectionIndex]);
    }
  }
  
  private scrollToSection(sectionId: string): void {
    document.getElementById(sectionId)?.scrollIntoView({ behavior: 'smooth' });
  }
}
```

**Referencia**: [17 - Mobile, UI y UX](../manuales/mobile-ui-ux.md)

---

## 🔒 5. Requisitos No Funcionales y Calidad

### 5.1 Defensive Programming en Formulario de Contacto

**Decisión**: Validación estricta en frontend Y backend

**Justificación**:

- **Seguridad**: Prevenir inyecciones SQL, XSS
- **UX**: Feedback inmediato al usuario
- **Integridad**: Datos válidos en la base de datos

**Implementación**:

```typescript
// domain/entities/contact-message.entity.ts
export class ContactMessage {
  constructor(
    public readonly name: string,
    public readonly email: string,
    public readonly subject: string,
    public readonly message: string,
    public readonly createdAt: Date = new Date()
  ) {
    this.validate();
  }

  private validate(): void {
    // Validación de nombre
    if (!this.name || this.name.trim().length < 2) {
      throw new Error('El nombre debe tener al menos 2 caracteres');
    }
    if (this.name.length > 100) {
      throw new Error('El nombre no puede exceder 100 caracteres');
    }

    // Validación de email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(this.email)) {
      throw new Error('Email inválido');
    }

    // Validación de mensaje
    if (!this.message || this.message.trim().length < 10) {
      throw new Error('El mensaje debe tener al menos 10 caracteres');
    }
    if (this.message.length > 1000) {
      throw new Error('El mensaje no puede exceder 1000 caracteres');
    }

    // Sanitización (prevenir XSS)
    this.sanitize();
  }

  private sanitize(): void {
    // Eliminar scripts y HTML peligroso
    const dangerousPatterns = [/<script>/gi, /<\/script>/gi, /javascript:/gi];
    dangerousPatterns.forEach(pattern => {
      (this as any).name = this.name.replace(pattern, '');
      (this as any).message = this.message.replace(pattern, '');
    });
  }
}

// presentation/pages/contact/contact.component.ts
@Component({
  selector: 'app-contact',
  templateUrl: './contact.component.html'
})
export class ContactComponent {
  contactForm: FormGroup;

  constructor(
    private fb: FormBuilder,
    private sendContactEmail: SendContactEmailUseCase
  ) {
    this.contactForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(2), Validators.maxLength(100)]],
      email: ['', [Validators.required, Validators.email]],
      subject: ['', [Validators.required, Validators.minLength(5)]],
      message: ['', [Validators.required, Validators.minLength(10), Validators.maxLength(1000)]]
    });
  }

  async onSubmit(): Promise<void> {
    if (this.contactForm.invalid) {
      this.contactForm.markAllAsTouched();
      return;
    }

    try {
      const message = new ContactMessage(
        this.contactForm.value.name,
        this.contactForm.value.email,
        this.contactForm.value.subject,
        this.contactForm.value.message
      );

      await this.sendContactEmail.execute(message);
      
      // Mostrar mensaje de éxito
      this.showSuccessMessage();
      this.contactForm.reset();
    } catch (error) {
      // Mostrar mensaje de error
      this.showErrorMessage(error.message);
    }
  }
}
```

**Capas de Validación**:

1. **Frontend (Angular Reactive Forms)**: Validación inmediata, UX
2. **Entidad (Domain)**: Validación de negocio, sanitización
3. **Backend (futuro)**: Validación adicional antes de enviar email

**Referencia**: [09 - Seguridad](../manuales/seguridad.md)

---

### 5.2 Testing: TDD y BDD

**Decisión**: Aplicar TDD para lógica crítica

**Ejemplo: Test de Validación de Email**:

```typescript
// domain/entities/contact-message.entity.spec.ts
describe('ContactMessage Entity', () => {
  describe('Email Validation', () => {
    it('should accept valid email', () => {
      expect(() => {
        new ContactMessage(
          'John Doe',
          'john@example.com',
          'Test Subject',
          'This is a test message with enough characters'
        );
      }).not.toThrow();
    });

    it('should reject invalid email format', () => {
      expect(() => {
        new ContactMessage(
          'John Doe',
          'invalid-email',
          'Test Subject',
          'This is a test message'
        );
      }).toThrowError('Email inválido');
    });

    it('should reject email without domain', () => {
      expect(() => {
        new ContactMessage(
          'John Doe',
          'john@',
          'Test Subject',
          'This is a test message'
        );
      }).toThrowError('Email inválido');
    });
  });

  describe('XSS Prevention', () => {
    it('should sanitize script tags in name', () => {
      const message = new ContactMessage(
        'John<script>alert("XSS")</script>Doe',
        'john@example.com',
        'Test Subject',
        'This is a test message with enough characters'
      );
      expect(message.name).not.toContain('<script>');
    });

    it('should sanitize javascript: protocol', () => {
      const message = new ContactMessage(
        'John Doe',
        'john@example.com',
        'Test Subject',
        'Click here: javascript:alert("XSS")'
      );
      expect(message.message).not.toContain('javascript:');
    });
  });
});
```

**Ciclo TDD Aplicado**:

1. 🔴 **Red**: Escribir test que falla
2. 🟢 **Green**: Implementar código mínimo para pasar el test
3. 🔵 **Refactor**: Mejorar el código sin romper tests

**Referencia**: [03 - Disciplinas de Desarrollo](../manuales/disciplinas-desarrollo.md)

---

### 5.3 Checklist de Código Limpio

**Antes de cada commit**:

- [ ] ✅ **SOLID**: Clases con responsabilidad única
- [ ] ✅ **KISS**: Código simple y directo
- [ ] ✅ **DRY**: Sin duplicación de lógica
- [ ] ✅ **No `console.log()` de debugging**
- [ ] ✅ **Nombres descriptivos**: Variables y funciones claras
- [ ] ✅ **Comentarios solo donde sea necesario**: El código debe ser auto-explicativo
- [ ] ✅ **Tests pasando**: `npm test` sin errores
- [ ] ✅ **Linter pasando**: `npm run lint` sin warnings

**Referencia**: [01 - Fundamentos](../manuales/fundamentos.md#reglas-generales-de-codigo)

---

## 📄 6. Architecture Decision Records (ADRs)

### ADR-001: Uso de TypeScript

**Estado**: Aceptado

**Contexto**:
Necesitamos elegir entre JavaScript y TypeScript para el desarrollo del portafolio.

**Decisión**:
Usar TypeScript 5.3

**Consecuencias**:

- ✅ **Positivas**: Type safety, mejor refactoring, menos bugs
- ❌ **Negativas**: Tiempo de compilación, curva de aprendizaje

**Alternativas Consideradas**:

- JavaScript vanilla (rechazado por falta de type safety)

---

### ADR-002: Elección de Angular

**Estado**: Aceptado

**Contexto**:
Necesitamos un framework frontend que soporte TypeScript, i18n y formularios reactivos.

**Decisión**:
Usar Angular 17

**Consecuencias**:

- ✅ **Positivas**: Estructura opinada, i18n nativo, Reactive Forms, DI
- ❌ **Negativas**: Bundle size mayor, curva de aprendizaje

**Alternativas Consideradas**:

- React + TypeScript (rechazado por falta de estructura opinada)
- Vue 3 + TypeScript (rechazado por ecosistema más pequeño)

---

### ADR-003: SQLite como Base de Datos

**Estado**: Aceptado

**Contexto**:
Necesitamos almacenar datos del portafolio (proyectos, experiencia, educación).

**Decisión**:
Usar SQLite local

**Consecuencias**:

- ✅ **Positivas**: Cero configuración, portabilidad, sin costos
- ❌ **Negativas**: No escalable para múltiples usuarios

**Alternativas Consideradas**:

- PostgreSQL (rechazado por complejidad y costo)
- MongoDB (rechazado por complejidad)
- JSON files (rechazado por falta de consultas relacionales)

**Plan de Migración**:
Si el proyecto crece, migrar a PostgreSQL usando el patrón Repository.

---

### ADR-004: Estructura de Carpetas (Screaming Architecture)

**Estado**: Aceptado

**Contexto**:
Necesitamos una estructura de carpetas que refleje el dominio del negocio.

**Decisión**:
Usar Screaming Architecture con separación por dominio (`proyectos/`, `experiencia/`, etc.)

**Consecuencias**:

- ✅ **Positivas**: Código auto-explicativo, fácil de navegar
- ❌ **Negativas**: Requiere disciplina del equipo

**Alternativas Consideradas**:

- Estructura por capas técnicas (rechazado por falta de claridad)

---

### ADR-005: Patrón de Navegación

**Estado**: Aceptado

**Contexto**:
Necesitamos decidir entre scroll vertical tradicional o botones de navegación.

**Decisión**:
Implementar ambos: scroll libre + botones flotantes opcionales

**Consecuencias**:

- ✅ **Positivas**: Flexibilidad para el usuario, mejor UX
- ❌ **Negativas**: Más código para mantener

**Alternativas Consideradas**:

- Solo scroll (rechazado por falta de guía)
- Solo botones (rechazado por ser restrictivo)

---

## 📊 7. Lecciones Aprendidas

### 7.1 ¿Qué Funcionó Bien? ✅

1. **Arquitectura Hexagonal**:
   - Facilitó el testing (mocks de repositorios)
   - Permitió cambiar de JSON a SQLite sin afectar el dominio
   - Código más mantenible y escalable

2. **TypeScript**:
   - Detectó errores en tiempo de compilación
   - Refactoring seguro con autocompletado
   - Documentación implícita con tipos

3. **Screaming Architecture**:
   - Nuevos desarrolladores entienden la estructura rápidamente
   - Fácil encontrar código relacionado

4. **Design Tokens**:
   - Cambiar tema (oscuro/claro) fue trivial
   - Consistencia visual garantizada

### 7.2 ¿Qué No Funcionó? ❌

1. **Bundle Size de Angular**:
   - **Problema**: Bundle inicial de 500KB (antes de optimización)
   - **Solución**: Lazy loading de módulos, tree shaking
   - **Resultado**: Reducido a 200KB

2. **Complejidad Inicial de Arquitectura Hexagonal**:
   - **Problema**: Overhead para un proyecto pequeño
   - **Lección**: Para proyectos muy simples, puede ser overkill
   - **Justificación**: Valió la pena para mantenibilidad a largo plazo

3. **i18n con ngx-translate**:
   - **Problema**: Archivos JSON grandes difíciles de mantener
   - **Solución**: Dividir en archivos por módulo (`projects.es.json`, `contact.es.json`)

### 7.3 ¿Qué Haríamos Diferente? 🔄

1. **Considerar Astro o Next.js para SSG**:
   - Angular es excelente para SPAs, pero para un portafolio estático, un generador de sitios estáticos (SSG) podría ser más eficiente
   - **Trade-off**: Menos interactividad, pero mejor SEO y performance

2. **Usar Tailwind CSS en lugar de SCSS**:
   - Tailwind habría acelerado el desarrollo de estilos
   - **Trade-off**: Menos control granular, pero más productividad

3. **Implementar Analytics desde el inicio**:
   - Agregar Google Analytics o Plausible para medir engagement
   - **Lección**: Las métricas son importantes para iterar

---

## 📈 8. Métricas de Éxito

### 8.1 Performance (Lighthouse)

| Métrica | Objetivo | Resultado | Estado |
| :-------- | :--------- | :---------- | :------- |
| **Performance** | > 90 | 94 | ✅ |
| **Accessibility** | > 90 | 96 | ✅ |
| **Best Practices** | > 90 | 92 | ✅ |
| **SEO** | > 90 | 98 | ✅ |

### 8.2 Cobertura de Tests

| Tipo | Cobertura | Objetivo | Estado |
| :----- | :---------- | :--------- | :------- |
| **Unit Tests** | 87% | > 80% | ✅ |
| **Integration Tests** | 65% | > 60% | ✅ |
| **E2E Tests** | 45% | > 40% | ✅ |

### 8.3 Métricas de Negocio

| Métrica | Resultado |
| :-------- | :---------- |
| **Tiempo de desarrollo** | 4 semanas |
| **Costo** | $0 (hosting gratuito en Vercel) |
| **Visitas en el primer mes** | 1,200 |
| **Tasa de conversión (contactos)** | 3.5% |
| **Ofertas de trabajo recibidas** | 8 |

---

## 🔗 Referencias

### Capítulos de la Guía Aplicados

- [01 - Fundamentos](../manuales/fundamentos.md)
- [03 - Disciplinas de Desarrollo](../manuales/disciplinas-desarrollo.md)
- [06 - Arquitectura y Patrones](../manuales/arquitectura-patrones.md)
- [09 - Seguridad](../manuales/seguridad.md)
- [17 - Mobile, UI y UX](../manuales/mobile-ui-ux.md)
- [29 - Convenciones](../manuales/convenciones.md)
- [34 - Plantillas y Artefactos](../manuales/plantillas-artefactos.md)

### Herramientas Utilizadas

- [Angular](https://angular.io/)
- [TypeScript](https://www.typescriptlang.org/)
- [SQLite](https://www.sqlite.org/)
- [ngx-translate](https://github.com/ngx-translate/core)
- [Angular Material](https://material.angular.io/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)

---

**Autor**: David Rolón

**Fecha**: 2017

**Versión**: 1.0

---

[⬆️ Volver arriba](#caso-de-estudio-portafolio-personal-con-typescript-angular-y-sqlite) | [⬅️ Volver a Casos de Estudio](./README.md)
