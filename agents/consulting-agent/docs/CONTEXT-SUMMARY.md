# Consulting Agent Project: Context & Requirements

**Created:** 2026-05-24
**Status:** ✅ Package format and model split established
**Next Phase:** Implementation & Testing

---

## 📋 Project Overview

**Goal:** Create a highly specialized **Agent Orchestration Consultant** that:
- Automatically applies the **grill-me skill** (intensive questioning)
- Designs specialized agent systems for any domain
- Generates finished subagent definitions under `.claude/agents/` plus the package around them
- Helps the user build complex multi-agent systems

---

## 🎯 Core Requirements

### 1. The Consultant Agent Itself
- **Persona:** 10+ years of experience in agent orchestration
- **Behavior:** Uses grill-me automatically (NO explicit request needed)
- **Tone:** Professional consultant, intensive, in-depth
- **Output:** A complete, pickable package — agent definitions, documentation, bundled skill

### 2. Grill-me Integration
- The consultant asks very intensively about:
  - Domain & goal of the system
  - Complexity & quality standards
  - Agent architecture & hierarchy
  - Iterations & error handling
  - Validation of requirements
- Questions, validates, asks follow-up questions
- **No superficial questions** – real consulting

### 3. Agent Composition
- The consultant automatically **diagnoses** which agents are needed
- **Not orchestrating itself** – only advising
- Every generated agent is a subagent under `.claude/agents/` with:
  - Specialized persona
  - `model:` in the frontmatter (`opus` for judgment roles, `haiku` for execution roles)
  - Clear responsibility (What does it DO? What NOT?)
  - Input/output description and `tools:`
  - Self-check checklist instead of grill-me (only the orchestrator grills)

### 4. Technical Architecture
- **Communication:** Delegation via the Agent tool (assignment as prompt, report back)
- **No internet:** Everything runs locally on the user's system
- **Storage location:** Default `agents/<package-name>/` in this repo; different path on request
- **Execution:** `cd agents/consulting-agent && claude`
- **Model split:** Opus for Phases 1–3 and 5, Haiku for Phase 4. The switch happens
  automatically — `settings.json` sets Opus at startup, and Phase 4 is delegated to the
  `package-writer` subagent, which carries `model: haiku` in its frontmatter. The consultant
  never asks the client to run `/model`.
- **Permission modes:** Plan mode covers Phases 0–2. Phase 3 is the first write, so at the end
  of Phase 2 the consultant requires the client to switch to Manual mode via Shift+Tab and
  waits for confirmation — it never switches the mode itself. After writing the build guide it
  checks back whether that write actually had to be confirmed.
- **Handling code:** Test-Driven Development (Red → Green)

### 5. Output Format
The consultant generates:

```
agents/<package-name>/
├── CLAUDE.md                     # What the system is, how to start it
├── README.md                     # Purpose, picking, dependencies
├── .claude/
│   ├── agents/
│   │   ├── orchestrator.md       # model: opus  (omitted for a single agent)
│   │   └── <specialist>.md       # model: haiku
│   └── skills/
│       └── grill-me/SKILL.md     # bundled
└── docs/
    ├── ORCHESTRATION-DESIGN.md   # only for an orchestra
    └── BUILD-SPEC.md             # the completed build guide
```

**PLUS:** Comprehensive documentation explaining:
- Which agent roles exist
- What each agent does
- How they communicate
- Which flow (sequential? feedback loops? parallel?)

---

## 🔧 Implemented Solution

✅ **`../CLAUDE.md`** — the agent definition, containing:
- Greeting & consultant persona
- Startup check (Phase 0) plus the five phases: grill-me → composition → build spec →
  file creation → sign-off
- Intensive grill-me questions (7 categories, including model choice per role)
- Templates for every file of the generated package
- Which model and which permission mode each phase belongs to

✅ **`../.claude/agents/package-writer.md`** — the Haiku subagent that turns `BUILD-SPEC.md`
into the actual files in Phase 4

✅ **`../.claude/settings.json`** — starts the session on Opus in Plan mode

---

## 📌 Important Design Decisions

1. **Grill-me is AUTOMATIC** – the consultant never asks "should I use grill-me?"
2. **No sub-orchestration** – the consultant does not orchestrate itself, only the generated agents do
3. **Delegation instead of file protocol** – no `orchestration/` directory, no `orchestration-config.json`
4. **Self-contained packages** – `grill-me` is copied into the package, picking = one copy operation
5. **One agent = one model** – determined per agent in conversation, with the consultant's recommendation
6. **Opus plans, Haiku builds, Opus signs off** – handoff via `docs/BUILD-SPEC.md`
7. **Only the orchestrator grills** – specialists check themselves
8. **Local only** – no APIs, no internet
9. **TDD for code** – Red → Green

---

## 🔄 Rework Guidelines

**IMPORTANT:** For every rework or improvement:

1. **GRILL WITH grill-me** – question the new requirements intensively
2. **Ask follow-up questions** – What changed? Why?
3. **Check impact** – How does the change affect the rest of the architecture?
4. **Validate** – Are we really done? Is anything missing?

**Example rework workflow:**
```
1. User: "I want to change X"
2. Claude (grill-me): "Why are we changing this? What's the new problem?"
3. Claude: Follow-up questions on impact
4. Claude: Adjustment of ../CLAUDE.md
5. Claude: Explanation of the changes here in CONTEXT-SUMMARY.md
```

---

## ✅ Open Items

- [ ] Test the consultant agent (run through a first session)
- [ ] Create templates for specialized agents (optional)
- [ ] Work through example projects (optional)
- [ ] Document real-world usage
- [ ] Generate the first package and actually run through the pick process into a foreign repo

---

## 📞 Quick Links

- **Consultant Agent:** `../CLAUDE.md` (originally designed as `consultant.md`)
- **Next Step:** `cd agents/consulting-agent && claude`
- **Documentation:** This file and `../README.md`

---

**Status:** Consultant is ready for use. Can be re-grilled at any time for changes.
