# Consulting-Agent Projekt: Kontext & Anforderungen

**Erstellt:** 2026-05-24  
**Status:** ✅ consultant.md fertiggestellt  
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
- Jeder generierte Agent hat:
  - Spezialisierte Persona
  - Klare Verantwortung (Was TUT er? Was NICHT?)
  - Input/Output Format
  - Kommunikations-Pattern
  - Eigener spezialisierter grill-me Skill (z.B. Test-Agent grillt Code-Quality)

### 4. Technische Architektur
- **Kommunikation:** Dateibasiert (JSON, lokal)
- **Kein Internet:** Alles läuft lokal auf Benutzer-System
- **Speicherort:** User gibt Pfad an → Consultant speichert dort
- **Ausführung:** `claude run consultant.md`
- **Code-Umgang:** Test-Driven Development (Rot → Grün)

### 5. Output-Format
Der Consultant generiert:

```
user-project/
├── agents/
│   ├── orchestrator.md          # Teamlead
│   ├── [specialist-agents].md   # Dev, Test, Review, Doc, etc.
│   └── ...
├── orchestration-config.json    # Architektur & Flow
└── ORCHESTRATION-DESIGN.md      # Dokumentation
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

1. **Grill-me ist AUTOMATISCH** – Der Consultant fragt nie "soll ich grill-me nutzen?"
2. **Keine Sub-Orchestrierung** – Der Consultant orchestriert nicht selbst, nur die generierten Agents
3. **Dateibasiert** – Alle Agent-Kommunikation über Dateien (JSON in `orchestration/` Folder)
4. **Lokal only** – Keine APIs, kein Internet, alles auf Benutzer-Rechner
5. **Spezialisierte grill-me** – Jeder Agent hat seinen eigenen grill-me Fokus
6. **TDD für Code** – Test-Driven Development (Rot → Grün) wenn Code involved

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

---

## 📞 Quick-Links

- **Consultant-Agent:** `../CLAUDE.md` (ursprünglich als `consultant.md` entworfen)
- **Nächster Schritt:** `cd agents/consulting-agent && claude`
- **Dokumentation:** Diese Datei sowie `../README.md`

---

**Status:** Konsultant ist ready zum Einsatz. Jederzeit kann neu gegrillet werden bei Änderungen.
