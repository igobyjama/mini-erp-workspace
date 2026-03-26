# AI Coding Pipeline Blueprint

A software development pipeline built on the **Model Workspace Protocol (MWP)** — an approach that replaces framework-level AI orchestration with filesystem structure. Folder hierarchy delivers context to a single AI agent at each stage. Human review gates control progression. Every artifact is a plain markdown file you can open, edit, and version.

Based on: *Interpretable Context Methodology: Folder Structure as Agent Architecture* (Van Clief, McDermott)

---

## How it works

A GitHub issue enters the pipeline as a `req-NNN/` folder. That folder moves through five stages, accumulating markdown artifacts at each step. A human approves every gate before the folder advances. One AI agent (Claude Code) reads stage-specific context at each step — no orchestration framework required.

```
GitHub Issue → 01_Requirements → 02_Development → 03_Testing → 04_Complete → 05_Published
                     ↓                  ↓               ↓              ↓
              req-NNN.md      +impl-notes.md    +test-cases.md  +release-notes.md
              criteria.md     +documentation.md +uat.md
                                                +test-report.md
```

---

## Repository structure

This blueprint uses a **two-repository model**: the pipeline workspace is one git repository, and the project code lives in an independent repository linked as a git submodule at `repository/`. The workspace contains only pipeline artifacts — CONTEXT files, req folders, and templates. All source code, branches, PRs, GitHub Actions, and releases live in the submodule repository.

```
my-project/                          ← workspace repo (pipeline context only)
│
├── CLAUDE.md                        ← Layer 0: workspace identity (read by Claude Code automatically)
├── CONTEXT.md                       ← Layer 1: pipeline router and current state
│
├── _config/                         ← Layer 3: stable project-wide reference material
│   ├── definition-of-done.md
│   ├── naming-conventions.md
│   ├── tech-stack.md
│   └── coding-standards.md
│
├── 01_Requirements/                 ← Stage 1
│   ├── CONTEXT.md
│   ├── _req-template.md
│   ├── _criteria-template.md
│   └── req-NNN/                     ← active requirement (moves forward after human approval)
│
├── 02_Development/                  ← Stage 2
│   ├── CONTEXT.md
│   ├── _implementation-notes-template.md
│   ├── _documentation-template.md
│   └── req-NNN/
│
├── 03_Testing/                      ← Stage 3 (sub-pipeline)
│   ├── CONTEXT.md
│   ├── 01_test-cases/
│   ├── 02_smoke-test/
│   ├── 03_install-to-test/
│   ├── 04_UAT/
│   ├── 05_output/
│   └── req-NNN/                     ← folder moves through sub-stages
│
├── 04_Complete/                     ← Stage 4
│   ├── CONTEXT.md
│   ├── _release-notes-template.md
│   ├── output/                      ← consolidated release notes per version
│   └── req-NNN/
│
├── 05_Published/                    ← Stage 5: archive, read-only
│   └── req-NNN/
│
└── repository/  [submodule]         ← independent code repo — source, tests, CI/CD, PRs, releases
    ├── src/
    ├── tests/
    └── ...
```

> **Why two repos?** The workspace and the codebase have different lifecycles and audiences. Pipeline artifacts (requirements, test reports, release notes) are workspace-internal records. Source code has its own branches, PR history, CI/CD, and release tags. Keeping them separate means the code repo stays clean and portable — it can be used independently, swapped out, or moved without affecting the pipeline workspace. GitHub Issues, Projects, PRs, and `Closes #NNN` all live in the submodule repository. `CLAUDE.md` at the workspace root is picked up automatically by Claude Code.

---

## Prerequisites

- [Claude Code](https://github.com/anthropics/claude-code) — the AI agent that executes each stage
- Git (with submodule support — standard in all modern Git versions)
- A GitHub account (for issue tracking and GitHub Projects)
- A separate GitHub repository for your project code (the submodule target)

---

## Setting up a new project

### 1. Create your workspace repository from this template

On GitHub: click **Use this template → Create a new repository**.

Clone it locally:

```bash
git clone https://github.com/your-org/your-project-pipeline.git
cd your-project-pipeline
```

### 2. Link your code repository as a submodule

Your project code lives in a separate repository. Add it as a submodule at `repository/`:

```bash
# Link an existing code repository
git submodule add https://github.com/your-org/your-project-code.git repository

# Or create a new empty repository on GitHub first, then link it
git submodule add https://github.com/your-org/your-project-code.git repository
```

Commit the submodule reference to the workspace:

```bash
git add .gitmodules repository
git commit -m "Add project code repository as submodule"
```

When cloning the workspace on a new machine, initialise the submodule:

```bash
git clone https://github.com/your-org/your-project-pipeline.git
cd your-project-pipeline
git submodule init && git submodule update
```

### 3. Configure the project-wide standards

Open each file in `_config/` and fill in your project's specifics. These are the stable reference files the AI agent loads at each stage.

| File | What to configure |
|------|------------------|
| `_config/tech-stack.md` | Runtime, language, frameworks, infrastructure |
| `_config/coding-standards.md` | Linter config, review checklist, security requirements |
| `_config/naming-conventions.md` | Branch naming pattern, commit format, PR title format |
| `_config/definition-of-done.md` | DoD checklist, minimum test coverage threshold |

### 4. Set up GitHub

All GitHub configuration below applies to the **submodule (code) repository**, not the workspace repository.

**Create the repository labels** used for pipeline tracking:

```
in-development      #0075ca
in-testing          #e4e669
ready-for-release   #d93f0b
```

**Create a GitHub Project** (Projects → New project → Board) with columns matching the pipeline stages. Link it to the submodule repository.

**Set the workspace repository as a template** (Settings → check "Template repository") so future projects can be created from it.

### 5. Open the workspace in Claude Code

```bash
cd your-project
claude
```

Claude Code reads `CLAUDE.md` automatically on startup. You are ready to work.

---

## Working with a requirement

### Starting a new requirement

1. Create a GitHub Issue describing the feature or bug
2. Note the issue number (e.g. `#42`)
3. In Claude Code, tell it:

   ```
   Create a requirement for GitHub issue #42
   ```

   Claude reads `CLAUDE.md` → `CONTEXT.md` → `01_Requirements/CONTEXT.md` and produces:
   - `01_Requirements/req-042/req-042.md`
   - `01_Requirements/req-042/criteria-req-042.md`

4. Review both files. Edit anything that is wrong or unclear.
5. When satisfied, move the folder forward:

   ```bash
   mv 01_Requirements/req-042 02_Development/req-042
   ```

   Then update the pipeline state table in `CONTEXT.md` and set the GitHub issue label to `in-development`.

### Moving through each stage

The pattern is the same at every stage:

```
Tell Claude which stage and which requirement
  → Claude reads the stage CONTEXT.md
  → Claude produces the required output files
  → You review and edit
  → You move the req-NNN/ folder to the next stage
  → You update the GitHub issue label
```

**Stage 2 — Development:**
```
Work on req-042 in development
```
Claude produces `implementation-notes-req-042.md` and `documentation-req-042.md` inside the existing `req-042/` folder, and writes code directly inside `repository/` (the submodule). All git operations for the code — branch creation, commits, and push — happen within `repository/` against its own remote. Review both the artifacts and the code, then move to `03_Testing/01_test-cases/`.

**Stage 3 — Testing (sub-pipeline):**

The requirement moves through five sub-stages. At each one, tell Claude where the requirement currently sits:

```
req-042 is in 03_Testing/01_test-cases — create test cases
```

The folder progresses: `01_test-cases → 02_smoke-test → 03_install-to-test → 04_UAT → 05_output`

Sub-stages 2 and 3 (`smoke-test`, `install-to-test`) are CI/CD gates — Claude does not produce files here. You confirm CI is green and deployment succeeded, then move the folder.

**Stage 4 — Complete:**
```
req-042 is ready for completion
```
Claude verifies the DoD checklist and produces `release-notes-req-042.md`.

**Release aggregation** (when all requirements for a version are in `04_Complete`):
```
Compile release notes for v1.2.0
```
Claude reads all `release-notes-req-NNN.md` files and produces `04_Complete/output/release-notes-v1.2.0.md`.

Review, then merge PRs and move all folders to `05_Published/`.

---

## The req folder lifecycle

```
01_Requirements/req-NNN/
  req-NNN.md
  criteria-req-NNN.md

02_Development/req-NNN/
  req-NNN.md
  criteria-req-NNN.md
  implementation-notes-req-NNN.md    ← added at this stage
  documentation-req-NNN.md           ← added at this stage

03_Testing/0X_.../req-NNN/
  ... all prior files ...
  test-cases-req-NNN.md              ← added at 01_test-cases
  uat-req-NNN.md                     ← added at 04_UAT
  test-report-req-NNN.md             ← added at 05_output

04_Complete/req-NNN/
  ... all prior files ...
  release-notes-req-NNN.md           ← added at this stage

05_Published/req-NNN/
  (complete, immutable record)
```

The folder **always moves as a unit**. Files are only added, never removed. By the time a requirement reaches `05_Published`, it carries the full audit trail of every decision made during its delivery.

---

## Context layers

Claude Code reads context in layers. Understanding this explains why the workspace performs well with scoped, focused inputs at each stage:

| Layer | File | Question answered |
|-------|------|------------------|
| 0 | `CLAUDE.md` | Where am I? What is this workspace? |
| 1 | `CONTEXT.md` | Where do I go? Which stage for this task? |
| 2 | `<stage>/CONTEXT.md` | What do I do? What are the inputs and outputs? |
| 3 | `_config/*.md` + stage templates | What rules and structure apply? |
| 4 | `req-NNN/*.md` | What am I working with right now? |

Each stage loads only the context it needs. The agent never sees instructions from other stages.

---

## GitHub integration

All GitHub activity (issues, labels, PRs, releases, GitHub Projects) lives in the **submodule (code) repository**.

| Event | Action |
|-------|--------|
| Requirement moves to `02_Development` | Set issue label `in-development` in submodule repo |
| Requirement moves to `03_Testing` | Set issue label `in-testing` in submodule repo |
| Requirement moves to `04_Complete` | Set issue label `ready-for-release` in submodule repo |
| PR merged to main in submodule repo | `Closes #NNN` in PR body auto-closes the issue |
| Moved to `05_Published` | Issue is closed, release tagged in submodule repo; workspace submodule pointer committed |

PR title format: `[req-NNN] Short description` (see `_config/naming-conventions.md`)

---

## Adapting this pipeline

**Adding a stage:** Create a new numbered folder, add a `CONTEXT.md` with Inputs / Process / Outputs / Human Gate sections, update `CLAUDE.md` and root `CONTEXT.md`.

**Changing a prompt:** Edit the relevant `CONTEXT.md` file. No code to redeploy.

**Changing a template:** Edit the relevant `_template.md` file. Takes effect on the next requirement that enters that stage.

**Skipping a stage for a specific requirement:** Move the req folder past it and note why in `implementation-notes-req-NNN.md`.

**Multiple requirements in parallel:** Each `req-NNN/` folder is independent. Any number can be in any stage simultaneously. The `03_Testing` stage is explicitly designed for batching — multiple requirements accumulate there before a release window closes.

