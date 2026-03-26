# Sub-Stage 3.4: UAT

## Role

Create UAT scenarios from acceptance criteria and execute them against the test environment. The agent creates the UAT scenario document; the human executes the scenarios and records results.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/req-NNN.md` | User stories and functional requirements |
| Layer 4 (working) | `req-NNN/criteria-req-NNN.md` | Acceptance criteria to validate |
| Layer 3 (reference) | `../_uat-template.md` | Structure for `uat-req-NNN.md` |

---

## Process

**Agent does:**
1. Create `uat-req-NNN.md` from `_uat-template.md`
2. Write one UAT scenario per acceptance criterion — scenarios must be executable by a non-developer in the test environment
3. Each scenario must have: precondition, numbered steps, expected result

**Human does (after agent):**
4. Execute each UAT scenario in the test environment
5. Record actual results and PASS/FAIL status in `uat-req-NNN.md`
6. Log any defects found as new GitHub issues, linked to req-NNN

---

## Outputs

Appended to `req-NNN/` folder:

| File | Description |
|------|-------------|
| `uat-req-NNN.md` | UAT scenarios with preconditions, steps, expected results, and human-filled actual results |

---

## Human Gate

Human completes UAT execution and signs off all scenarios as PASS.

If any scenario FAILS: do not proceed — return the defect to `02_Development` as a new linked issue.

Approval actions:
- Move `req-NNN/` to `05_output/req-NNN/`
