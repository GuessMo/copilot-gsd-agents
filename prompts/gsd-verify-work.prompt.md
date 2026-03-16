---
agent: 'agent'
description: 'GSD: Interactive verification of milestone done-criteria. Reads milestone-*.md files, walks through deliverables, diagnoses failures. Usage: attach this prompt and optionally name the milestone(s) to verify.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

You are running the **GSD verify-work** flow (user acceptance testing).

The user will specify which milestone(s) to verify, or leave it to you to infer. Check the `status` of each milestone:
- `status: done` or `current` → use the **Implementation Verify** flow below
- `status: open` (no code written yet) → use **Plan-Review Mode** (see end of this prompt)

## Steps

1. **Extract deliverables** — Read the relevant `.planning/milestone-*.md` files and collect each `## Done` criterion into a checklist

2. **Show all deliverables at once** — List all `## Done` criteria in a single message and ask the user to mark which pass / fail. Don't ask one at a time.

3. **Diagnose failures** — Run one **Hicks** call for all failing criteria together (not one per criterion):
   > "Diagnose why the following criterion is not met: [criterion]. Milestone: `.planning/milestone-N.md`. Examine the relevant code and tests."

   Present the diagnosis to the user.

4. **Create `.planning/VERIFICATION.md`** (or append to an existing one):

   ```markdown
   ## Verification — [Date or Milestone Range]

   | Milestone | Deliverable | Status | Notes |
   |-----------|-------------|--------|-------|
   | milestone-N.md | [done criterion] | ✅/❌ | ... |

   ## Issues Found
   [Detailed description of what failed and why]

   ## Fix Milestones
   [If issues found: concrete tasks — create new milestone-*.md files for them]
   ```

5. **Offer re-execution** — If issues were found, ask:
   > _"Fix milestones created. Run `gsd execute-phase` to apply them?"_

If everything passes: congratulate the user and suggest adding the next work items as `milestone-*.md`.

> **Legacy note:** This prompt previously read `N-M-PLAN.md` files. It now reads `milestone-*.md` files exclusively. Legacy plan files may still be referenced for historical context but are not part of the active verification path.

---

## Plan-Review Mode (open milestones)

Use this mode when the provided milestone(s) have `status: open` — no implementation has been written yet.

### Steps

1. **Read each open `milestone-*.md`** — collect `## Goal`, `## Action`, `## Verify`, `## Done`, and `## To-dos`
2. **Run one Hicks call** in Plan-Review Mode:
   > "Review the following open milestone(s) for planning quality: [paths]. Check scope completeness, ambiguity, milestone size, `## Verify` quality, `## Done` quality, and `## To-dos` completeness. Do not run tests or inspect implementation."
3. **Present the findings** to the user. For each milestone:
   - State overall: **READY** or **NEEDS REVISION**
   - List per-criterion findings (scope completeness, ambiguity, milestone size, `## Verify`/`## Done`/`## To-dos` quality)
   - Provide concrete revision suggestions where needed
4. **Offer next step** after the review:
   > _"Plan review complete. Run `gsd execute-phase` to start implementing, or ask me to revise specific milestones first."_

> **No `VERIFICATION.md` entry for plan reviews** — findings are reported in the chat only. Do not create fix milestones in this mode.
