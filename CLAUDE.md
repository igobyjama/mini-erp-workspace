# AI Coding Pipeline Workspace

## What this workspace is

This workspace implements the **Model Workspace Protocol (MWP)** for a software development pipeline. It structures the delivery of context to an AI agent across sequential development stages, with human review gates between each stage.

Each requirement travels as a folder (`req-NNN/`) through the pipeline, accumulating artifacts at each stage. The folder is never split — it moves as a unit after human approval.

> **Roles:** The AI agent writes all code and technical implementations during the Development stage. The human actor reviews agent output, corrects or updates it as needed, and is the final authority at every stage gate.

> **Repository directory:** `repository/` is a **git submodule** pointing to an independent remote repository. All production code and technical implementation files live there. The workspace repo contains only pipeline artifacts (planning docs, test reports, release notes) — these are never mixed with source code. When cloning this workspace, run `git submodule init && git submodule update` to initialise `repository/`.

---

## Pipeline Stages

| Stage | Folder | Role | Human Gate |
|-------|--------|------|------------|
| 1 | `01_Requirements/` | Transform GitHub issue into structured requirement + acceptance criteria | Approve before moving to dev |
| 2 | `02_Development/` | Agent writes code in `repository/`; produces implementation notes and feature docs | Approve before moving to testing |
| 3 | `03_Testing/` | Test case creation, smoke tests, install to test env, UAT, test report | Approve before marking complete |
| 4 | `04_Complete/` | Definition of Done verification, release notes, PR readiness | Approve before publishing |
| 5 | `05_Published/` | Archive — PR merged, issue closed, release shipped | Final state |

---

## Req Folder Lifecycle

A `req-NNN/` folder accumulates files as it moves through the pipeline:

```
After 01_Requirements:
  req-NNN/
    req-NNN.md                        ← requirement definition
    criteria-req-NNN.md               ← acceptance criteria

After 02_Development (appended):
    implementation-notes-req-NNN.md   ← planning, branch, design decisions
    documentation-req-NNN.md          ← feature/API documentation

After 03_Testing (appended):
    test-cases-req-NNN.md             ← unit + integration + smoke test descriptions
    uat-req-NNN.md                    ← UAT scenarios and results
    test-report-req-NNN.md            ← consolidated test report

After 04_Complete (appended):
    release-notes-req-NNN.md          ← release notes entry

05_Published: full folder archived here
```

---

## Naming Conventions

- Requirement folders: `req-NNN` (lowercase, three-digit zero-padded, e.g. `req-001`, `req-042`)
- Requirement files: `req-NNN.md`, `criteria-req-NNN.md`, etc. (all lowercase, hyphenated)
- NNN matches the GitHub issue number where possible
- Templates are prefixed with `_` (e.g. `_req-template.md`) so they sort to the top and are never confused with live req files

---

## Layer Structure

This workspace follows a five-layer context hierarchy:

- **Layer 0** — this file (`CLAUDE.md`): workspace identity and navigation
- **Layer 1** — `CONTEXT.md` (root): task routing across stages
- **Layer 2** — `<stage>/CONTEXT.md`: stage-specific contracts (inputs, process, outputs, human gate)
- **Layer 3** — `_config/` and stage `references/`: stable reference material (tech stack, standards, conventions)
- **Layer 4** — `req-NNN/` working artifacts: per-requirement files that change each run

---

## Shared Configuration

Global reference material lives in `_config/`. Each stage CONTEXT.md declares which config files it loads.

```
_config/
  definition-of-done.md       ← DoD checklist all requirements must satisfy
  naming-conventions.md        ← branch names, file names, commit messages
  tech-stack.md                ← approved technologies, versions, package sources
  coding-standards.md          ← code style, linting rules, review checklist
```

---

## Project Monitoring

All requirements are tracked as GitHub Issues in the **submodule repository** (`repository/`). That is where GitHub Issues, Projects, PRs, and release tags live — not in the workspace repository. GitHub Projects provides the kanban view. Stage movement in this workspace should be reflected in the GitHub issue status.

- Moving to `02_Development` → set issue label `in-development`
- Moving to `03_Testing` → set issue label `in-testing`
- Moving to `04_Complete` → set issue label `ready-for-release`
- Moving to `05_Published` → close issue via PR (auto-close with `Closes #NNN` in PR description)
