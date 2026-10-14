# Project Rules — Najath Muhammad Portfolio

> This document defines mandatory product, engineering, security, and quality rules for the project.
> All contributors (human and AI) must follow these rules throughout the project lifecycle.
> Rules may only be changed with explicit approval from Najath Muhammad.

---

## 1. Product Vision

Create a **premium dark, futuristic, isometric 3D developer portfolio** for Najath Muhammad.

### 1.1 Visual Characteristics

| Element | Specification |
|---------|--------------|
| Background | Near-black with a perspective grid |
| Paths | Glowing cyan circuit-style paths connecting section objects |
| Core | A luminous central processor/core representing Najath's identity |
| Section Objects | Distinct 3D models for WORKS, SKILLS, EXPERIENCE, ABOUT ME, RESUME, and CONTACT |
| Labels | Small, readable labels integrated into the 3D scene |
| Aesthetic | Premium, technical, and restrained |
| Responsive | Navigation responsive across all device sizes |
| Accessibility | Non-3D fallback for users who cannot or do not want to interact with 3D |

### 1.2 Interaction Model

- Clicking a 3D object **must navigate** to the correct section.
- Provide **conventional navigation** (e.g., top nav or sidebar) for visitors who cannot or do not want to interact with 3D.
- The 3D scene must **never block access** to CV, projects, or contact information.
- **Respect `prefers-reduced-motion`** — disable or simplify animations when the media query is active.
- **Support devices without WebGL** — provide a graceful non-3D fallback.

### 1.3 3D Implementation Rules

- Use **React Three Fiber** and **Three.js** where appropriate.
- Prefer **reusable procedural geometry** for the first version over complex imported GLTF/OBJ models.
- Do **not** require seven complex imported 3D models before the website can function.
- Lazy-load the 3D scene so it does not block initial page render.

---

## 2. Application Stack

### 2.1 Frontend

| Layer | Technology |
|-------|-----------|
| Framework | Next.js |
| UI Library | React |
| Language | TypeScript |
| Styling | Tailwind CSS |
| 3D Rendering | React Three Fiber + Three.js |

### 2.2 Backend

| Layer | Technology |
|-------|-----------|
| Runtime | Node.js |
| Framework | Express |
| Language | TypeScript |
| ODM | Mongoose |
| Validation | Zod |
| Database | MongoDB |

### 2.3 Media

- Use **Cloudinary** or an equivalent configured storage provider for media assets.

### 2.4 Quality & Deployment

| Concern | Tool / Practice |
|---------|----------------|
| Linting | ESLint |
| Type safety | TypeScript strict checks |
| Testing | Automated tests (unit + integration) |
| CI/CD | GitHub Actions |
| Configuration | Environment-based (`.env`, never hardcoded) |
| Deployment | A documented production deployment process |

> **Rule**: Inspect the repository and package scripts before deciding on exact versions or commands.
> Avoid unnecessary dependencies.

---

## 3. Public Routes

| Route | Description |
|-------|-------------|
| `/` | Landing / 3D world |
| `/projects` | All projects |
| `/projects/[slug]` | Individual project case study |
| `/about` | About Najath |
| `/resume` | CV / Resume |
| `/contact` | Contact form |

---

## 4. Private Admin Routes

| Route | Description |
|-------|-------------|
| `/admin/login` | Admin authentication |
| `/admin` | Admin dashboard |
| `/admin/projects` | Manage projects |
| `/admin/skills` | Manage skills |
| `/admin/experience` | Manage experience |
| `/admin/profile` | Manage profile |
| `/admin/messages` | View contact messages |

---

## 5. Backend Architecture

### 5.1 Required Directory Structure

```
src/
├── controllers/
│   └── <entity>/
│       ├── interfaces/
│       └── implementation/
├── services/
│   └── <entity>/
│       ├── interfaces/
│       └── implementation/
├── repositories/
│   └── <entity>/
│       ├── interfaces/
│       └── implementation/
├── routes/
├── middleware/
├── models/
├── dto/
├── mappers/
├── validators/
├── constants/
├── errors/
├── responses/
├── config/
└── composition/   ← dependency graph composition root
```

### 5.2 Architectural Rules

| Rule | Description |
|------|-------------|
| **Controllers** | Handle HTTP input/output, request validation, and response mapping only. |
| **Validation** | Zod validates request bodies, route params, query params, and all untrusted input. |
| **Services** | Own business logic, domain rules, and expected edge cases. |
| **Repositories** | Own all database interactions. No raw queries outside repositories. |
| **Controller ↔ Service** | Controllers depend on **service interfaces**, not concrete implementations. |
| **Service ↔ Repository** | Services depend on **repository interfaces**, not concrete implementations. |
| **Dependency Injection** | Inject dependencies through constructors. Compose the dependency graph in the composition root. |
| **Single Responsibility** | Each class has one clear responsibility. |
| **Interfaces** | Use interfaces when they provide a real architectural boundary; avoid needless abstractions. |
| **DTOs & Mappers** | Return only approved fields. Never serialize database documents directly to public responses. |
| **Input Handling** | Validate and normalize all input. Sanitize untrusted content without destroying legitimate text. |
| **Response Models** | Centralize response models, HTTP status constants, route constants, response messages, and error codes. |
| **Error Messages** | Return safe, meaningful client messages. Never expose raw exceptions, stack traces, DB errors, or secrets. |
| **Error Handling** | Use try/catch where errors can be handled or meaningfully translated. No empty or redundant catches. |
| **Async Errors** | Let unhandled async errors reach centralized error middleware. |
| **Logging** | Log appropriately; never log credentials, tokens, or sensitive data. |

---

## 6. Frontend Rules

| Rule | Requirement |
|------|-------------|
| **Components** | Build reusable, typed components. |
| **API Layer** | Keep API requests in dedicated API/service modules, not presentation components. |
| **DTOs** | Use typed request/response DTOs on the frontend. |
| **Forms** | Controlled forms with validation state, clear field errors, loading/submission states, success/failure feedback. |
| **Search** | Implement debounced search for admin and public search inputs. |
| **Pagination** | Implement server-side pagination, sorting, and filtering for admin collections. |
| **States** | Handle loading, empty, error, retry, and stale-data states in all data-fetching components. |
| **Submissions** | Prevent duplicate form submissions and race conditions. |
| **Accessibility** | Accessible controls, keyboard navigation, visible focus, responsive layouts. |
| **Performance** | Lazy-load expensive 3D scenes and large assets. |
| **Secrets** | Never expose backend secrets in browser code or bundles. |

---

## 7. Admin Security Rules

| Rule | Requirement |
|------|-------------|
| **Server-side checks** | Admin access must be checked by the backend on every protected endpoint. Do not rely on hiding buttons or pages in React. |
| **Passwords** | Use securely hashed passwords (e.g., bcrypt with sufficient cost factor). |
| **Sessions/Tokens** | Use a properly designed session/access-token strategy and secure cookie settings where applicable. |
| **Logout** | Provide logout and server-side session invalidation. |
| **Rate limiting** | Rate-limit login and contact endpoints. |
| **Uploads** | Validate uploads by file type and size. |
| **CSRF** | Apply CSRF protection where required by cookie-based authentication design. |
| **Secrets** | Keep all secrets out of Git and client bundles. Use environment variables. |
| **Public access** | Public users may only read published content. Unpublished content must not appear in public API responses. |

---

## 8. Git Strategy

### 8.1 Branch Model

| Branch | Purpose |
|--------|---------|
| `main` | Production releases only. Never develop directly on main. |
| `dev` | Integration branch during development. Never develop directly on dev. |
| `feature/*` | Feature development. Branch from the latest `dev`. Merge back into `dev` after verification. |

### 8.2 Workflow

1. Create a feature branch from the latest `dev`.
2. Implement, test, and commit on the feature branch.
3. Merge completed features into `dev` after verification.
4. Only merge `dev` → `main` when a production release is approved.
5. Never invent a GitHub URL or overwrite existing Git history.
6. Inspect the repository and Git state before making branch or remote changes.

> **Note**: The actual GitHub repository URL will be provided by Najath after he creates the repository.

### 8.3 Suggested Feature Branches

| Branch | Description |
|--------|-------------|
| `feature/project-knowledge-base` | Source-of-truth docs, project rules, implementation plan |
| `feature/project-scaffold` | Monorepo/workspace setup, Next.js app, Express API bootstrap |
| `feature/quality-tooling` | ESLint, TypeScript config, testing setup, CI/CD pipelines |
| `feature/portfolio-design-system` | Tailwind config, global styles, typography, design tokens |
| `feature/interactive-3d-world` | React Three Fiber scene, procedural geometry, navigation |
| `feature/project-showcase` | Projects page, slug route, case study layout, local data |
| `feature/portfolio-content-api` | Projects/skills/experience public API endpoints |
| `feature/contact-api` | Contact form endpoint, rate limiting, message storage |
| `feature/admin-authentication` | Admin login, JWT/session, logout, route protection |
| `feature/admin-project-editor` | CRUD for projects in admin CMS |
| `feature/admin-skills-editor` | CRUD for skills in admin CMS |
| `feature/admin-profile-editor` | Profile editing in admin CMS |
| `feature/admin-content-publishing` | Draft/publish workflow, content status management |
| `feature/public-api-integration` | Frontend connects to live backend APIs |
| `feature/production-hardening` | Security headers, rate limits, error handling, prod build |

> Combine or split branches where it improves delivery; do not create branches for trivial individual edits.

---

## 9. Quality Gate

After every feature branch, the following checklist **must** be completed before merging to `dev`:

- [ ] Inspect changed files and Git diff.
- [ ] Run applicable frontend/backend ESLint checks.
- [ ] Run TypeScript type checks (`tsc --noEmit`).
- [ ] Run relevant automated tests.
- [ ] Run production builds (`next build`, `tsc` for backend).
- [ ] Test acceptance criteria and important edge cases manually.
- [ ] Fix all introduced failures.
- [ ] Document anything that could not be verified.
- [ ] Commit on the feature branch and prepare for review.

> **Rules**:
> - Do not claim checks succeeded unless they were actually run.
> - Never disable lint rules or suppress TypeScript errors with `any` to silence failures.
> - Never commit secrets or credentials.
> - Never overwrite unrelated work.

---

## 10. Delivery Priority

> The goal is a **presentable, working version by tomorrow**.

Priorities in order:

1. ✅ **Working project scaffold** — monorepo, Next.js, Express API, environment config.
2. ✅ **Premium dark visual design** — design system, Tailwind tokens, global styles, typography.
3. ✅ **Functioning 3D world** — React Three Fiber scene with clickable section navigation.
4. ✅ **Accurate CV content** — all content from `PORTFOLIO_SOURCE_OF_TRUTH.md` displayed correctly.
5. ✅ **Project case studies** — project pages with accurate descriptions.
6. ✅ **Reliable local execution** and a passing production build.

> **Note**: The initial demo **may use accurate typed local data**. Do not delay the demo until the
> full backend, authentication, and CMS are complete. Backend and admin CMS are secondary priorities.

---

*This document is version-controlled. All changes require explicit approval from Najath Muhammad.*
