---
agent: 'ask'
description: 'GSD: Show current project progress — phase status, next steps, open blockers.'
tools: ['read']
---

Read the following files from `.planning/` and present a clear progress summary:

1. **`.planning/ROADMAP.md`** — list all phases with their status (`[ ]` todo / `[x]` done)
2. **`.planning/STATE.md`** — show open questions and blockers
3. **`.planning/`** — check which plan and summary files exist to infer the current phase's execution state

## Output format

```
## GSD Progress

### Completed
- Phase 1: [Name] ✅
- ...

### In Progress
- Phase N: [Name] 🔄
  - Plans created: N-1-PLAN.md, N-2-PLAN.md
  - Executed: N-1-SUMMARY.md ✅
  - Pending: N-2-PLAN.md

### Upcoming
- Phase N+1: [Name] ⏳
- ...

### Blockers
- [from STATE.md]

### Next Step
[Single clear action to take]
```
