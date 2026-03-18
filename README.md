# copilot-gsd-agents

Centralised GitHub Copilot configuration for the **GSD (Get Shit Done)** workflow.

This repository is consumed as a **Git submodule** mounted at `.github/` inside any consumer project.
VS Code Copilot then picks up all agent and prompt files automatically.

## Structure

```text
agents/    — GSD agent definitions (*.agent.md)
prompts/   — GSD prompt files (*.prompt.md)
```

## Execution model

- Planning is intentionally biased toward small sequential milestones instead of broad umbrella steps.
- Default target size per milestone: 2–5 files, 2–4 checklist items, one dominant change focus.
- Before execution, Mother and the execute prompt run a preflight. Oversized milestones are sent back to Bishop for decomposition instead of being forced through Xenomorph.
- Xenomorph now treats oversized milestones as a planning blocker and escalates them for splitting before coding begins.

## Agents

| File | Agent | Role |
| ---- | ----- | ---- |
| `agents/gsd.orchestrator.agent.md` | 🧠 Mother (GSD Coordinator) | Central orchestration — spawns all other agents |
| `agents/gsd.researcher.agent.md` | 🔎 Ripley (GSD Researcher) | Brownfield investigation, produces RESEARCH.md |
| `agents/gsd.planner.agent.md` | ♟️ Bishop (GSD Planner) | Decomposes phases into atomic PLAN.md files |
| `agents/gsd.executor.agent.md` | 👾 Xenomorph (GSD Executor) | Implements exactly one PLAN.md, no improvisation |
| `agents/gsd.verifier.agent.md` | ✅ Hicks (GSD Verifier) | Ground-truth verification against done criteria |
| `agents/gsd.integration.agent.md` | 🔗 Ash (GSD Integration) | Bridge to Jira, Confluence, and Figma via MCP |

## Prompts

| File | Description |
| ---- | ----------- |
| `prompts/gsd-new-project.prompt.md` | Initialize a new project or milestone |
| `prompts/gsd-plan-phase.prompt.md` | Plan a phase — research + atomic PLAN.md files |
| `prompts/gsd-execute-phase.prompt.md` | Execute a phase — dependency waves + verification |
| `prompts/gsd-verify-work.prompt.md` | Interactive user acceptance testing |
| `prompts/gsd-quick.prompt.md` | Quick mode for ad-hoc tasks |
| `prompts/gsd-progress.prompt.md` | Show current project progress |

## Usage as submodule

```bash
# Add to a new project (mounts the entire .github/ directory)
git submodule add https://github.com/GuessMo/copilot-gsd-agents.git .github

# After cloning a project that already uses this submodule
git submodule update --init
```

## Update across all projects

```bash
# Inside a consumer project
cd .github
git pull origin main
cd ..
git add .github
git commit -m "chore(agents): update GSD submodule"
```

## Remote

Canonical remote: `https://github.com/GuessMo/copilot-gsd-agents.git`
