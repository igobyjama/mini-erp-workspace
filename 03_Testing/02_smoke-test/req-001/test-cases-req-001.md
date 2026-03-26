# Test Cases: req-001

## Coverage Map

| AC | Test ID(s) | Type |
|----|-----------|------|
| AC-001 | TC-010 | Smoke (manual) |
| AC-002 | TC-011 | Smoke (manual) |
| AC-003 | TC-012 | Smoke (manual) |
| AC-004 | TC-001 | Integration |
| AC-005 | TC-002 | Integration |
| AC-006 | TC-003 | Integration |
| AC-007 | TC-004 | Integration |
| AC-008 | TC-005 | Integration |
| AC-009 | TC-006 | Integration |
| AC-010 | TC-007, TC-008 | Integration |
| AC-011 | TC-013 | Smoke (manual) |
| AC-012 | TC-014 | Smoke (CI) |
| AC-013 | TC-009 | Unit |

---

## Unit Tests

### TC-001: Health endpoint returns 200 with correct body

**Tests AC:** AC-004
**Component/Function:** `healthRoutes` — `GET /api/v1/health`
**Scenario:** Request made with no auth cookie
**Input:** `GET /api/v1/health` — no headers, no cookie
**Expected Output:** HTTP 200, body `{ "status": "ok" }`
**Test File:** `apps/api/src/__tests__/health.test.ts`

---

### TC-009: Seed script creates a user and is idempotent

**Tests AC:** AC-013
**Component/Function:** `prisma/seed.ts`
**Scenario:** Seed run twice with the same email — second run must not create a duplicate
**Input:** `SEED_USER_EMAIL=admin@example.com`, `SEED_USER_PASSWORD=testpassword`
**Expected Output:**
- After first run: one user row with `email = admin@example.com` and a valid bcrypt hash in `password_hash`
- After second run: still exactly one user row (upsert is idempotent)
**Test File:** `apps/api/src/__tests__/seed.test.ts`

---

## Integration Tests

### TC-002: Login with valid credentials — JWT cookie set

**Tests AC:** AC-005
**Flow:** Client POSTs valid credentials → server verifies password → returns 200 + httpOnly cookie
**Precondition:** A user exists in the (mocked) data store with email `admin@example.com` and a known bcrypt hash
**Steps:**
1. POST `{ "email": "admin@example.com", "password": "correctpassword" }` to `/api/v1/auth/login`
2. Inspect response status and `set-cookie` header
**Expected Result:** HTTP 200; `set-cookie` header contains `token=<jwt>; HttpOnly`; response body `{ "ok": true }`
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-003: Login with wrong password — rejected with 401

**Tests AC:** AC-006
**Flow:** Client POSTs correct email but wrong password → server rejects
**Precondition:** User exists with a known password hash
**Steps:**
1. POST `{ "email": "admin@example.com", "password": "wrongpassword" }` to `/api/v1/auth/login`
2. Inspect response status, body, and headers
**Expected Result:** HTTP 401; body `{ "error": "Invalid credentials" }`; no `set-cookie` header
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-004: Login with unknown email — same 401 as wrong password (no enumeration)

**Tests AC:** AC-007
**Flow:** Client POSTs an email that does not exist → server returns identical 401 (no user enumeration)
**Precondition:** No user with email `nobody@example.com` in the data store
**Steps:**
1. POST `{ "email": "nobody@example.com", "password": "anything" }` to `/api/v1/auth/login`
2. Compare response to TC-003 response
**Expected Result:** HTTP 401; body `{ "error": "Invalid credentials" }` — identical to wrong-password response
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-005: Protected route without token — 401

**Tests AC:** AC-008
**Flow:** Unauthenticated client hits a protected route → auth hook rejects before reaching handler
**Precondition:** No cookie present in request
**Steps:**
1. Send `GET /api/v1/protected-test-route` with no cookie
2. Inspect response status and body
**Expected Result:** HTTP 401; body `{ "error": "Unauthorized" }`
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-006: Protected route with valid token — passes auth hook

**Tests AC:** AC-009
**Flow:** Authenticated client hits a protected route with a valid JWT cookie → request reaches handler
**Precondition:** A valid signed JWT cookie is present
**Steps:**
1. Obtain valid JWT by calling login (TC-002 flow)
2. Send request to protected route with the JWT cookie
3. Inspect response — expect not 401
**Expected Result:** Response is not HTTP 401 (auth hook passed; handler responds normally)
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-007: Protected route with tampered JWT — 401

**Tests AC:** AC-010
**Flow:** Client sends a JWT cookie with an invalid signature → `jwtVerify` throws → 401
**Precondition:** Token cookie set to `tampered.jwt.token`
**Steps:**
1. Send `GET /api/v1/protected-test-route` with cookie `token=tampered.jwt.token`
2. Inspect response status and body
**Expected Result:** HTTP 401; body `{ "error": "Unauthorized" }`
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

### TC-008: Protected route with expired JWT — 401

**Tests AC:** AC-010
**Flow:** Client sends a JWT cookie that was signed with the correct secret but has an `exp` in the past
**Precondition:** A JWT signed with the test secret and `exp = now - 1 second`
**Steps:**
1. Craft an expired JWT using `jose` with the known test secret
2. Send request to protected route with that cookie
3. Inspect response
**Expected Result:** HTTP 401; body `{ "error": "Unauthorized" }`
**Test File:** `apps/api/src/__tests__/auth.test.ts`

---

## Smoke Tests

### [SMOKE] TC-010: `pnpm dev` starts both servers

**Tests AC:** AC-001
**Purpose:** Confirms the monorepo dev workflow works end-to-end on a clean clone — the most critical daily-use path
**What it verifies:**
- `pnpm dev` exits with a non-error startup (both processes start)
- API responds at `http://localhost:3000/api/v1/health`
- Web app serves HTML at `http://localhost:5173`
- Changing a source file triggers hot reload (observed manually)
**Test File:** Manual execution — no automated test file

---

### [SMOKE] TC-011: `pnpm test` exits 0

**Tests AC:** AC-002
**Purpose:** Verifies the test runner is wired correctly across all monorepo packages
**What it verifies:**
- `pnpm test --run` exits with code 0
- Output shows passing counts for both `apps/api` tests
**Test File:** Manual execution / CI output

---

### [SMOKE] TC-012: `pnpm lint` exits 0

**Tests AC:** AC-003
**Purpose:** Verifies ESLint flat config is valid and all committed code passes linting
**What it verifies:**
- `pnpm lint` exits with code 0
- Zero errors and zero warnings reported
**Test File:** Manual execution / CI output

---

### [SMOKE] TC-013: Docker Compose brings up the full stack

**Tests AC:** AC-011
**Purpose:** Confirms the containerised dev environment works for onboarding and environment parity
**What it verifies:**
- `docker compose -f docker/compose.dev.yml up` completes without errors
- `postgres` service passes its health check (`pg_isready`)
- `api` service passes its health check (`/api/v1/health` returns 200)
- `web` service starts and serves HTML on port 5173
**Test File:** Manual execution

---

### [SMOKE] TC-014: GitHub Actions CI workflow passes

**Tests AC:** AC-012
**Purpose:** The CI pipeline is the automated gate for every future commit — it must be green before this req is considered done
**What it verifies:**
- All steps succeed: `pnpm install` → `prisma generate` → `prisma migrate deploy` → `pnpm lint` → `pnpm test` → `pnpm build`
- Workflow run shows green on the `feature/req-001-project-foundation` branch
**Test File:** GitHub Actions — [igobyjama/mini-erp Actions](https://github.com/igobyjama/mini-erp/actions)
