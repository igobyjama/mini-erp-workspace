# Sub-Stage 3.1: Test Cases

## Role

Write the automated test case descriptions for the requirement. These descriptions cover unit tests, integration tests, and smoke test scenarios. Actual test code lives in the repository — this document defines what must be tested and why.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/req-NNN.md` | Functional requirements to derive tests from |
| Layer 4 (working) | `req-NNN/criteria-req-NNN.md` | Acceptance criteria — every AC needs at least one test |
| Layer 4 (working) | `req-NNN/implementation-notes-req-NNN.md` | Files to change, test coverage plan |
| Layer 3 (reference) | `../_test-cases-template.md` | Structure for `test-cases-req-NNN.md` |

---

## Process

1. Read the acceptance criteria — each AC-NNN must map to at least one test case
2. Create `test-cases-req-NNN.md` from `_test-cases-template.md`
3. Write unit test descriptions for each function/component being changed
4. Write integration test descriptions for end-to-end flows
5. Mark which tests are smoke tests (critical path, run on every deploy)
6. Cross-check: the test coverage plan in `implementation-notes-req-NNN.md` should align

---

## Outputs

Appended to `req-NNN/` folder:

| File | Description |
|------|-------------|
| `test-cases-req-NNN.md` | Full test case descriptions |

---

## Human Gate

Human reviews test cases for completeness against acceptance criteria.

Approval actions:
- Move `req-NNN/` to `02_smoke-test/req-NNN/`
