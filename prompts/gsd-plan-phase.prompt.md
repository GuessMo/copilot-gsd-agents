---
agent: 'agent'
description: 'GSD: Plan phase N. Uses ♟️ Bishop for targeted codebase research and atomic PLAN.md files, then presents them for review. Usage: attach this prompt and tell the agent which phase number.'
tools: ['read', 'edit', 'search', 'agent']
---

You are running the **GSD plan-phase** flow.

The user will tell you which phase number to plan (e.g. "plan phase 1" or just "1").

## Steps

1. **Read context**
   - `.planning/ROADMAP.md` — find the phase description
   - `.planning/REQUIREMENTS.md` — note which requirements this phase covers
   - `.planning/PROJECT.md` — stack and constraints
   - `.planning/N-CONTEXT.md` if it exists — user preferences for this phase

2. **Plan** — Run the **♟️ Bishop (GSD Planner)** subagent with everything it needs in one call:
   > "Create atomic PLAN.md files for Phase N: [phase description]. Read `.planning/REQUIREMENTS.md` and `.planning/PROJECT.md`. Search the codebase for existing patterns relevant to this phase. Create `.planning/N-1-PLAN.md`, `.planning/N-2-PLAN.md`, … (2–4 plans). No separate research file needed — embed findings directly in the plan actions."

   _(🔎 Ripley is skipped here — ♟️ Bishop has search tools and handles the targeted research inline.)_

3. **Review** — Present a summary of the created plans to the user:
   - How many plans were created
   - What each plan covers
   - Which plans depend on others (execution order)

4. Tell the user:
   _"Phase N is planned. Next: `gsd execute-phase N` or open the GSD agent and type `gsd execute-phase N`."_
