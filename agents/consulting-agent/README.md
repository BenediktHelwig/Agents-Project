# Consulting-Agent

Ein Berater-Agent mit Persona „10+ Jahre Erfahrung in Agent-Orchestrierung". Er nimmt
Anforderungen durch intensives Nachfragen auf, diagnostiziert daraus die nötigen Agent-Rollen
und generiert ein fertiges Multi-Agent-System.

## Was er produziert

```
agents/<paket-name>/
├── CLAUDE.md                     # Was das System ist, wie man es startet
├── README.md                     # Zweck, Picken, Abhängigkeiten
├── .claude/
│   ├── agents/
│   │   ├── orchestrator.md       # model: opus  (entfällt beim Einzelagenten)
│   │   └── <spezialist>.md       # model: haiku
│   └── skills/
│       └── grill-me/SKILL.md     # mitgeliefert
└── docs/
    ├── ORCHESTRATION-DESIGN.md   # nur beim Orchester
    └── BUILD-SPEC.md             # die abgearbeitete Bauanleitung
```

Das Paket ist selbstenthalten: Ein Kopiervorgang genügt, um es in ein anderes Repo zu übernehmen.

Der Consultant **orchestriert nicht selbst** — er berät und generiert. Die Ausführung übernehmen
die erzeugten Agenten.

## Starten

```bash
cd agents/consulting-agent
claude
```

Der Agent beginnt von sich aus mit der Anforderungsaufnahme. Die Session startet dank
`.claude/settings.json` automatisch auf Opus im Plan-Mode. Während des Laufs fordert der Agent
dich auf, vor Phase 4 in den Normal-Mode zu wechseln (Shift+Tab). Der Modellwechsel auf Haiku
(Phase 4) und zurück auf Opus (Phase 5) läuft über den `package-writer`-Subagent — keine
manuellen Wechsel nötig. Standardmäßig legt er das Paket unter `agents/<paket-name>/` dieses
Repos ab; einen abweichenden Zielpfad nennst du ihm im Gespräch.

## Abhängigkeiten

| Skill | Herkunft | Wozu |
| --- | --- | --- |
| `grill-me` | `.claude/skills/grill-me/` (Repo-Root) | Phase 1 des Beratungs-Workflows: die Anforderungen relentless durchfragen |

Beim Übernehmen in ein anderes Repo: diesen Ordner kopieren **und** `grill-me` nach
`.claude/skills/grill-me/` im Zielrepo mitkopieren. Ohne den Skill fehlt dem Agent seine
Kernmechanik.

## Aufbau

| Datei | Inhalt |
| --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | Die Agent-Definition: Persona, 5-Phasen-Workflow, Templates für generierte Agenten und Konfiguration |
| [`.claude/settings.json`](.claude/settings.json) | Startkonfiguration: `model: opus`, `permissions.defaultMode: plan` |
| [`.claude/agents/package-writer.md`](.claude/agents/package-writer.md) | Subagent für Phase 4: Dateien-Anlage unter Haiku |
| [`docs/CONTEXT-SUMMARY.md`](docs/CONTEXT-SUMMARY.md) | Entstehungskontext, Kern-Anforderungen und Design-Entscheidungen |

## Design-Entscheidungen

- **grill-me läuft automatisch** — der Agent fragt nie, ob er nachfragen darf.
- **Opus plant, Haiku baut, Opus nimmt ab** — der Modellwechsel geschieht automatisch über
  `.claude/settings.json` (Startcheck) und über den `package-writer`-Subagent (Phase 4). Der
  Consultant fordert niemals einen Modellwechsel an; er fordert nur Permission-Modes an
  (Plan-Mode, Normal-Mode).
- **Generierte Rollen sind echte Subagents** unter `.claude/agents/` mit `model:` im Frontmatter.
  Der Orchestrator delegiert über das Agent-Tool; es gibt kein dateibasiertes JSON-Protokoll und
  keine `orchestration-config.json`.
- **Ein Agent = ein festes Modell** — das Modell wird pro Agent im Gespräch bestimmt, jeweils mit
  Empfehlung des Consultants.
- **Nur der Orchestrator grillt** — Spezialisten haben als Subagents keinen Gesprächspartner und
  bekommen stattdessen eine Selbstprüf-Checkliste.
- **Das Paket bringt `grill-me` mit** — herauspicken heißt: einen Ordner kopieren.
- **Lokal only** — keine API-Calls, kein Internet.
