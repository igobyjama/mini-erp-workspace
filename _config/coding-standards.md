# Coding Standards

## General Principles

- Write code for the next developer to read, not for the compiler
- One function does one thing
- Prefer explicit over clever
- No magic numbers or strings — use named constants

---

## Code Style

| Tool | Config file | Notes |
|------|-------------|-------|
| ESLint | `eslint.config.ts` (flat config) | Shared across workspaces via `packages/shared` |
| Prettier | `.prettierrc` | Single quotes, 2-space indent, trailing commas |
| TypeScript | `tsconfig.json` | `strict: true`, `noUncheckedIndexedAccess: true` |

---

## TypeScript Rules

- Never use `any` — use `unknown` and narrow explicitly
- All Fastify route handlers must have typed request/reply via Zod schemas
- Prisma types must not leak into HTTP layer — map to DTO types defined in `packages/shared`
- Avoid type assertions (`as Foo`) unless wrapping an external library boundary

---

## Backend Conventions (Fastify)

- Routes registered via `fastify.register()` with a prefix — one file per resource
- Business logic lives in service functions, not route handlers
- Route handlers only: validate input → call service → return response
- All database access goes through Prisma service layer, never raw SQL unless documented
- All async route handlers must `await` — no floating promises

---

## Frontend Conventions (React)

- Components: PascalCase, one component per file
- Hooks: `use` prefix, camelCase (`useProjects.ts`)
- All API calls through TanStack Query — no `fetch` directly in components
- Form schemas defined in `packages/shared` so they match backend Zod schemas exactly
- No `console.log` in production code

---

## File Names

| Context | Convention | Example |
|---------|-----------|---------|
| React components | `PascalCase.tsx` | `ProjectCard.tsx` |
| Hooks | `useCamelCase.ts` | `useProjects.ts` |
| Fastify route files | `camelCase.routes.ts` | `contacts.routes.ts` |
| Service files | `camelCase.service.ts` | `projects.service.ts` |
| Test files | `*.test.ts` or `*.test.tsx` | `projects.service.test.ts` |
| Zod schema files | `camelCase.schema.ts` | `contact.schema.ts` |

---

## Code Review Checklist

### Correctness
- [ ] The implementation satisfies the acceptance criteria in `criteria-req-NNN.md`
- [ ] Edge cases and error paths are handled
- [ ] No obvious security vulnerabilities (injection, auth bypass, exposed secrets)

### Quality
- [ ] No duplicated logic that should be extracted
- [ ] Functions and variables are named clearly
- [ ] Complex logic has a brief comment explaining why, not what

### Testing
- [ ] New code has test coverage
- [ ] Tests test behaviour, not implementation details
- [ ] Tests would catch a regression if the code breaks

### Standards
- [ ] Follows naming conventions above
- [ ] No new dependencies added without justification in implementation notes
- [ ] No debug code, console logs, or TODOs left in

---

## Security Standards

- All user input validated by Zod at the route layer before reaching services
- JWT must be verified on every protected route via Fastify auth hook
- Secrets (DB URL, JWT secret) in environment variables only — never hardcoded
- Prisma parameterised queries only — never string-concatenated SQL
- HTTP responses must not expose internal error messages or stack traces to the client

---

## Error Handling

- All async Fastify handlers use `try/catch` or Fastify's `setErrorHandler`
- Service functions throw typed errors — route handlers catch and map to HTTP status codes
- User-facing error responses: `{ error: string }` — no stack traces
- Server errors are logged with context (route, user id, request id) via pino
