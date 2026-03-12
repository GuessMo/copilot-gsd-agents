---
agent: 'agent'
description: 'GSD: Initialize a new project or milestone. Asks questions, maps the codebase, and creates PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md in .planning/.'
tools: ['read', 'edit', 'search', 'agent']
---

You are running the **GSD new-project** flow.

## Goal

Fully understand what should be built and create a structured, phased roadmap for it.

## Steps

1. **Intake** — Ask the user targeted questions until you understand:
   - What they want to build (goal, user value)
   - Hard constraints (stack, integrations, deadlines)
   - What is explicitly **out of scope** for this version
   - Edge cases or known risks they're aware of

   Ask follow-up questions. Don't stop until the picture is clear.

2. **Map codebase (optional)** — Ask the user: _"Is this an existing codebase? If yes, I'll analyse it first."_
   Only if yes: run **Ripley** with the instruction: _"Analyse the full codebase: stack, architecture, conventions, key patterns. Create `.planning/CODEBASE.md`._"
   For greenfield projects, skip this step entirely.

3. **Create planning files yourself** in `.planning/` (no subagent — you have all the information from the intake):

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

   **`ROADMAP.md`**
   ```markdown
   # Roadmap

   ## Phase 1: [Name] [ ]
   [1–2 sentences: what gets built, what requirements it covers]

   ## Phase 2: [Name] [ ]
   ...
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

4. **Present the roadmap** to the user for approval. Adjust phases if needed.

Once approved, tell the user:
_"Roadmap approved. Next: use `gsd plan-phase 1` or open the GSD agent and type `gsd plan-phase 1`."_
