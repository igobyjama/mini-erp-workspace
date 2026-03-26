# Sub-Stage 3.3: Install to Test Environment

## Role

Merge the feature branch to the test branch and confirm deployment to the test environment. This is a CI/CD gate — no new artifacts are written by the agent here. The human confirms deployment status.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/implementation-notes-req-NNN.md` | Branch name and PR target |

---

## Process

No agent output is produced at this sub-stage. The steps are:

1. Create a PR from the feature branch to the test/develop branch in the **submodule repository**
2. Complete code review on the PR (reviewer must not be the author)
3. Merge the PR — this triggers the CI/CD pipeline defined in the submodule repository's GitHub Actions
4. Confirm the test environment deployment completed successfully
5. Confirm the feature is accessible in the test environment at the expected URL or endpoint

If deployment fails: do not proceed — resolve the deployment issue first.

> Note: All PRs, GitHub Actions workflows, and deployments are part of the submodule repository, not the workspace repository.

---

## Outputs

No new files added to `req-NNN/` at this sub-stage.

---

## Human Gate

Human confirms the feature is deployed and accessible in the test environment.

Approval actions:
- Move `req-NNN/` to `04_UAT/req-NNN/`
