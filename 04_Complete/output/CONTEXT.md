# Stage 4 Output: Release Notes Aggregation

## Role

Collect the individual `release-notes-req-NNN.md` files from all requirements currently in `04_Complete` and compile them into a single, publishable release notes document for the upcoming version.

This sub-stage runs once per release — when all requirements for the release are in `04_Complete` and approved.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `../req-NNN/release-notes-req-NNN.md` (for each req in this release) | Individual release notes entries to aggregate |
| Layer 3 (reference) | `./_release-notes-vX.Y.Z-template.md` | Structure for the consolidated release document |

---

## Process

1. List all `req-NNN/` folders currently in `04_Complete`
2. Read each `release-notes-req-NNN.md`
3. Group requirements by type (Feature / Bug Fix / Improvement / Breaking Change)
4. Create `release-notes-vX.Y.Z.md` with:
   - Version number and release date
   - All changes grouped by type
   - All breaking changes and migration steps consolidated into one section
   - GitHub issue references for each entry
5. The document must be ready to publish as-is — no raw template placeholders remaining

---

## Outputs

| File | Description |
|------|-------------|
| `release-notes-vX.Y.Z.md` | Consolidated, publishable release notes for the version |

---

## Human Gate

Human reviews the consolidated release notes for accuracy and completeness.

Approval actions:
- Move all `req-NNN/` folders in `04_Complete` to `05_Published/`
- Publish `release-notes-vX.Y.Z.md` to the project changelog or release page
- Tag the release in the repository (`git tag vX.Y.Z`)
- Merge PRs to main (auto-closes all linked GitHub issues)
- Update the pipeline state table in the root `CONTEXT.md`
