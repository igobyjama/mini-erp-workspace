# Sub-Stage 3.5: Test Output

## Role

Consolidate all test results into a single test report and verify the Definition of Done checklist is complete. This is the final gate before the requirement is marked ready for release.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/test-cases-req-NNN.md` | Automated test results reference |
| Layer 4 (working) | `req-NNN/uat-req-NNN.md` | UAT results |
| Layer 4 (working) | `req-NNN/criteria-req-NNN.md` | DoD checklist to verify |
| Layer 3 (reference) | `../_test-report-template.md` | Structure for `test-report-req-NNN.md` |
| Layer 3 (reference) | `../../_config/definition-of-done.md` | Project-wide DoD |

---

## Process

1. Create `test-report-req-NNN.md` from `_test-report-template.md`
2. Summarise automated test results (pass/fail counts, coverage)
3. Summarise UAT results from `uat-req-NNN.md`
4. List any open issues found during testing and their resolution status
5. Verify the DoD checklist in `criteria-req-NNN.md` — mark each item checked or flag it as incomplete
6. Write an overall PASS or FAIL verdict

If any DoD item is incomplete or any issue is unresolved: flag it clearly — do not mark PASS.

---

## Outputs

Appended to `req-NNN/` folder:

| File | Description |
|------|-------------|
| `test-report-req-NNN.md` | Consolidated test report with DoD verification and overall verdict |

---

## Human Gate

Human reviews the test report and DoD checklist. All items must be checked and verdict must be PASS.

Approval actions:
- Move `req-NNN/` to `04_Complete/req-NNN/`
- Update issue label to `ready-for-release` on GitHub
- Update the pipeline state table in the root `CONTEXT.md`
