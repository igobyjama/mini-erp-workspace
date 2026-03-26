# Definition of Done

All requirements must satisfy this checklist before moving to `04_Complete`.

## Standard Definition of Done

### Code Quality
- [ ] Code follows the standards in `coding-standards.md`
- [ ] No linting errors or warnings (`pnpm lint` passes)
- [ ] No commented-out code left in the codebase
- [ ] No hardcoded secrets, credentials, or environment-specific values

### Testing
- [ ] All acceptance criteria have a corresponding automated test case
- [ ] All new and existing automated tests pass (`pnpm test` passes)
- [ ] Smoke tests pass on the feature branch
- [ ] Test coverage meets project minimum (see thresholds below)

### Review
- [ ] Self-review completed against coding standards checklist
- [ ] All review comments resolved or explicitly deferred with justification

### Documentation
- [ ] Feature documentation written in `documentation-req-NNN.md`
- [ ] API changes documented (OpenAPI via Fastify JSON Schema / Zod-to-OpenAPI)
- [ ] Any new environment variables documented in `.env.example`

### Testing Sign-off
- [ ] Deployed to test environment (local Docker Compose) successfully
- [ ] All UAT scenarios executed and marked PASS in `uat-req-NNN.md`
- [ ] No open blocker or critical defects linked to this requirement

### Release Readiness
- [ ] Release notes entry written in `release-notes-req-NNN.md`
- [ ] PR created against `main` with `Closes #NNN` in the description
- [ ] No unresolved merge conflicts

---

## Project-Specific Thresholds

| Metric | Threshold |
|--------|-----------|
| Minimum test coverage | 70% |
| Required reviewers | 1 (human review of AI output) |
| Performance regression limit | < 10% degradation on existing endpoints |
