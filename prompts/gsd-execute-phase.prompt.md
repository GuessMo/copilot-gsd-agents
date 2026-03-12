---
agent: 'agent'
description: 'GSD: Execute phase N. Groups plans into dependency waves, runs Xenomorph per plan, then verifies. Usage: attach this prompt and tell the agent which phase number.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

You are running the **GSD execute-phase** flow.

The user will tell you which phase number to execute (e.g. "execute phase 1" or just "1").

## Steps

1. **Read plans** — Find all `.planning/N-M-PLAN.md` files for phase N

2. **Build dependency waves**
   - Inspect the `depends-on` attribute of each plan
   - Wave 1: plans with no dependencies
   - Wave 2: plans whose dependencies are in Wave 1
   - Continue until all plans are assigned

3. **Execute wave by wave**
   For each wave, run one **Xenomorph** subagent per plan:
   > "Execute the plan at `.planning/N-M-PLAN.md`. Create `.planning/N-M-SUMMARY.md`."

   Wait for all plans in a wave to complete before starting the next wave.

4. **Update ROADMAP.md** — Mark phase N status as `[x]` (done)

5. **Self-check** — Read all `N-M-SUMMARY.md` files and compare against `<done>` criteria from the plans:
   - All criteria met with no reported blockers → write a brief `.planning/N-VERIFICATION.md` (status: PASS) yourself
   - Any blocker or failed criterion → run **Hicks** for that specific plan only:
     > "Verify plan `.planning/N-M-PLAN.md`. The executor reported this blocker: [describe]. Diagnose and append fix tasks to `.planning/N-VERIFICATION.md`."

6. **Report results**
   - If PASS: report success, tell user next step
   - If issues: show the open issues and ask whether to create fix plans and re-execute

Tell the user:
_"Phase N executed. Next: `gsd verify N` for interactive UAT, or proceed to `gsd plan-phase N+1`."_
