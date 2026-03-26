# Stage 1: Requirements

## Role

Transform a GitHub issue into structured requirement and acceptance criteria documents. This stage produces the foundation that all downstream stages depend on — do not proceed if the requirement is ambiguous.

---

## Inputs

| Layer | File | Purpose |
|-------|------|---------|
| Layer 4 (working) | GitHub issue text or URL | Source of truth for the requirement |
| Layer 3 (reference) | `./_req-template.md` | Structure for `req-NNN.md` |
| Layer 3 (reference) | `./_criteria-template.md` | Structure for `criteria-req-NNN.md` |

---

## Process

1. Determine the requirement number NNN from the GitHub issue number (zero-padded to three digits)
2. Create folder `req-NNN/` inside `01_Requirements/`
3. Create `req-NNN.md` from `_req-template.md` — populate all sections from the issue content
4. Create `criteria-req-NNN.md` from `_criteria-template.md` — derive acceptance criteria strictly from what the issue describes
5. Do not invent scope, requirements, or acceptance criteria not present in the issue
6. Flag any ambiguities in the **Notes** section of `req-NNN.md` rather than making assumptions

---

## Outputs

Both files are written into `01_Requirements/req-NNN/`:

| File | Description |
|------|-------------|
| `req-NNN.md` | Full requirement definition |
| `criteria-req-NNN.md` | Acceptance criteria and Definition of Done checklist |

---

## Human Gate

Human reviews both files before this stage is considered complete.

Approval actions:
- Move the entire `req-NNN/` folder to `02_Development/req-NNN/`
- Update issue label to `in-development` on GitHub
- Update the pipeline state table in the root `CONTEXT.md`
