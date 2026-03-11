# copilot-gsd-agents

Centralised GitHub Copilot agent definitions for the **GSD (Get Shit Done)** workflow.

This repository is consumed as a **Git submodule** inside any project that uses the GSD agent stack.
It is mounted at `.github/agents/` so VS Code Copilot picks up all agent files automatically.

## Agents

| File | Agent | Role |
|------|-------|------|
| `gsd.orchestrator.agent.md` | 🧠 Mother (GSD Coordinator) | Central orchestration — spawns all other agents |
| `gsd.researcher.agent.md` | 🔎 Ripley (GSD Researcher) | Brownfield investigation, produces RESEARCH.md |
| `gsd.planner.agent.md` | ♟️ Bishop (GSD Planner) | Decomposes phases into atomic PLAN.md files |
| `gsd.executor.agent.md` | 👾 Xenomorph (GSD Executor) | Implements exactly one PLAN.md, no improvisation |
| `gsd.verifier.agent.md` | ✅ Hicks (GSD Verifier) | Ground-truth verification against done criteria |
| `gsd.integration.agent.md` | 🔗 Ash (GSD Integration) | Bridge to Jira, Confluence, and Figma via MCP |

## Usage as submodule

```bash
# Add to a new project
git submodule add https://github.com/GuessMo/copilot-gsd-agents.git .github/agents

# After cloning a project that already uses this submodule
git submodule update --init
```

## Update agents across all projects

```bash
# Inside a consumer project
cd .github/agents
git pull origin main
cd ../..
git add .github/agents
git commit -m "chore(agents): update GSD agent submodule"
```

## Remote

Canonical remote: `https://github.com/GuessMo/copilot-gsd-agents.git`
