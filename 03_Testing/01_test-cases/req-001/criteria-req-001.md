# Acceptance Criteria: req-001

## Acceptance Criteria

### AC-001: Monorepo starts with a single command

**Given** the repository is cloned and `pnpm install` has been run
**When** the developer runs `pnpm dev` from the monorepo root
**Then** both `apps/api` and `apps/web` start concurrently, the API is reachable at `http://localhost:3000`, the web app is reachable at `http://localhost:5173`, and both hot-reload on file changes

---

### AC-002: Test suite runs with a single command

**Given** the monorepo is set up and dependencies are installed
**When** the developer runs `pnpm test` from the monorepo root
**Then** Vitest runs across all packages and exits with code 0 (all tests pass)

---

### AC-003: Lint passes with zero errors

**Given** the codebase is in its committed state
**When** the developer runs `pnpm lint` from the monorepo root
**Then** ESLint exits with code 0 and reports zero errors or warnings

---

### AC-004: Health endpoint is publicly accessible

**Given** `apps/api` is running
**When** a GET request is made to `http://localhost:3000/api/v1/health` without any auth header
**Then** the response is HTTP 200 with body `{ "status": "ok" }`

---

### AC-005: Login with valid credentials returns a JWT

**Given** a user exists in the database with email `admin@example.com` and a known password (created via seed script)
**When** a POST request is made to `/api/v1/auth/login` with `{ "email": "admin@example.com", "password": "<correct password>" }`
**Then** the response is HTTP 200 and sets an httpOnly cookie containing a signed JWT

---

### AC-006: Login with invalid credentials is rejected

**Given** a user exists in the database
**When** a POST request is made to `/api/v1/auth/login` with an incorrect password
**Then** the response is HTTP 401 with body `{ "error": "Invalid credentials" }` and no cookie is set

---

### AC-007: Login with unknown email is rejected

**Given** the database contains no user with a given email
**When** a POST request is made to `/api/v1/auth/login` with that email
**Then** the response is HTTP 401 with body `{ "error": "Invalid credentials" }` (same message — no user enumeration)

---

### AC-008: Protected route is inaccessible without a JWT

**Given** `apps/api` is running
**When** a request is made to any route under `/api/v1/` (except `/auth/login`, `/auth/logout`, `/health`) without an auth cookie
**Then** the response is HTTP 401 with body `{ "error": "Unauthorized" }`

---

### AC-009: Protected route is accessible with a valid JWT

**Given** a valid JWT cookie has been obtained via a successful login
**When** a request is made to a protected route with that cookie
**Then** the response is HTTP 200 (or appropriate success code for that route)

---

### AC-010: Expired or tampered JWT is rejected

**Given** a JWT cookie with an invalid signature or past expiry
**When** a request is made to a protected route with that cookie
**Then** the response is HTTP 401 with body `{ "error": "Unauthorized" }`

---

### AC-011: Docker Compose brings up the full stack

**Given** Docker and Docker Compose are installed and a valid `.env` file is present
**When** `docker compose -f docker/compose.dev.yml up` is run
**Then** all three services (`postgres`, `api`, `web`) start without error, health checks pass, and the API health endpoint responds at `http://localhost:3000/api/v1/health`

---

### AC-012: CI pipeline passes on push

**Given** a commit is pushed to any branch on GitHub
**When** the GitHub Actions workflow triggers
**Then** the lint, test, and build steps all complete successfully and the workflow run is green

---

### AC-013: Seed script creates initial user

**Given** a running Postgres instance and environment variables `SEED_USER_EMAIL` and `SEED_USER_PASSWORD` are set
**When** `pnpm prisma:seed` is run
**Then** a user record is created in the `users` table with the specified email and a bcrypt hash of the password, and the script is idempotent (re-running does not create a duplicate)

---

## Definition of Done

### Standard DoD
- [ ] All acceptance criteria above have a corresponding automated test case
- [ ] All automated tests pass (`pnpm test` exits 0)
- [ ] Code reviewed and approved
- [ ] Feature documentation written (`documentation-req-001.md`)
- [ ] No open blocker issues linked to this requirement
- [ ] UAT scenarios executed and signed off (`uat-req-001.md`)
- [ ] Release notes entry written (`release-notes-req-001.md`)

### Requirement-Specific DoD
- [ ] `pnpm dev` confirmed working end-to-end (api + web + postgres)
- [ ] `docker compose -f docker/compose.dev.yml up` confirmed working on a clean clone
- [ ] `.env.example` reviewed — all variables documented and no secrets committed
- [ ] Prisma seed script confirmed idempotent
- [ ] JWT httpOnly cookie approach confirmed and documented in `implementation-notes-req-001.md`
- [ ] GitHub Actions workflow run is green on the feature branch
