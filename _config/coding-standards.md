# Coding Standards

> Configure this file for your project. Referenced by Stage 2 (Development).

## General Principles

- Write code for the next developer to read, not for the compiler
- One function does one thing
- Prefer explicit over clever
- No magic numbers or strings — use named constants

---

## Code Style

> Specify your linter/formatter configuration here, or reference the config file in the repository.

| Tool | Config file | Notes |
|------|-------------|-------|
| <!-- e.g. ESLint --> | <!-- e.g. .eslintrc.json --> | |
| <!-- e.g. Prettier --> | <!-- e.g. .prettierrc --> | |

---

## Code Review Checklist

Reviewers must verify:

### Correctness
- [ ] The implementation satisfies the acceptance criteria in `criteria-req-NNN.md`
- [ ] Edge cases and error paths are handled
- [ ] No obvious security vulnerabilities (injection, XSS, auth bypass, exposed secrets)

### Quality
- [ ] No duplicated logic that should be extracted
- [ ] Functions and variables are named clearly
- [ ] Complex logic has a brief comment explaining why, not what

### Testing
- [ ] New code has test coverage
- [ ] Tests test behaviour, not implementation details
- [ ] Tests would catch a regression if the code breaks

### Standards
- [ ] Follows the conventions in `naming-conventions.md`
- [ ] No new dependencies added without justification
- [ ] No debug code, console logs, or TODOs left in

---

## Security Standards

> Configure for your project:

- All user input must be validated and sanitized
- Authentication and authorisation must be checked on every protected endpoint
- Secrets must not be committed to the repository — use environment variables
- <!-- Add project-specific security requirements -->

---

## Error Handling

> Configure for your project:

- <!-- e.g. All async functions must handle errors explicitly -->
- <!-- e.g. User-facing errors must not expose internal stack traces -->
- <!-- e.g. Errors must be logged with context -->
