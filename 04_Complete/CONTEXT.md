# Stage 4: Complete

## Role

Verify the Definition of Done is fully satisfied, write the release notes entry, and confirm the PR is ready for merge to main. This stage is the final quality gate before a requirement enters the release.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/criteria-req-NNN.md` | DoD checklist — must be fully checked |
| Layer 4 (working) | `req-NNN/test-report-req-NNN.md` | Test report — verdict must be PASS |
| Layer 4 (working) | `req-NNN/implementation-notes-req-NNN.md` | Branch name and PR target for verification |
| Layer 4 (working) | `req-NNN/documentation-req-NNN.md` | Feature documentation to summarise in release notes |
| Layer 3 (reference) | `./_release-notes-template.md` | Structure for `release-notes-req-NNN.md` |
| Layer 3 (reference) | `../../_config/definition-of-done.md` | Project-wide DoD to cross-check |

---

## Process

1. Read `test-report-req-NNN.md` — if verdict is not PASS, stop and flag to human; do not proceed
2. Read `criteria-req-NNN.md` — verify every DoD item is checked; flag any unchecked items
3. Create `release-notes-req-NNN.md` from `_release-notes-template.md`:
   - Write a user-facing description of the change
   - Reference the GitHub issue number
   - Note any breaking changes or migration steps
4. Verify the PR exists in the submodule repository with `Closes #NNN` in the description (to auto-close the issue on merge)

---

## Outputs

Appended to `req-NNN/` folder:

| File | Description |
|------|-------------|
| `release-notes-req-NNN.md` | Release notes entry ready to be included in the project changelog |

---

## Human Gate

Human reviews DoD completeness and the individual release notes entry for this requirement.

Approval actions (per requirement):
- Requirement stays in `04_Complete/req-NNN/` until the release window closes
- Update the pipeline state table in the root `CONTEXT.md`

---

## Release Aggregation (when all requirements for a release are approved)

When the release window closes and all requirements for the version are approved in this stage, run the release aggregation step:

1. Go to `output/` and read `output/CONTEXT.md`
2. Compile all individual `release-notes-req-NNN.md` files into `output/release-notes-vX.Y.Z.md`
3. Human approves the consolidated release notes

Final approval actions:
- Move all `req-NNN/` folders to `05_Published/`
- Publish `output/release-notes-vX.Y.Z.md` to changelog / release page
- Merge all PRs to main in the **submodule repository** (auto-closes GitHub issues)
- Tag the release in the **submodule repository**
- Update the workspace submodule pointer to the release commit and commit it to the workspace repository
- Update the pipeline state table in the root `CONTEXT.md`
