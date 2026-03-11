---
agent: 'agent'
description: 'GSD: Interactive user acceptance testing for phase N. Walks through deliverables, diagnoses failures, creates fix plans. Usage: attach this prompt and tell the agent which phase number.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

You are running the **GSD verify-work** flow (user acceptance testing).

The user will tell you which phase number to verify.

## Steps

1. **Extract deliverables** — Read all `.planning/N-M-PLAN.md` files and collect each `<done>` criterion into a checklist

2. **Show all deliverables at once** — List all `<done>` criteria in a single message and ask the user to mark which pass / fail. Don't ask one at a time.

3. **Diagnose failures** — Run one **Hicks** call for all failing criteria together (not one per criterion):
   > "Diagnose why the following criterion is not met: [criterion]. Phase N, plan M. Examine the relevant code and tests."

   Present the diagnosis to the user.

4. **Create `.planning/N-UAT.md`**

   ```markdown
   ## Phase N — User Acceptance Testing

   | Deliverable | Status | Notes |
   |-------------|--------|-------|
   | [criterion] | ✅/❌  | ...   |

   ## Issues Found
   [Detailed description of what failed and why]

   ## Fix Plans
   [If issues found: concrete tasks to fix them]
   ```

5. **Offer re-execution** — If issues were found, ask:
   > _"Fix plans created. Run `gsd execute-phase N` to apply them?"_

If everything passes: congratulate the user and suggest `gsd plan-phase N+1`.
