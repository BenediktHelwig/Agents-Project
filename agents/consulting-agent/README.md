# Consulting Agent

A consultant agent with the persona "10+ years of experience in agent orchestration". It gathers
requirements through intensive questioning, designs the leanest agent composition that solves
the problem, and writes a finished, pickable package. It can also refactor an existing package
against the same rules.

## What It Produces

```
agents/<package-name>/
├── CLAUDE.md                     # The main agent (orchestrator), flow, mode gate, git
├── README.md                     # Purpose, starting, picking, dependencies
├── .claude/
│   ├── settings.json             # starts on Opus in Plan mode
│   ├── agents/                   # subagents only, e.g. implementer (sonnet), reviewer (opus)
│   └── skills/
│       └── grill-me/SKILL.md     # bundled
└── docs/
    └── ORCHESTRATION-DESIGN.md   # only if there is at least one subagent
```

The package is self-contained: a single copy operation is enough to adopt it into another repo.
The consultant **does not orchestrate itself** — it advises and generates.

## Starting

```bash
cd agents/consulting-agent
claude
```

Thanks to `.claude/settings.json`, the session starts on Opus in Plan mode. To rework an existing
package instead of creating one, name it at the start (e.g. "refactor `feature-crew`").

## How a Run Proceeds

| Phase | Mode | What happens |
| --- | --- | --- |
| 0 Startup check | Plan | Model and mode are verified |
| 1 Grill-me | Plan | Requirements, splitting rule per role, model per role, package name |
| 2 Composition | Plan | Roles, flow — then you switch to **auto mode** with Shift+Tab |
| 3 Build | auto | Branch, all files written in one go |
| 4 Sign-off | auto | Substantive check, commit, pull request |

You review the whole package as a single diff in the pull request and merge it yourself. If `gh`
is missing or the repository is not on GitHub, branch and commit stay local and the message waits
at `.agents/pull-request.md` for you to paste.

## Dependencies

| Skill | Origin | Purpose |
| --- | --- | --- |
| `grill-me` | `.claude/skills/grill-me/` (repo root) | Phase 1: relentlessly questioning the requirements; copied into every generated package |

When adopting into another repo: copy this folder **and** copy `grill-me` along to
`.claude/skills/grill-me/` in the target repo.

## Structure

| File | Content | Loaded |
| --- | --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | Persona, ground rules, splitting rule, Phases 0–4 | every turn |
| [`.claude/settings.json`](.claude/settings.json) | `model: opus`, `permissions.defaultMode: plan` | at startup |
| [`docs/PACKAGE-TEMPLATES.md`](docs/PACKAGE-TEMPLATES.md) | Target structure, file templates, sign-off checklist | Phase 3 only |
| [`docs/REFACTOR-MODE.md`](docs/REFACTOR-MODE.md) | Diagnosis and rework of an existing package | refactor runs only |
| [`docs/CONTEXT-SUMMARY.md`](docs/CONTEXT-SUMMARY.md) | Origin context and design decisions | never by the agent |

## Design Decisions

- **grill-me runs automatically** — the agent never asks whether it may ask questions.
- **Opus writes the package directly.** The former flow (Opus writes a full `BUILD-SPEC.md`, a
  Haiku subagent copies it into files) saved nothing: Opus had already produced every word, and
  the spec doubled the pull request. Files are written in parallel in a single response.
- **Binding splitting rule** — a role becomes a subagent only for a fresh view, real
  parallelism, a large block a cheaper model does reliably, or restricted tools. Split by
  context, not by job title.
- **Variant B as the default** for implementation work: main agent (opus) plans and signs off,
  one `implementer` (sonnet) does test → code → docs in one context, a read-only `reviewer`
  (opus) checks the diff.
- **Three models:** `opus` for judgment, `sonnet` for demanding implementation, `haiku` for
  purely mechanical work.
- **The orchestrator is the main session**, never a subagent — subagents cannot talk to the user
  or call other subagents.
- **Templates are loaded on demand** — they only cost tokens in the phase that needs them.
- **The pull request is the review point**, not the individual write confirmation.
- **Local only** — no API calls, no internet.
