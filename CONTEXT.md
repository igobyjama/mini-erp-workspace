# Workspace Router

## Purpose

This file routes agent tasks to the correct stage. Read `CLAUDE.md` first for workspace identity and the full pipeline overview.

---

## Stage Routing

| If the task is... | Go to stage |
|-------------------|-------------|
| Creating a requirement from a GitHub issue | `01_Requirements/` |
| Writing code and documentation for an approved requirement | `02_Development/` |
| Writing test cases, running UAT, producing test reports | `03_Testing/` |
| Verifying DoD, writing release notes, preparing PR | `04_Complete/` |
| Archiving a merged and released requirement | `05_Published/` |

---

## Current Pipeline State

> Update this section as requirements move through the pipeline.

| req-NNN | Current Stage | GitHub Issue | Notes |
|---------|---------------|--------------|-------|
| req-001 | 03_Testing | [#1](https://github.com/igobyjama/mini-erp/issues/1) | Project Foundation: monorepo, DB, auth |
| req-002 | 01_Requirements | [#2](https://github.com/igobyjama/mini-erp/issues/2) | Contacts module: clients and vendors |
| req-003 | 01_Requirements | [#3](https://github.com/igobyjama/mini-erp/issues/3) | Finance module: invoices and expenses |
| req-004 | 01_Requirements | [#4](https://github.com/igobyjama/mini-erp/issues/4) | Projects and tasks module |

---

## Shared Resources

All stages may reference the following Layer 3 config files:

- [`_config/definition-of-done.md`](_config/definition-of-done.md) — DoD checklist
- [`_config/naming-conventions.md`](_config/naming-conventions.md) — branch names, file names, commits
- [`_config/tech-stack.md`](_config/tech-stack.md) — approved technologies
- [`_config/coding-standards.md`](_config/coding-standards.md) — code style and review checklist

---

## How to Start Work on a New Requirement

1. Read the GitHub issue
2. Go to `01_Requirements/`
3. Read `01_Requirements/CONTEXT.md` for the stage contract
4. Create `req-NNN/` using the templates in that folder
