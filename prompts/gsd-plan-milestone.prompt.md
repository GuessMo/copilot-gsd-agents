---
agent: 'agent'
description: 'GSD: Plan new work as milestone-*.md files. Reads the current backlog, creates new open milestones. Usage: attach this prompt and describe the work to plan.'
tools: ['read', 'edit', 'search', 'agent']
---

You are running the **plan-phase** command — a compatibility alias for the GSD milestone workflow.

> This prompt creates milestones; the command name is retained for backwards compatibility only.

The user will describe what they want to plan.

## Steps

1. **Read the current backlog**
   - `.planning/README.md` — milestone naming rules and format
   - All existing `.planning/milestone-*.md` files — understand what is already open, current, or done
   - `.planning/REQUIREMENTS.md` if it exists — requirements context
   - `.planning/PROJECT.md` if it exists — stack and constraints
   - If `.planning/README.md` is missing in a legacy repo, have Bishop create a minimal compatible one before planning new milestones

2. **Plan** — Run the **Bishop** subagent with everything it needs in one call:
   > "Read the milestone backlog in `.planning/`, including `.planning/README.md`. If `.planning/README.md` is missing, create a minimal compatible model reference first. Create new `milestone-*.md` files using speaking names (e.g. `milestone-4-user-auth.md`, or an insertion like `milestone-2a-theme-worlds-fix.md`) for this work: [describe scope]. Split aggressively into the smallest sequential milestones that still deliver visible progress. Default target per milestone: 2–5 files, 2–4 checklist items, and one dominant change focus. If the work mixes UI structure, state/bindings, action migration, localization, or tests, create multiple insertion milestones instead of one broad step. Each milestone must have `status: open`, an unambiguous `done` criterion, and a `## To-dos` section with a Markdown checklist whose items are precisely derived from the scope — all entries start as `[ ]`. The `## To-dos` section appears between `## Action` and `## Verify`. Search the codebase for existing patterns inline — no separate research file needed."

   _(Researcher is skipped — Planner has search tools and handles it inline. Only run Ripley first if there is significant brownfield ambiguity.)_

3. **Review** — Present a summary of the created milestones to the user:
   - Which files were created and in what order
   - The `done` criterion of each milestone
   - Any insertion labels used (e.g. `milestone-2a.md`) and why

4. Tell the user:
   _"Milestones are planned and open in `.planning/`. Two options for the next step:_

   _**A — Optional: High Accuracy Plan-Review** — Ask Hicks to review the plan before any code is written: scope gaps, ambiguities, oversized milestones, weak `## Verify`/`## Done` criteria, incomplete `## To-dos`. No implementation, no tests — pure planning-quality check._

   _**B — Execute** — Run `gsd execute-phase` (or open the GSD orchestrator) to start working through the milestones."_
