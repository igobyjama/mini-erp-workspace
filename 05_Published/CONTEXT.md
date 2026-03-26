# Stage 5: Published

## Role

Archive for requirements that have been merged to main, released, and whose GitHub issue has been closed. This folder is read-only — nothing is edited here. It is the audit trail of completed work.

---

## What lives here

Each `req-NNN/` folder in this stage is the complete, immutable record of a delivered requirement:

```
req-NNN/
  req-NNN.md                        ← original requirement definition
  criteria-req-NNN.md               ← acceptance criteria and DoD checklist
  implementation-notes-req-NNN.md   ← sprint plan, branch, design decisions
  documentation-req-NNN.md          ← feature/API documentation
  test-cases-req-NNN.md             ← test case descriptions
  uat-req-NNN.md                    ← UAT scenarios and results
  test-report-req-NNN.md            ← consolidated test report
  release-notes-req-NNN.md          ← release notes entry
```

---

## When a requirement arrives here

- PR has been merged to main
- GitHub issue has been closed (auto-closed via `Closes #NNN` in PR)
- Release has been tagged or shipped
- Root `CONTEXT.md` pipeline state table updated to reflect completion

---

## No Human Gate

This is a terminal stage. There is no further action required. Requirements are never moved out of this folder.
