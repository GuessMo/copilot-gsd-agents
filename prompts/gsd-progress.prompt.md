---
agent: 'ask'
description: 'GSD: Show current project progress — milestone backlog grouped by Current/Open/Done, plus blockers. Shows todo checklist for the current milestone and todo counters for open milestones.'
tools: ['read']
---

Read the following files from `.planning/` and present a clear progress summary:

1. **All `.planning/milestone-*.md`** — sorted by numeric stem, then alphabetic suffix. For each, read the `status` frontmatter field, the milestone name, and the `## To-dos` section (if present).
2. **`.planning/STATE.md`** — show open questions and blockers

## Output format

```
## GSD Progress

### Current
- milestone-N-slug.md: [Name] 🔄
  To-dos:
  - [x] Erledigter Schritt
  - [ ] Noch offener Schritt
  (all todo lines from ## To-dos shown with their [x]/[ ] state)

### Open
- milestone-N-slug.md: [Name] ⏳  [2/5 To-dos erledigt]
    offene Punkte: Schritt C, Schritt D, Schritt E
- milestone-N-slug.md: [Name] ⏳  [0/3 To-dos erledigt]
  (for open milestones: show N/M counter; if compact, also list the open [ ] items)

### Done
- milestone-N-slug.md: [Name] ✅
- ...

### Blocked
- milestone-N-slug.md: [Name] 🚫 — [blocker description]

### Blockers (from STATE.md)
- [open questions and blockers]

### Next Step
[Single clear action to take]
```

**To-do display rules:**
- **Current milestone:** Show every entry of the `## To-dos` list with its exact `[x]` or `[ ]` state.
- **Open milestones:** Show a counter (`N/M To-dos erledigt`) next to the name. If the milestone has open items and the output remains compact, also list those open `[ ]` items beneath.
- **Done/Blocked milestones:** No todo detail needed — a count summary is fine.
- If a milestone has no `## To-dos` section, omit the todo lines silently.

> **Note:** Legacy files (`1-1-PLAN.md`, `1-2-PLAN.md`, etc.) are preserved but not part of the active backlog. Do not include them in the progress output. Milestone files in legacy bare-number format (`milestone-N.md`, `milestone-Na.md`) are still active backlog files — include them sorted by numeric stem then alpha suffix.
