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

## Anforderungen an einen neuen Agenten

Ein Ordner unter `agents/` ist erst vollständig, wenn er enthält:

| Datei | Inhalt |
| --- | --- |
| `CLAUDE.md` | Persona, Verantwortung, Inputs, Outputs, Kommunikation, spezialisierter grill-me-Fokus |
| `README.md` | Wozu der Agent da ist, wie man ihn startet, welche Skills er braucht |
| `docs/` | Optional: Entstehungskontext und Design-Entscheidungen |

Danach: Eintrag in der Agenten-Tabelle in [`README.md`](README.md) ergänzen.

## Umgang mit generierten Agenten

Der [`consulting-agent`](agents/consulting-agent/) erzeugt komplette Agenten-Systeme
(`agents/`, `orchestration-config.json`, `ORCHESTRATION-DESIGN.md`). Wird so ein System hier
abgelegt, kommt es als **ein** Ordner unter `agents/` — mit seinem Orchestrator und seinen
Spezialisten darin, nicht flach neben die anderen Agenten verteilt.

## Arbeitsweise

- **Sprache:** Deutsch in Dokumentation und Agent-Definitionen. Technische Begriffe und
  Bezeichner bleiben im Original.
- **Test-Driven Development**, wenn tatsächlich Code entsteht (Rot → Grün).
- **Commits** in deutscher Sprache, imperativ, mit Präfix des betroffenen Agenten,
  z.B. `consulting-agent: Grill-me-Phase um Eskalationspfade erweitern`.
