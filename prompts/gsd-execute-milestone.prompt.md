agent: 'agent'
description: 'GSD: Execute the next open or current milestone. Linear milestone execution through the backlog. Usage: attach this prompt to work through the milestone backlog.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

You are running the **execute-phase** command — a compatibility alias for the GSD milestone workflow.

> This prompt executes the next open milestone; the command name is retained for backwards compatibility only.

## Steps

1. **Read the backlog** — Find all `.planning/milestone-*.md` files, sorted by numeric stem then alphabetic suffix. Both speaking names (`milestone-N-slug.md`) and legacy bare-number names (`milestone-N.md`) are accepted and sorted the same way.

2. **Identify the next candidate**
   - If a milestone has `status: current` → that is the candidate (resume interrupted work)
   - Otherwise → the first milestone with `status: open`
   - If nothing is `current` or `open`: report that the backlog is complete

3. **Execute the candidate milestone** — Run one **Xenomorph** subagent:
   > "Execute the milestone at `.planning/milestone-N-slug.md` (use the actual filename of the candidate; both speaking names and legacy bare-number names are valid). Set its frontmatter `status` to `current` before starting. Actively update the `## To-dos` checklist during execution: mark each step `[x]` as soon as it is complete. Set `status: done` only when **both** the fachliche Done-Kriterium is met **and** no `[ ]` entries remain in the `## To-dos` list. Respect `.vscode/agent-session.json` for the commit decision."

4. **Check outcome** — Read the Summary section appended to the milestone file:
   - Done criterion met → confirm `done`, report success
   - Blocker reported → surface the specific blocker to the user, run **Hicks** for that milestone only:
     > "Verify milestone `.planning/milestone-N-slug.md` (use the actual filename). The executor reported this blocker: [describe]. Diagnose and report."

5. **Offer to continue** — If more `open` milestones remain, ask the user whether to proceed to the next one

Tell the user:
_"Milestone done. Next: run `gsd execute-phase` again for the next open milestone, or `gsd verify` to check the done criteria interactively."_
