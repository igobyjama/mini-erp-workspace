# Definition of Done

> Configure this file for your project. All requirements must satisfy this checklist before moving to 04_Complete.

## Standard Definition of Done

### Code Quality
- [ ] Code follows the standards in `coding-standards.md`
- [ ] No linting errors or warnings introduced
- [ ] No commented-out code left in the codebase
- [ ] No hardcoded secrets, credentials, or environment-specific values

### Testing
- [ ] All acceptance criteria have a corresponding automated test case
- [ ] All new and existing automated tests pass
- [ ] Smoke tests pass on the feature branch
- [ ] Test coverage meets the project minimum threshold (configure below)

### Review
- [ ] Code review completed by at least one reviewer who is not the author
- [ ] All review comments resolved or explicitly deferred with justification

### Documentation
- [ ] Feature documentation written in `documentation-req-NNN.md`
- [ ] API changes documented (OpenAPI or project-agreed format)
- [ ] Any configuration or environment variable changes documented

### Testing Sign-off
- [ ] Deployed to test environment successfully
- [ ] All UAT scenarios executed and marked PASS
- [ ] No open blocker or critical defects linked to this requirement

### Release Readiness
- [ ] Release notes entry written in `release-notes-req-NNN.md`
- [ ] PR created with `Closes #NNN` in the description
- [ ] No unresolved merge conflicts

---

## Project-Specific Thresholds

> Fill these in for your project:

| Metric | Threshold |
|--------|-----------|
| Minimum test coverage | <!-- e.g. 80% --> |
| Required reviewers | <!-- e.g. 1 --> |
| Performance regression limit | <!-- e.g. < 10% degradation --> |
