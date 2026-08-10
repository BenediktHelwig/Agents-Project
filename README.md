# Agents

Eine Sammlung spezialisierter Claude-Code-Agenten. Jeder Agent liegt eigenständig unter
`agents/<agent-name>/` und lässt sich einzeln in ein anderes Repository übernehmen.

Ausgangspunkt ist der **Consulting-Agent**: ein Berater, der per Interview-Verfahren
Anforderungen aufnimmt und daraus fertige Multi-Agent-Systeme entwirft und generiert.
Die von ihm erzeugten Agenten wandern zurück in dieses Repo — als jeweils eigener Ordner
unter `agents/`.

---

## Struktur

```
.
├── .claude/
│   └── skills/            # Skills, die für alle Agenten in diesem Repo verfügbar sind
│       └── grill-me/
├── agents/                # Ein Ordner pro Agent – jeder für sich vollständig
│   └── consulting-agent/
│       ├── CLAUDE.md      # Die Agent-Definition (Persona, Workflow, Output-Format)
│       ├── README.md      # Zweck, Einsatz, Abhängigkeiten
│       └── docs/          # Entstehungskontext, Design-Entscheidungen
├── CLAUDE.md              # Konventionen dieses Repos (gilt für alle Agenten)
├── .gitignore
└── .gitattributes
```

## Vorhandene Agenten

| Agent | Zweck | Abhängigkeiten |
| --- | --- | --- |
| [`consulting-agent`](agents/consulting-agent/) | Designt und generiert Multi-Agent-Systeme für beliebige Domänen | Skill `grill-me` |
| [`software-architect`](agents/software-architect/) | Dokumentiert Bestandsarchitekturen, entwirft neue, erzeugt draw.io-UML und ADRs | Skill `grill-me` (mitgeliefert) |

## Einen Agenten benutzen

Claude Code lädt die `CLAUDE.md` aus dem Verzeichnis, in dem es gestartet wird. Um mit einem
Agenten zu arbeiten, startest du die Session in dessen Ordner:

```bash
cd agents/consulting-agent
claude
```

Die Skills aus `.claude/skills/` des Repo-Roots stehen dabei zur Verfügung, weil Claude Code
die Verzeichnisse oberhalb des Arbeitsverzeichnisses mit einbezieht.

> **Hinweis:** Ein `claude`-Start im Repo-Root lädt bewusst *nicht* den Consulting-Agent,
> sondern die Repo-Konventionen aus [`CLAUDE.md`](CLAUDE.md). So bleibt das Repo neutral,
> wenn mehrere Agenten nebeneinander existieren.

## Einen Agenten in ein anderes Repo übernehmen

Jeder Agent ist so geschnitten, dass er sich mit einem Kopiervorgang herauslösen lässt:

1. Ordner `agents/<agent-name>/` in das Zielrepo kopieren.
2. Die im README des Agenten unter *Abhängigkeiten* genannten Skills nach
   `.claude/skills/<skill-name>/` im Zielrepo kopieren.
3. Fertig — im Zielrepo `cd` in den Agent-Ordner und `claude` starten.

Es gibt keine repo-weiten Build-Schritte, keine Pfad-Magie und keine geteilten Zustände
zwischen den Agenten. Alles ist Markdown und JSON.

## Einen neuen Agenten hinzufügen

Konventionen und Mindestanforderungen stehen in [`CLAUDE.md`](CLAUDE.md).
