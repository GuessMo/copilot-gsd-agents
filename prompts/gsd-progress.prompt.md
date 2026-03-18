---
agent: 'ask'
description: 'GSD: Zeigt den aktuellen Projektfortschritt — Meilenstein-Backlog gruppiert nach Aktuell/Offen/Erledigt plus Blocker. Enthält die To-do-Checkliste des aktuellen Meilensteins und To-do-Zähler für offene Meilensteine.'
tools: ['read']
---

Lies die folgenden Dateien aus `.planning/` und präsentiere eine klare Fortschrittsübersicht:

1. **All `.planning/milestone-*.md`** — sorted by numeric stem, then alphabetic suffix. For each, read the `status` frontmatter field, the milestone name, and the `## To-dos` section (if present).
2. **`.planning/STATE.md`** — show open questions and blockers

## Output format

```
## GSD Progress

- **Aktuell:** `milestone-N-slug` 🔄 — [Name]
- **Offen:** `milestone-A-slug`, `milestone-B-slug` _(N verbleibend)_
- **Erledigt:** N Meilensteine ✅ _(zuletzt: `milestone-X-slug`)_
- **Blockiert:** — _(oder blockierte Meilenstein-Namen mit 🚫 auflisten)_
- **Nächster Schritt:** [Eine klare nächste Aktion]

---

### Aktuelle To-dos
- [x] Erledigter Schritt
- [ ] Noch offener Schritt
(alle To-do-Zeilen aus `## To-dos` des aktuellen Meilensteins mit ihrem exakten `[x]`-/`[ ]`-Status)

### Offene Meilensteine
- milestone-N-slug.md: [Name] ⏳  [2/5 To-dos erledigt]
    offene Punkte: Schritt C, Schritt D, Schritt E

### Blocker (aus STATE.md)
- [offene Fragen und Blocker]
```

**To-do display rules:**
- **Aktueller Meilenstein:** Zeige jeden Eintrag aus der `## To-dos`-Liste mit seinem exakten `[x]`- oder `[ ]`-Status.
- **Offene Meilensteine:** Zeige einen Zähler (`N/M To-dos erledigt`) neben dem Namen. Wenn der Meilenstein offene Punkte hat und die Ausgabe kompakt bleibt, liste diese offenen `[ ]`-Punkte zusätzlich darunter auf.
- **Erledigte/Blockierte Meilensteine:** Keine To-do-Details nötig — eine knappe Zählsumme reicht.
- Wenn ein Meilenstein keinen Abschnitt `## To-dos` hat, lass die To-do-Zeilen stillschweigend weg.

> **Hinweis:** Legacy-Dateien (`1-1-PLAN.md`, `1-2-PLAN.md` usw.) bleiben erhalten, gehören aber nicht zum aktiven Backlog. Nimm sie nicht in die Fortschrittsausgabe auf. Meilenstein-Dateien im Legacy-Format ohne sprechenden Namen (`milestone-N.md`, `milestone-Na.md`) gehören weiterhin zum aktiven Backlog — beziehe sie sortiert nach numerischem Stem und Alpha-Suffix ein.
