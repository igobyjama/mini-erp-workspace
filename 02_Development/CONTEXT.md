# Stage 2: Development

## Role

Write the code and produce all technical documentation for an approved requirement. The agent implements the feature directly in the `repository/` directory at the workspace root, then records the decisions and documents the result. The human actor reviews all code and artifacts, correcting or updating them as needed before approving the stage gate.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | `req-NNN/req-NNN.md` | Requirement definition from Stage 1 |
| Layer 4 (working) | `req-NNN/criteria-req-NNN.md` | Acceptance criteria to implement against |
| Layer 3 (reference) | `./_implementation-notes-template.md` | Structure for `implementation-notes-req-NNN.md` |
| Layer 3 (reference) | `./_documentation-template.md` | Structure for `documentation-req-NNN.md` |
| Layer 3 (reference) | `../../_config/tech-stack.md` | Approved technologies and versions |
| Layer 3 (reference) | `../../_config/coding-standards.md` | Code style, linting, review standards |
| Layer 3 (reference) | `../../_config/naming-conventions.md` | Branch names, file names, commit messages |

---

## Process

1. Read `req-NNN.md` and `criteria-req-NNN.md` fully before producing any output
2. Create `implementation-notes-req-NNN.md` from `_implementation-notes-template.md`:
   - Define sprint, due date, target release
   - Derive the branch name from `naming-conventions.md`
   - List all files that will be changed or created in the repository
   - Record key design decisions and the reasoning behind them
3. Write the code — implement the feature in the `repository/` directory following `coding-standards.md` and `tech-stack.md`
   - `repository/` is a git submodule: create the feature branch, commit, and push **within that repo** using its own remote — not the workspace remote
   - The workspace repo does not need its own feature branch; only `repository/` does
   - The human actor may correct or update any code before the stage gate is approved
4. Create `documentation-req-NNN.md` from `_documentation-template.md`:
   - Document all new or changed APIs in the project-agreed format (e.g. OpenAPI)
   - Document any configuration or environment changes
   - Write usage examples for the feature
5. Cross-check: every acceptance criterion in `criteria-req-NNN.md` must be addressed by the implementation

---

## Outputs

Both files are appended into the existing `req-NNN/` folder:

| File | Description |
|------|-------------|
| `implementation-notes-req-NNN.md` | Sprint plan, branch, design decisions, files to change |
| `documentation-req-NNN.md` | Feature and API documentation |

---

## Human Gate

Human reviews both files and all code changes before this stage is considered complete.

Approval actions:
- Confirm the feature branch is pushed to the submodule repository's remote
- Move the entire `req-NNN/` folder to `03_Testing/01_test-cases/req-NNN/`
- Update issue label to `in-testing` on GitHub
- Update the pipeline state table in the root `CONTEXT.md`
