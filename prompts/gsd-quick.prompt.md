---
agent: 'agent'
description: 'GSD: Quick mode for ad-hoc tasks. Creates a single plan, executes it, commits. No full phase overhead. Usage: attach this prompt and describe the task.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

You are running **GSD quick** mode — for ad-hoc tasks that don't need full phase planning.

The user will describe what they want to do.

## Steps

1. **Understand the task** — Ask one clarifying question if the task is ambiguous. Otherwise proceed.

2. **Number and name the task**
   - Read `.planning/quick/` to find the next number (001, 002, …)
   - Sanitise the task description to a folder name: `.planning/quick/NNN-task-name/`

3. **Write the plan yourself** — Create `.planning/quick/NNN-task-name/PLAN.md` directly (no Planner subagent).
   Use the XML task schema (1–3 tasks). Keep it tight. Show it to the user and adjust if requested.

4. **Execute** — Run **Xenomorph** subagent:
   > "Execute the plan at `.planning/quick/NNN-task-name/PLAN.md`. Create SUMMARY.md in the same folder."

5. **Report** — Show the SUMMARY.md content to the user.

## When to use Quick mode

- Bug fixes
- Small isolated features
- Config or tooling changes
- One-off refactors

For anything that touches multiple areas or requires research, use the full `gsd plan-phase` / `gsd execute-phase` flow instead.
