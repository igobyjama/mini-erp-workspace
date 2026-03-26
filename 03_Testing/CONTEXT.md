# Stage 3: Testing

## Role

Testing is a sub-pipeline with five sequential steps. The `req-NNN/` folder moves through each sub-stage, accumulating test artifacts. Multiple requirements can be in this stage simultaneously and are batched for release together.

---

## Sub-Stage Overview

| Sub-Stage | Folder | What happens | Human Gate |
|-----------|--------|--------------|------------|
| 1 | `01_test-cases/` | Write automated test cases from requirements | Approve test cases |
| 2 | `02_smoke-test/` | Run smoke tests on development branch | Confirm CI green |
| 3 | `03_install-to-test/` | Merge to test branch, deploy to test environment | Confirm deployment |
| 4 | `04_UAT/` | Execute UAT scenarios in test environment | Sign off UAT |
| 5 | `05_output/` | Write consolidated test report | Approve report, move to Complete |

---

## Inputs (at stage entry)

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/` folder from `02_Development` | All prior artifacts: req, criteria, implementation notes, documentation |
| Layer 3 (reference) | `./_test-cases-template.md` | Structure for `test-cases-req-NNN.md` |
| Layer 3 (reference) | `./_uat-template.md` | Structure for `uat-req-NNN.md` |
| Layer 3 (reference) | `./_test-report-template.md` | Structure for `test-report-req-NNN.md` |
| Layer 3 (reference) | `../../_config/definition-of-done.md` | DoD checklist to verify |

---

## Sub-Stage Routing

Read the CONTEXT.md in the sub-stage folder where `req-NNN` currently resides for the specific contract for that step.

- `req-NNN` in `01_test-cases/` → read `01_test-cases/CONTEXT.md`
- `req-NNN` in `02_smoke-test/` → read `02_smoke-test/CONTEXT.md`
- `req-NNN` in `03_install-to-test/` → read `03_install-to-test/CONTEXT.md`
- `req-NNN` in `04_UAT/` → read `04_UAT/CONTEXT.md`
- `req-NNN` in `05_output/` → read `05_output/CONTEXT.md`

---

## Outputs Added in This Stage

All files are appended to the `req-NNN/` folder:

| File | Added at sub-stage |
|------|--------------------|
| `test-cases-req-NNN.md` | `01_test-cases` |
| `uat-req-NNN.md` | `04_UAT` |
| `test-report-req-NNN.md` | `05_output` |

---

## Final Human Gate

After `05_output/`, human approves the test report and DoD checklist.

Approval actions:
- Move the entire `req-NNN/` folder to `04_Complete/req-NNN/`
- Update issue label to `ready-for-release` on GitHub
- Update the pipeline state table in the root `CONTEXT.md`
