# Package Templates

Read in Phase 3 (and in refactor mode, Phase 3). Fill every `<…>` with real content — the
written package contains no placeholders.

## Target Structure

```
agents/<package-name>/
├── CLAUDE.md                     # The main agent (orchestrator): persona, flow, gates
├── README.md                     # Purpose, starting, picking, dependencies
├── .claude/
│   ├── settings.json             # model: opus, defaultMode: plan
│   ├── agents/                   # subagents only — never the orchestrator
│   │   ├── implementer.md        # e.g. model: sonnet
│   │   └── reviewer.md           # e.g. model: opus, read-only
│   └── skills/
│       └── grill-me/SKILL.md     # cp from the repo root
└── docs/
    └── ORCHESTRATION-DESIGN.md   # only if there is at least one subagent
```

A single agent has no `.claude/agents/` and no `ORCHESTRATION-DESIGN.md`. The orchestrator always
lives in the package `CLAUDE.md`: subagents can neither talk to the user nor call subagents.

## `.claude/settings.json`

```json
{
  "model": "opus",
  "permissions": {
    "defaultMode": "plan"
  }
}
```

## `.claude/agents/<name>.md`

```markdown
---
name: <agent-name>
description: <One sentence that makes clear when the main agent calls this subagent>
model: <opus | sonnet | haiku>
tools: <comma-separated, e.g. Read, Write, Edit, Glob, Grep, Bash>
---

# <Agent Name>

## Persona
<Experience, specialization, strength>

## Responsibility
<What it handles — and explicitly what it does NOT handle>

## Inputs
<What it receives in the delegation prompt>

## Outputs
<What it produces in the project and what its report contains. Reports stay short: files
touched, result, open issues — never repeat file contents.>

## Self-Check Before Submitting
- [ ] <domain-specific check question>
- [ ] <domain-specific check question>
- [ ] Nothing changed outside the assignment
```

A **reviewer** gets `tools: Read, Glob, Grep, Bash` and the line *"Bash only for `git diff`,
`git log` and running tests — never to write a file."* It receives the diff range, not the
conversation.

## Package `CLAUDE.md`

```markdown
# <System Name>

<Persona of the main agent: experience, specialization, stance.>

## What this system is
<One paragraph: which problem it solves.>

## Roles

| Agent | Model | Task |
| --- | --- | --- |
| main session (you) | opus | clarifies, plans, delegates, signs off |
| `<subagent>` | <model> | <…> |

## What you never do
<Boundaries of the main agent, e.g. which work always goes to a subagent.>

## Requirements Clarification
Use the grill-me skill before anything else. One question at a time, each with your
recommendation. Clarify at least: scope, boundaries, what is explicitly out of scope.

## Flow
<Order, feedback loops, escalation rule — three to eight sentences or a numbered list.>

## Delegation
Call subagents via the Agent tool. The prompt carries the assignment, the relevant paths and the
acceptance criteria — enough that the subagent need not re-derive what you already know.

## Mode gate — once per assignment

Clarification and planning run in Plan mode. Before the first write, say **verbatim**:

> The plan is settled. Please press **Shift+Tab** to switch to **auto mode** — from here on files
> are written without stopping at each one, and you review the result as a whole in the pull
> request at the end. Reply "go" when you are there.

Then wait. Never ask to switch back — review only reads, and a defect must be re-delegatable
immediately.

## Git and the pull request

1. **A branch before the first change.** Never work on a main branch.
2. **A commit after each phase**, so the sequence of work stays legible in the history.
3. **At the end, open a pull request** — check first that `gh auth status` succeeds and that
   `git remote get-url origin` names a GitHub host. Both true: push the branch and run
   `gh pr create` with `--body-file`. Either false: leave branch and commits local and write the
   message to `.agents/pull-request.md`. A missing tool never fails a run.
4. **Never push to a main branch, never force-push**, and never merge — the merge is the user's.
5. **Review remarks come back as an additional commit** on the same branch.
```

## Package `README.md`

```markdown
# <System Name>

<Purpose in two sentences.>

## Included Agents

| Agent | Model | Task |
| --- | --- | --- |
| main session | opus | … |

## Starting

```bash
cd agents/<package-name>
claude
```

## How a Run Proceeds

The session starts on Opus in Plan mode and stays there while the assignment is clarified —
nothing is written yet. Once the plan is settled you are asked once to switch to **auto mode**;
after that the work runs through in one go: a branch before the first change, a commit per phase,
and a pull request at the end that you review as a whole and merge yourself.

If `gh` is missing or the project is not on GitHub, branch and commits stay local and the message
waits at `.agents/pull-request.md` for you to paste.

## Adopting into Another Repo

Copy this folder — done. The required `grill-me` skill lives in
`.claude/skills/grill-me/` and is already included.

## Dependencies

| Skill | Origin | Purpose |
| --- | --- | --- |
| `grill-me` | bundled under `.claude/skills/grill-me/` | requirements clarification by the main agent |
```

## `docs/ORCHESTRATION-DESIGN.md`

```markdown
# Orchestration Design: <System Name>

## Problem
<What the client described.>

## Role Split

| Role | Model | Splitting criterion | Rationale |
| --- | --- | --- | --- |
| main session | opus | — | clarifies, plans, signs off |
| `<subagent>` | <model> | <1 fresh view · 2 parallelism · 3 cheaper-model block · 4 restricted tools> | <…> |

## Flow
<Order, feedback loops, parallelization, escalation.>

## Deliberately Not Included
<Roles discussed and folded into the main agent or dropped — with the reason.>
```

## Root `README.md` Row

```markdown
| [`<package-name>`](agents/<package-name>/) | <purpose in a half sentence> | Skill `grill-me` (bundled) |
```

## Sign-off Checklist

- [ ] Every file of the target structure that applies exists; no placeholders left
- [ ] No `orchestrator.md` under `.claude/agents/` — the orchestrator is the package `CLAUDE.md`
- [ ] Every subagent has `name`, `description`, `model`, `tools` in the frontmatter
- [ ] Every model is exactly `opus`, `sonnet` or `haiku`
- [ ] Every subagent meets a splitting criterion, named in `ORCHESTRATION-DESIGN.md`
- [ ] A reviewer, if present, has no write tools
- [ ] `SKILL.md` is byte-identical to `.claude/skills/grill-me/SKILL.md` (`cmp`)
- [ ] The package `CLAUDE.md` carries the mode gate and the git/pull-request section
- [ ] The package has its own `.claude/settings.json`
- [ ] The root `README.md` table contains the new row; all links relative
