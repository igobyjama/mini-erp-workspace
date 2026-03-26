# Implementation Notes: req-001

## Sprint Planning

| Field | Value |
|-------|-------|
| Sprint | 1 |
| Target Release | v0.1.0 |
| Due Date | 2026-04-02 |
| Assigned To | igobyjama |
| Story Points | 8 |

---

## Branch

| Field | Value |
|-------|-------|
| Branch Name | `feature/req-001-project-foundation` |
| Base Branch | `main` |
| PR Target | `main` |

---

## Design Decisions

### Decision 1: JWT stored in httpOnly cookie

**Decision:** JWT issued as an httpOnly, SameSite=Strict cookie, not returned in the response body for localStorage.
**Alternatives considered:** localStorage (simpler client code); bearer token in Authorization header (stateless, explicit).
**Reason:** httpOnly cookie prevents XSS access to the token. SameSite=Strict blocks CSRF. No client-side JS needs to manage the token. Slightly more complex logout (clear cookie server-side) but worth the security gain for a personal finance tool.

### Decision 2: Prisma at monorepo root, not inside apps/api

**Decision:** `prisma/` directory lives at the monorepo root; `apps/api` imports the generated Prisma client.
**Alternatives considered:** Prisma inside `apps/api` only.
**Reason:** Keeps schema and migrations in one place. If a future CLI tool or script needs DB access it can share the client without coupling to the API package.

### Decision 3: Password hashing with bcrypt via `bcryptjs`

**Decision:** Use `bcryptjs` (pure JS, no native bindings) at cost factor 12.
**Alternatives considered:** `bcrypt` (native, faster); `argon2` (more modern, recommended by OWASP).
**Reason:** `bcryptjs` has zero native compilation issues across environments and Docker images. For a single-user personal tool the performance difference is irrelevant. Can be upgraded to argon2 later.

### Decision 4: Seed script creates initial user from env vars

**Decision:** `prisma/seed.ts` reads `SEED_USER_EMAIL` and `SEED_USER_PASSWORD` from env, upserts the user (idempotent).
**Alternatives considered:** Manual SQL insert; interactive CLI prompt.
**Reason:** Env-var driven seed works cleanly in Docker Compose and CI without interactive input. Upsert ensures re-running is safe.

### Decision 5: `packages/shared` scope

**Decision:** `packages/shared` exports only Zod schemas and derived TypeScript types. No runtime utilities.
**Alternatives considered:** Including API client helpers, constants, formatting utils.
**Reason:** Keep the shared package minimal for now. Adding utilities creates coupling — expand only when a concrete need arises from req-002+.

---

## Implementation Approach

1. Initialise pnpm workspace: root `package.json` with `workspaces`, `pnpm-workspace.yaml`
2. Scaffold `packages/shared`: Zod schemas for auth (`loginSchema`)
3. Scaffold `apps/api`: Fastify app, plugins (`@fastify/cookie`, `@fastify/env`), health route, auth routes, JWT preHandler hook
4. Scaffold `apps/web`: Vite + React + TanStack Router + TanStack Query, login page wired to auth API
5. Set up Prisma at root: `schema.prisma` (User model), `.env.example`, seed script
6. Configure ESLint (flat config) + Prettier at root, shared across packages
7. Write Vitest tests for auth endpoints (integration, using supertest against the Fastify instance)
8. Docker Compose dev file: postgres, api, web services with health checks
9. GitHub Actions CI workflow: install → lint → test → build

---

## Files to Change

### New Files

**Monorepo root**
- `package.json` — root workspace package with scripts
- `pnpm-workspace.yaml` — workspace package globs
- `tsconfig.base.json` — shared TS compiler options
- `eslint.config.ts` — flat ESLint config
- `.prettierrc` — Prettier config
- `.env.example` — all env vars documented
- `.gitignore` — node_modules, .env, dist, generated prisma

**Prisma**
- `prisma/schema.prisma` — datasource, generator, User model
- `prisma/seed.ts` — upsert initial user from env vars

**packages/shared**
- `packages/shared/package.json`
- `packages/shared/tsconfig.json`
- `packages/shared/src/index.ts`
- `packages/shared/src/schemas/auth.schema.ts` — `loginSchema` Zod schema

**apps/api**
- `apps/api/package.json`
- `apps/api/tsconfig.json`
- `apps/api/src/index.ts` — Fastify app bootstrap, plugin registration, listen
- `apps/api/src/app.ts` — app factory (exported for tests)
- `apps/api/src/plugins/env.ts` — `@fastify/env` schema + registration
- `apps/api/src/plugins/cookie.ts` — `@fastify/cookie` registration
- `apps/api/src/hooks/authenticate.ts` — JWT preHandler hook
- `apps/api/src/routes/health.routes.ts` — `GET /api/v1/health`
- `apps/api/src/routes/auth.routes.ts` — `POST /api/v1/auth/login`, `POST /api/v1/auth/logout`
- `apps/api/src/services/auth.service.ts` — login logic, JWT sign/verify
- `apps/api/src/lib/prisma.ts` — Prisma client singleton
- `apps/api/vitest.config.ts`
- `apps/api/src/__tests__/auth.test.ts` — integration tests for auth endpoints
- `apps/api/src/__tests__/health.test.ts`

**apps/web**
- `apps/web/package.json`
- `apps/web/tsconfig.json`
- `apps/web/vite.config.ts`
- `apps/web/index.html`
- `apps/web/src/main.tsx` — React + TanStack Router + Query bootstrap
- `apps/web/src/routeTree.gen.ts` — generated by TanStack Router
- `apps/web/src/routes/__root.tsx` — root layout
- `apps/web/src/routes/login.tsx` — login page, calls auth API
- `apps/web/src/routes/index.tsx` — protected home/dashboard stub
- `apps/web/src/lib/queryClient.ts` — TanStack Query client
- `apps/web/src/lib/api.ts` — typed fetch wrapper

**Docker**
- `docker/compose.dev.yml` — postgres + api + web

**CI**
- `.github/workflows/ci.yml` — lint → test → build

### Modified Files
- `repository/README.md` — replace placeholder with project setup instructions

---

## Test Coverage Plan

| AC | Test Type | Test Location |
|----|-----------|---------------|
| AC-001 | Manual smoke | `pnpm dev` |
| AC-002 | — | `pnpm test` itself verifies |
| AC-003 | — | `pnpm lint` itself verifies |
| AC-004 | Integration | `apps/api/src/__tests__/health.test.ts` |
| AC-005 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-006 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-007 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-008 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-009 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-010 | Integration | `apps/api/src/__tests__/auth.test.ts` |
| AC-011 | Manual smoke | `docker compose up` |
| AC-012 | CI | `.github/workflows/ci.yml` |
| AC-013 | Integration | `apps/api/src/__tests__/seed.test.ts` |

---

## Dependencies & Blockers

- [x] GitHub repo exists and Actions is enabled
- [x] Tech stack decisions resolved (see Design Decisions above)
- [x] JWT storage decision resolved: httpOnly cookie
