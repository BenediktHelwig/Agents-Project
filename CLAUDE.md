# Repo-Konventionen: Agents

Dieses Repository ist eine Sammlung eigenständiger Claude-Code-Agenten. Diese Datei gilt für
das Repo als Ganzes — sie ist **keine** Agent-Persona. Die Persona eines Agenten steht in
`agents/<agent-name>/CLAUDE.md` und wird geladen, wenn die Session in dessen Ordner startet.

## Grundprinzipien

1. **Ein Agent = ein Ordner.** Alles, was ein Agent braucht, liegt unter
   `agents/<agent-name>/`. Kein Agent liest Dateien aus dem Ordner eines anderen Agenten.
2. **Herauspickbarkeit.** Jeder Agent muss sich durch Kopieren seines Ordners (plus der
   dokumentierten Skills) in ein fremdes Repo übernehmen lassen. Wenn eine Änderung diese
   Eigenschaft bricht, ist sie falsch.
3. **Nur Markdown und JSON.** Keine Build-Schritte, keine Laufzeitabhängigkeiten, keine
   Installation.
4. **Lokal only.** Agenten arbeiten ohne Internet-Calls auf dem System des Nutzers.
5. **Geteilte Skills liegen im Root.** Alles unter `.claude/skills/` steht allen Agenten
   dieses Repos zur Verfügung. Ein Agent, der einen Skill braucht, nennt ihn in seinem
   README unter *Abhängigkeiten* — sonst geht er beim Herauspicken verloren.
   **Ausnahme:** Vom `consulting-agent` erzeugte Pakete bringen ihre Skills als Kopie in
   `agents/<paket-name>/.claude/skills/` mit, damit ein einziger Kopiervorgang zum Picken reicht.

## Anforderungen an einen neuen Agenten

Ein Ordner unter `agents/` ist erst vollständig, wenn er enthält:

| Datei | Inhalt |
| --- | --- |
| `CLAUDE.md` | Persona, Verantwortung, Inputs, Outputs, Kommunikation, spezialisierter grill-me-Fokus |
| `README.md` | Wozu der Agent da ist, wie man ihn startet, welche Skills er braucht |
| `docs/` | Optional: Entstehungskontext und Design-Entscheidungen |

Danach: Eintrag in der Agenten-Tabelle in [`README.md`](README.md) ergänzen.

## Umgang mit generierten Agenten

Der [`consulting-agent`](agents/consulting-agent/) erzeugt fertige Pakete — einen einzelnen
Agenten oder ein ganzes Orchester. Ein solches Paket liegt als **ein** Ordner unter `agents/`
und hat dieselbe Form wie ein handgeschriebener Agent, ergänzt um seine Rollen:

```
agents/<paket-name>/
├── CLAUDE.md
├── README.md
├── .claude/
│   ├── agents/            # die Rollen als Subagents, mit model: im Frontmatter
│   └── skills/            # mitgelieferte Skills, damit ein Kopiervorgang genügt
└── docs/
```

Die Rollen kommunizieren über Delegation (Agent-Tool), nicht über Dateien.

**Hinweis:** Auch handgeschriebene Agenten dürfen ein `.claude/`-Verzeichnis mit eigenen
Subagents und einer `settings.json` haben — Herauspickbarkeit bleibt gewahrt, weil alles im
Agent-Ordner liegt und beim Kopieren mitgenommen wird.

## Arbeitsweise

- **Sprache:** Deutsch in Dokumentation und Agent-Definitionen. Technische Begriffe und
  Bezeichner bleiben im Original.
- **Test-Driven Development**, wenn tatsächlich Code entsteht (Rot → Grün).
- **Commits** in deutscher Sprache, imperativ, mit Präfix des betroffenen Agenten,
  z.B. `consulting-agent: Grill-me-Phase um Eskalationspfade erweitern`.
