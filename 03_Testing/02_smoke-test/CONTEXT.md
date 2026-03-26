# Sub-Stage 3.2: Smoke Test

## Role

Verify that the automated smoke tests pass on the feature branch before merging to the test environment. This is a CI/CD gate — no new artifacts are written by the agent here. The human confirms CI status.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/test-cases-req-NNN.md` | Reference: which tests are marked as smoke tests |
| Layer 4 (working) | `req-NNN/implementation-notes-req-NNN.md` | Branch name to check CI against |

---

## Process

No agent output is produced at this sub-stage. The steps are:

1. Confirm the feature branch named in `implementation-notes-req-NNN.md` exists in the **submodule repository** (`repository/`) remote
2. Confirm CI pipeline has run on that branch
3. Confirm all smoke tests (marked in `test-cases-req-NNN.md`) are passing
4. If CI is failing: do not proceed — return to `02_Development` to resolve the failure

---

## Outputs

No new files added to `req-NNN/` at this sub-stage.

---

## Human Gate

Human confirms CI is green on the feature branch.

Approval actions:
- Move `req-NNN/` to `03_install-to-test/req-NNN/`
