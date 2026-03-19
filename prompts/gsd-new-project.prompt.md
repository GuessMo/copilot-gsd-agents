---
agent: 'agent'
description: 'GSD: Initialize a new project or milestone backlog. Asks questions, maps the codebase if needed, and creates PROJECT.md, REQUIREMENTS.md, STATE.md, plus the first milestone-*.md files in .planning/.'
tools: ['read', 'edit', 'search', 'agent']
---

You are running the **GSD new-project** flow.

## Goal

Fully understand what should be built and create an initial milestone backlog for it.

## Steps

1. **Intake** — Ask the user targeted questions until you understand:
   - What they want to build (goal, user value)
   - Hard constraints (stack, integrations, deadlines)
   - What is explicitly **out of scope** for this version
   - Edge cases or known risks they're aware of

   Ask follow-up questions. Don't stop until the picture is clear.

2. **Map codebase (optional)** — Ask the user: _"Is this an existing codebase? If yes, I'll analyse it first."_
   Only if yes: run **Ripley** with the instruction: _"Analyse the full codebase: stack, architecture, conventions, key patterns. Pass findings back as context — no separate file needed unless the scope is large."_
   For greenfield projects, skip this step entirely.

3. **Create planning context files yourself** in `.planning/` (no subagent — you have all the information from the intake):

   **`README.md`**
   ```markdown
   # Planning README

   ## Purpose
   [Milestone model reference for naming, format, sizing, status values, and commit behavior]

   ## File Naming
   [Preferred `milestone-N-slug.md` and insertion rules like `milestone-2a-slug.md`]

   ## Required Milestone Format
   [YAML status header, Goal, Files, Action, To-dos, Verify, Done, Commit Message]

   ## Sizing Rules
   [2-5 files, 2-4 todos, one dominant focus; split umbrellas aggressively]

   ## Commit Behavior
   [Respect `.vscode/agent-session.json` and only commit with `autoCommit: true`]
   ```

   **`PROJECT.md`**
   ```markdown
   # Project: [Name]

   ## Vision
   [One paragraph]

   ## Stack & Constraints
   [Tech decisions, integrations, hard limits]

   ## Success Criteria
   [How we know v1 is done]
   ```

   **`REQUIREMENTS.md`**
   ```markdown
   # Requirements

   ## v1 — Must Have
   - R1: ...
   - R2: ...

   ## v2 — Nice to Have
   - ...

   ## Out of Scope
   - ...
   ```

   **`STATE.md`**
   ```markdown
   # State

   ## Decisions
   - [Key decisions made during intake]

   ## Open Questions
   - [Unresolved questions]

   ## Blockers
   - none
   ```

4. **Create the first milestone files** — Run **Bishop** with the intake results and context files:
   > "Create the first `milestone-*.md` files in `.planning/` for this new project using speaking names (e.g. `milestone-1-initial-setup.md`). Read `.planning/PROJECT.md`, `.planning/REQUIREMENTS.md`, and `.planning/README.md`. Start with `milestone-1-slug.md` and create as many open milestones as needed to cover the v1 scope (each covering a vertical slice). Status: `open`."

5. **Present the milestone backlog** to the user for approval. Adjust milestones if needed.

Once approved, tell the user:
_"Milestone backlog ready. Two options for the next step:_

_**A — Optional: High Accuracy Plan-Review** — Ask Hicks to review the plan before any code is written: scope gaps, ambiguities, oversized milestones, weak `## Verify`/`## Done` criteria, incomplete `## To-dos`. No implementation, no tests — pure planning-quality check._

_**B — Execute** — Run `gsd execute-phase` or open the GSD orchestrator to start working through the milestones."_

> **Legacy note:** This prompt previously created a `ROADMAP.md` with numbered phases. The active path is now `milestone-*.md` files directly. `ROADMAP.md` files from earlier projects are preserved as legacy context but are not created for new projects.
