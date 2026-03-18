---
agent: 'agent'
description: 'gsd quick — compatibility alias for inserting and immediately executing a single milestone-*.md in the main backlog. No separate workflow. Usage: attach this prompt and describe the task.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

> **Compatibility alias:** `gsd quick` is a shortcut for inserting and immediately executing a single milestone in the main backlog. It is not a separate planning workflow — new quick work always lands in `.planning/milestone-N.md`.

You are the **`gsd quick`** compatibility alias — a shortcut for inserting and immediately executing a single milestone in the main backlog, not a separate planning workflow.

The user will describe what they want to do.

## Steps

1. **Understand the task** — Ask one clarifying question if the task is ambiguous. Otherwise proceed.

2. **Assess scope**
   - **Trivial / single-step** (a commit, rename, one-liner fix, status check): execute directly without a plan. **Stop here.**
   - **Non-trivial** (more than one distinct step): continue to step 3.

3. **Create small milestone slices in the main backlog** — Read the existing `.planning/milestone-*.md` files to find the next free slot (or an insertion label like `2a`). If the task still spans more than one dominant change focus, use **Bishop** to create 1–3 small sequential `milestone-N-slug.md` files instead of one broad milestone. Default target per milestone: 2–5 files and 2–4 checklist items. Only write a single milestone directly when the task clearly fits in one focused execution pass. Show the created milestone(s) to the user and adjust if requested.

4. **Execute** — Run **Xenomorph** only for the first approved small milestone:
   > "Execute the milestone at `.planning/milestone-N-slug.md` (use the actual filename). Respect `.vscode/agent-session.json` for the commit decision. If the milestone is still too coarse, stop and request further splitting instead of forcing the whole task through one run."

5. **Report** — Show the Summary section from the executed milestone file to the user and list any remaining open follow-up milestones.

## When to use `gsd quick`

- Bug fixes
- Small isolated features
- Config or tooling changes
- One-off refactors

For work that spans multiple areas or requires research, use `gsd plan-phase` / `gsd execute-phase` instead. Prefer two or three small milestones over one large quick milestone.

> **Legacy note:** Quick mode previously stored plans in `.planning/quick/NNN-task-name/PLAN.md`. That structure is no longer the active path — new quick work lands directly in `.planning/milestone-N-slug.md` (using a speaking name). Bare number names like `milestone-N.md` remain valid for legacy compatibility.
