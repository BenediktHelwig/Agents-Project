# Orchestration Design: Dorfromantik Crew

## Problem

Build the Dorfromantik scoring app in a repository that already holds requirement docs and a
working base-game prototype. Four sequential steps, hard architecture rules (raw data only, pure
core), a complex path logic, rules the docs may leave open. Goal: minimal tokens at the same quality.

## Role Split

| Role | Model | Splitting criterion | Rationale |
| --- | --- | --- | --- |
| main session | opusplan | — | Opus in Plan mode reads, plans and asks; Sonnet in auto mode writes tests, code and docs in the same context |
| `reviewer` | opus | 1 fresh view · 4 restricted tools | The author's reading of the rules must not leak in; read-only by tool list |

## Flow

One step per session so the context stays small; `.agents/progress.md` is the only handover. Per
step: read → questions → plan → mode gate → red → green → docs → review (max. three attempts,
delta reviews through `SendMessage` to the same reviewer) → stacked pull request → stop.

## Why `opusplan` instead of an implementer subagent

An implementer on `sonnet` would cost a handoff prompt, a cold start that re-reads docs and
prototype, and a report in the main context — for every step and every fix round. `opusplan` gives
the same cheaper model for the writing without any of that: the plan lives in the context that
builds it. The price: the main context also holds the build. One session per step caps that.

This deviates from the repo's default of an `implementer` subagent (variant B) and from the
template's `"model": "opus"` in `settings.json` on purpose: the splitting rule's criterion 3
(a large block a cheaper model does reliably) is met by the model switch itself, so no subagent
is needed for it.

## Token measures

- One session per step; later steps' docs are not read early
- Delta re-review through `SendMessage`; the full rule-to-test match only on attempt 1
- Paths instead of content in every delegation; short fixed report formats

## Deliberately Not Included

- **Implementer subagent** — replaced by `opusplan`, see above.
- **Separate test author / acceptance agent** — the reviewer derives the rules from the docs before
  reading the tests; a second test source would need reconciling.
- **Core/UI split or parallel implementers** — same files, same data model, no real parallelism.
- **Nextcloud specialist** — step 4 is one block in the same context.
- **A Sonnet reviewer** — Sonnet writes the code; the independent view needs a different model.
