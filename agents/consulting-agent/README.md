# Consulting Agent

A consultant agent with the persona "10+ years of experience in agent orchestration". It gathers
requirements through intensive questioning, diagnoses the necessary agent roles from that,
and generates a finished multi-agent system.

## What It Produces

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

The package is self-contained: a single copy operation is enough to adopt it into another repo.

The consultant **does not orchestrate itself** — it advises and generates. The generated
agents take over execution.

## Starting

```bash
cd agents/consulting-agent
claude
```

The agent begins requirements gathering on its own. Thanks to
`.claude/settings.json`, the session automatically starts on Opus in Plan mode. Phases 1–2
stay there; at the end of Phase 2 the agent asks you to switch to **auto mode** with Shift+Tab
and waits until you confirm. From there the run goes through in one piece — a branch before the
first write, the build guide, then the generated files — and you review the whole package as a
single diff in the pull request it opens in Phase 5, which you merge yourself.

If `gh` is missing or the repository is not on GitHub, branch and commit stay local and the
message waits at `.agents/pull-request.md` for you to paste.

The model switch to Haiku (Phase 4) and back to Opus (Phase 5) runs through the
`package-writer` subagent — no manual switching needed. By default, it places the package under
`agents/<package-name>/` of this repo; you name a different target path during the conversation.

## Dependencies

| Skill | Origin | Purpose |
| --- | --- | --- |
| `grill-me` | `.claude/skills/grill-me/` (repo root) | Phase 1 of the consulting workflow: relentlessly questioning the requirements |

When adopting into another repo: copy this folder **and** copy `grill-me` along to
`.claude/skills/grill-me/` in the target repo. Without the skill, the agent is missing its
core mechanic.

## Structure

| File | Content |
| --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | The agent definition: persona, 5-phase workflow, templates for generated agents and configuration |
| [`.claude/settings.json`](.claude/settings.json) | Startup configuration: `model: opus`, `permissions.defaultMode: plan` |
| [`.claude/agents/package-writer.md`](.claude/agents/package-writer.md) | Subagent for Phase 4: file creation under Haiku |
| [`docs/CONTEXT-SUMMARY.md`](docs/CONTEXT-SUMMARY.md) | Origin context, core requirements, and design decisions |

## Design Decisions

- **grill-me runs automatically** — the agent never asks whether it may ask questions.
- **Opus plans, Haiku builds, Opus signs off** — the model switch happens automatically via
  `.claude/settings.json` (startup check) and via the `package-writer` subagent (Phase 4). The
  consultant never requires a model switch; it only handles permission modes (Plan mode for
  Phases 1–2, auto mode from Phase 3 on).
- **The pull request is the review point** — not the individual write confirmation. Confirming
  each file one at a time produces click-through, not review; a single diff on a branch is the
  better thing to read. Opening the pull request is conditional on `gh` and a GitHub remote, so
  the agent stays usable elsewhere.
- **Generated roles are real subagents** under `.claude/agents/` with `model:` in the frontmatter.
  The orchestrator delegates via the Agent tool; there is no file-based JSON protocol and
  no `orchestration-config.json`.
- **One agent = one fixed model** — the model is determined per agent in conversation, each with
  the consultant's recommendation.
- **Only the orchestrator grills** — as subagents, specialists have no conversation partner and
  instead get a self-check checklist.
- **The package brings `grill-me` along** — picking means copying a folder.
- **Local only** — no API calls, no internet.
