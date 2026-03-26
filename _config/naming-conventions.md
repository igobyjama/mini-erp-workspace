# Naming Conventions

> Configure this file for your project. Referenced by Stage 2 (Development) and Stage 3 (Testing).

## Branch Names

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/req-NNN-short-description` | `feature/req-042-user-login` |
| Bug fix | `fix/req-NNN-short-description` | `fix/req-043-token-expiry` |
| Hotfix | `hotfix/req-NNN-short-description` | `hotfix/req-044-crash-on-start` |
| Release | `release/vX.Y.Z` | `release/v1.2.0` |

Rules:
- Always lowercase, words separated by hyphens
- Always include `req-NNN` so branches trace back to requirements
- Keep the description short (3–5 words max)

---

## Commit Messages

Format: `type(req-NNN): short description`

| Type | When to use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `refactor` | Code change that is not a feature or fix |
| `chore` | Build process, tooling, dependencies |

Example: `feat(req-042): add JWT authentication to login endpoint`

Rules:
- Subject line max 72 characters
- Use imperative mood ("add", not "added" or "adds")
- Reference issue in PR description, not in every commit

---

## File Names

> Configure for your project's language and framework conventions.

| Context | Convention | Example |
|---------|-----------|---------|
| <!-- e.g. React components --> | <!-- e.g. PascalCase.tsx --> | <!-- e.g. UserProfile.tsx --> |
| <!-- e.g. Utility modules --> | <!-- e.g. camelCase.ts --> | <!-- e.g. formatDate.ts --> |
| <!-- e.g. Test files --> | <!-- e.g. *.test.ts --> | <!-- e.g. userService.test.ts --> |

---

## Pull Request Titles

Format: `[req-NNN] Short description of the change`

Example: `[req-042] Add JWT authentication to login flow`

Rules:
- Always include `[req-NNN]`
- Always include `Closes #NNN` in the PR body to auto-close the GitHub issue on merge
