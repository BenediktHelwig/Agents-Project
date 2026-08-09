# Consulting-Agent

Ein Berater-Agent mit Persona „10+ Jahre Erfahrung in Agent-Orchestrierung". Er nimmt
Anforderungen durch intensives Nachfragen auf, diagnostiziert daraus die nötigen Agent-Rollen
und generiert ein fertiges Multi-Agent-System.

## Was er produziert

```
dein-projekt/
├── agents/
│   ├── orchestrator.md          # Teamlead: koordiniert, vergibt Tasks
│   └── <spezialisten>.md        # z.B. dev-agent.md, test-agent.md, review-agent.md
├── orchestration-config.json    # Architektur: Rollen, Kommunikation, Reihenfolge
└── ORCHESTRATION-DESIGN.md      # Dokumentation: Überblick, Rollen, Workflows
```

Der Consultant **orchestriert nicht selbst** — er berät und generiert. Die Ausführung
übernehmen die erzeugten Agenten.

## Starten

```bash
cd agents/consulting-agent
claude
```

Der Agent beginnt von sich aus mit der Anforderungsaufnahme. Den Zielpfad für die generierten
Dateien gibst du ihm im Gespräch an.

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
| [`CLAUDE.md`](CLAUDE.md) | Die Agent-Definition: Persona, 3-Phasen-Workflow, Templates für generierte Agenten und Konfiguration |
| [`docs/CONTEXT-SUMMARY.md`](docs/CONTEXT-SUMMARY.md) | Entstehungskontext, Kern-Anforderungen und Design-Entscheidungen |

## Design-Entscheidungen

- **grill-me läuft automatisch** — der Agent fragt nie, ob er nachfragen darf.
- **Dateibasierte Kommunikation** zwischen den generierten Agenten (JSON im Ordner
  `orchestration/`).
- **Lokal only** — keine API-Calls, kein Internet.
- **Jeder generierte Agent bekommt einen eigenen grill-me-Fokus** — der Test-Agent grillt
  Code-Quality und Coverage, der Dev-Agent Architektur und Edge-Cases, der Reviewer
  Standards und Security.

Details und Rework-Richtlinien: [`docs/CONTEXT-SUMMARY.md`](docs/CONTEXT-SUMMARY.md).
