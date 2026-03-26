# Tech Stack

## Runtime & Language

| Component | Technology | Version | Notes |
|-----------|-----------|---------|-------|
| Runtime | Node.js | 22.x LTS | |
| Language | TypeScript | 5.x | strict mode enabled |

---

## Frameworks & Libraries

### Backend

| Purpose | Package | Version | Notes |
|---------|---------|---------|-------|
| HTTP framework | Fastify | ^4.x | TypeScript-native, schema-based validation |
| ORM | Prisma | ^5.x | PostgreSQL adapter |
| Validation | Zod | ^3.x | Schema validation for inputs and env vars |
| Auth | jose | ^5.x | JWT signing/verification |
| Logging | pino | (bundled with Fastify) | Structured JSON logging |

### Frontend

| Purpose | Package | Version | Notes |
|---------|---------|---------|-------|
| UI framework | React | ^18.x | |
| Build tool | Vite | ^5.x | |
| Routing | TanStack Router | ^1.x | File-based, type-safe |
| Server state | TanStack Query | ^5.x | Data fetching and cache |
| Forms | React Hook Form | ^7.x | |
| Validation (forms) | Zod | ^3.x | Shared with backend schemas |
| UI components | shadcn/ui | latest | Radix-based, Tailwind-styled |
| Styles | Tailwind CSS | ^3.x | |

### Shared / Tooling

| Purpose | Package | Version | Notes |
|---------|---------|---------|-------|
| Monorepo | pnpm workspaces | pnpm 9.x | `apps/api`, `apps/web`, `packages/shared` |
| Testing | Vitest | ^2.x | Unit and integration tests |
| API integration tests | supertest | ^7.x | HTTP-level tests against Fastify |
| Linter | ESLint | ^9.x | Flat config |
| Formatter | Prettier | ^3.x | |

---

## Infrastructure

| Component | Technology | Notes |
|-----------|-----------|-------|
| Database | PostgreSQL 16 | Managed on the Hetzner VPS via Docker |
| Containerisation | Docker + Docker Compose | One compose file per environment |
| CI/CD | GitHub Actions | Lint → test → build → deploy |
| Hosting | Hetzner Cloud VPS | Ubuntu 24.04, Docker host |
| Reverse proxy | Caddy | Automatic HTTPS on the VPS |
| Secrets at runtime | `.env` files (not committed) | `dotenv` loaded by Fastify via `@fastify/env` |

---

## Repository Structure

```
mini-erp/
  apps/
    api/          ← Fastify backend
    web/          ← React + Vite frontend
  packages/
    shared/       ← Zod schemas, TypeScript types shared between api and web
  prisma/         ← schema.prisma and migrations
  docker/         ← Dockerfiles and compose files
  .github/
    workflows/    ← CI/CD pipelines
```

---

## Package Sources

- All packages from the public npm registry via pnpm
- No packages from unverified private registries without approval

## Adding New Dependencies

Before adding a new package:
1. Check if an existing approved package covers the need
2. Review the package's license (MIT/Apache/ISC preferred)
3. Note it in `implementation-notes-req-NNN.md` under Design Decisions
