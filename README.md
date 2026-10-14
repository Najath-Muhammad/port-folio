# Najath Muhammad — Portfolio

Premium dark, futuristic, isometric 3D developer portfolio with a custom admin CMS.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js · React · TypeScript · Tailwind CSS · React Three Fiber |
| Backend | Node.js · Express · TypeScript · MongoDB · Mongoose · Zod |
| Media | Cloudinary |
| CI/CD | GitHub Actions |

## Documentation

- [`docs/PORTFOLIO_SOURCE_OF_TRUTH.md`](docs/PORTFOLIO_SOURCE_OF_TRUTH.md) — Canonical CV data
- [`docs/PROJECT_RULES.md`](docs/PROJECT_RULES.md) — Architecture, security & quality rules
- [`docs/IMPLEMENTATION_PLAN.md`](docs/IMPLEMENTATION_PLAN.md) — Phased implementation plan

## Branch Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Production releases only |
| `dev` | Integration (active development) |
| `feature/*` | Individual feature branches — branch from `dev` |

## Getting Started

> Setup instructions will be added in `feature/project-scaffold`.

## Environment Variables

Copy `.env.example` and fill in your values:

```bash
cp .env.example .env.local
```

> `.env.example` will be added in `feature/project-scaffold`.
