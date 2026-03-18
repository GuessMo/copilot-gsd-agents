agent: 'agent'
description: 'GSD: Führt den nächsten offenen oder aktuellen Meilenstein aus. Lineare Abarbeitung des Backlogs. Verwendung: Diesen Prompt anhängen, um das Meilenstein-Backlog abzuarbeiten.'
tools: ['read', 'edit', 'execute', 'search', 'agent']
---

Du führst den Befehl **execute-phase** aus — ein Kompatibilitätsalias für den GSD-Meilenstein-Workflow.

> Dieser Prompt führt den nächsten offenen Meilenstein aus; der Befehlsname bleibt nur aus Gründen der Rückwärtskompatibilität erhalten.

## Steps

1. **Read the backlog** — Find all `.planning/milestone-*.md` files, sorted by numeric stem then alphabetic suffix. Both speaking names (`milestone-N-slug.md`) and legacy bare-number names (`milestone-N.md`) are accepted and sorted the same way.

2. **Identify the next candidate**
   - If a milestone has `status: current` → that is the candidate (resume interrupted work)
   - Otherwise → the first milestone with `status: open`
   - If nothing is `current` or `open`: report that the backlog is complete

3. **Preflight the candidate**
   - Read the candidate milestone completely before execution
   - If it is too large for one focused execution pass, do **not** run Xenomorph yet. Use **Bishop** to split it into smaller insertion milestones first. Treat these signals as a mandatory split: more than 6 files, more than 4 checklist items, or one milestone mixing UI restructuring, state/bindings, action migration, localization, and tests
   - After splitting, report the new smaller milestones to the user and stop instead of forcing execution through the oversized step

4. **Execute the candidate milestone** — Run one **Xenomorph** subagent only if the preflight passes:
   > "Execute the milestone at `.planning/milestone-N-slug.md` (use the actual filename of the candidate; both speaking names and legacy bare-number names are valid). Set its frontmatter `status` to `current` before starting. Actively update the `## To-dos` checklist during execution: mark each step `[x]` as soon as it is complete. Set `status: done` only when **both** the fachliche Done-Kriterium is met **and** no `[ ]` entries remain in the `## To-dos` list. If the milestone still turns out to be too coarse before coding begins, stop, request a split, and do not push through the whole scope in one run. Respect `.vscode/agent-session.json` for the commit decision."

5. **Check outcome** — Read the Summary section appended to the milestone file:
   - Done criterion met → confirm `done`, report success, then output the **GSD Status block** (see format below)
   - Blocker reported → surface the specific blocker to the user, run **Hicks** for that milestone only:
     > "Verify milestone `.planning/milestone-N-slug.md` (use the actual filename). The executor reported this blocker: [describe]. Diagnose and report."

6. **Offer to continue** — If more `open` milestones remain, ask the user whether to proceed to the next one

Tell the user:
_"Meilenstein abgeschlossen. Nächster Schritt: `gsd execute-phase` erneut für den nächsten offenen Meilenstein ausführen oder `gsd verify` zum interaktiven Prüfen der Done-Kriterien verwenden."_

---

## GSD-Statusblock (immer nach erfolgreichem execute-phase ausgeben)

Nach jeder erfolgreichen Meilenstein-Ausführung gib genau diesen Block aus — befüllt aus dem aktuellen Zustand in `.planning/`:

```markdown
## GSD Status

- **Aktuell:** — _(keiner)_
- **Offen:** `milestone-N+1-slug`, `milestone-N+2-slug` _(N verbleibend)_
- **Erledigt:** N Meilensteine abgeschlossen _(zuletzt: `milestone-N-slug`)_
- **Blockiert:** — _(oder blockierte Meilenstein-Namen auflisten)_
- **Nächster Schritt:** `gsd execute-phase` → `milestone-N+1-slug`
```

Regeln für den Block:
- Liste nur Meilenstein-**Namen** auf (Stem + Slug), nicht den vollständigen Inhalt
- Wenn keine offenen Meilensteine mehr übrig sind, ersetze die Offen-Zeile durch: `**Offen:** — _(Backlog abgeschlossen)_`
- Wenn keine blockierten Meilensteine existieren, zeige `—` bei **Blockiert**
- Halte den Block kompakt — keine To-do-Details, keine Diff-Ausgabe
