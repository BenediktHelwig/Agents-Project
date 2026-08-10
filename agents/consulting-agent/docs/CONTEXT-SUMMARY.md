# Consulting-Agent Projekt: Kontext & Anforderungen

**Erstellt:** 2026-05-24  
**Status:** ✅ Paket-Format und Modell-Split festgelegt  
**Nächste Phase:** Implementierung & Testing

---

## 📋 Projektübersicht

**Ziel:** Einen hochspezialisierten **Agent Orchestration Consultant** erstellen, der:
- Automatisch den **grill-me Skill** einsetzt (intensives Hinterfragen)
- Spezialisierte Agent-Systeme für beliebige Domänen designt
- Fertige `agent.md` Dateien und Konfigurationen generiert
- Dem Nutzer hilft, komplexe Multi-Agent-Systeme aufzubauen

---

## 🎯 Kern-Anforderungen

### 1. Der Consultant-Agent selbst
- **Persona:** 10+ Jahre Erfahrung in Agent-Orchestrierung
- **Verhalten:** Nutzt automatisch grill-me (KEINE explizite Anfrage nötig)
- **Tonalität:** Professioneller Berater, intensiv, tiefgreifend
- **Output:** Generiert fertige Agent-Dateien + Konfiguration + Dokumentation

### 2. Grill-me Integration
- Der Consultant fragt sehr intensiv nach:
  - Domäne & Ziel des Systems
  - Komplexität & Quality-Standards
  - Agenten-Architektur & Hierarchie
  - Iterationen & Fehlerbehandlung
  - Validierung der Anforderungen
- Hinterfragt, validiert, stellt Follow-up-Fragen
- **Keine oberflächlichen Fragen** – echte Beratung

### 3. Agent-Komposition
- Der Consultant **diagnostiziert** automatisch, welche Agenten nötig sind
- **Nicht orchestrierend selbst** – nur beratend
- Jeder generierte Agent ist ein Subagent unter `.claude/agents/` mit:
  - Spezialisierter Persona
  - `model:` im Frontmatter (`opus` für urteilende, `haiku` für ausführende Rollen)
  - Klarer Verantwortung (Was TUT er? Was NICHT?)
  - Input/Output-Beschreibung und `tools:`
  - Selbstprüf-Checkliste statt grill-me (nur der Orchestrator grillt)

### 4. Technische Architektur
- **Kommunikation:** Delegation über das Agent-Tool (Auftrag als Prompt, Report zurück)
- **Kein Internet:** Alles läuft lokal auf Benutzer-System
- **Speicherort:** Standard `agents/<paket-name>/` in diesem Repo; abweichender Pfad auf Ansage
- **Ausführung:** `cd agents/consulting-agent && claude`
- **Modell-Split:** Opus für Phase 1–3 und 5, Haiku für Phase 4; Wechsel per `/model`, vom
  Consultant eingefordert
- **Code-Umgang:** Test-Driven Development (Rot → Grün)

### 5. Output-Format
Der Consultant generiert:

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

**PLUS:** Eine umfassende Dokumentation, die erklärt:
- Welche Agenten-Rollen es gibt
- Was jeder Agent tut
- Wie sie kommunizieren
- Welcher Flow (sequenziell? Feedback-Loops? Parallel?)

---

## 🔧 Implementierte Lösung

✅ **consultant.md** fertiggestellt mit:
- Begrüßung & Berater-Persona
- 3-phasiger Workflow (Aufnahme → Diagnosis → Generierung)
- Intensive grill-me Fragen (6 kritische Kategorien)
- Templates für generierte Agents
- Struktur für `orchestration-config.json`
- Dokumentations-Vorlage
- Hinweise auf spezialisierte grill-me Skills pro Agent

---

## 📌 Wichtige Design-Entscheidungen

1. **Grill-me ist AUTOMATISCH** – der Consultant fragt nie "soll ich grill-me nutzen?"
2. **Keine Sub-Orchestrierung** – der Consultant orchestriert nicht selbst, nur die generierten Agenten
3. **Delegation statt Dateiprotokoll** – kein `orchestration/`-Verzeichnis, keine `orchestration-config.json`
4. **Selbstenthaltene Pakete** – `grill-me` wird ins Paket kopiert, Picken = ein Kopiervorgang
5. **Ein Agent = ein Modell** – pro Agent im Gespräch bestimmt, mit Empfehlung des Consultants
6. **Opus plant, Haiku baut, Opus nimmt ab** – Übergabe über `docs/BUILD-SPEC.md`
7. **Nur der Orchestrator grillt** – Spezialisten prüfen sich selbst
8. **Lokal only** – keine APIs, kein Internet
9. **TDD für Code** – Rot → Grün

---

## 🔄 Rework-Richtlinien

**WICHTIG:** Bei jedem Rework oder Verbesserung:

1. **GRILLE MIT grill-me** – Die neuen Anforderungen intensiv durchfragen
2. **Follow-up-Fragen stellen** – Was hat sich geändert? Warum?
3. **Auswirkungen prüfen** – Wie beeinflusst die Änderung die restliche Architektur?
4. **Validieren** – Sind wir wirklich fertig? Fehlt noch was?

**Beispiel-Rework-Workflow:**
```
1. User: "Ich möchte X ändern"
2. Claude (grill-me): "Warum ändern wir das? Was ist das neue Problem?"
3. Claude: Follow-up-Fragen zur Auswirkung
4. Claude: Anpassung von consultant.md
5. Claude: Erklärung der Änderungen hier in CONTEXT-SUMMARY.md
```

---

## ✅ Offene Punkte

- [ ] Consultant-Agent testen (erste Session durchlaufen)
- [ ] Templates für spezialisierte Agents erstellen (optional)
- [ ] Beispiel-Projekte durcharbeiten (optional)
- [ ] Real-World-Einsatz dokumentieren
- [ ] Erstes Paket erzeugen und den Pick-Vorgang in ein fremdes Repo real durchspielen

---

## 📞 Quick-Links

- **Consultant-Agent:** `../CLAUDE.md` (ursprünglich als `consultant.md` entworfen)
- **Nächster Schritt:** `cd agents/consulting-agent && claude`
- **Dokumentation:** Diese Datei sowie `../README.md`

---

**Status:** Konsultant ist ready zum Einsatz. Jederzeit kann neu gegrillet werden bei Änderungen.
