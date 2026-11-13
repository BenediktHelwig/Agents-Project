# Consulting Agent: Context & Design History

## Origin (2026-05-24)

The goal was an **Agent Orchestration Consultant** that applies grill-me automatically, designs
agent systems for any domain, and generates them as self-contained, pickable packages. It
advises and generates; it does not orchestrate itself.

## Rework (2026-11-13)

A review of the agent and of the packages it had produced led to these changes:

| Finding | Change |
| --- | --- |
| The template put the orchestrator under `.claude/agents/`. A subagent can neither talk to the user nor call subagents, so it could not grill or delegate. `feature-crew` had already worked around it by hand. | The orchestrator is the package `CLAUDE.md`; `.claude/agents/` holds subagents only. |
| Opus wrote the full content of every file into `BUILD-SPEC.md` (~11k output tokens for `feature-crew`); a Haiku subagent then wrote it all a second time. The spec stayed in the package and doubled the diff. | Opus writes the files directly, in parallel in one response. `package-writer` and `BUILD-SPEC.md` are gone. |
| `CLAUDE.md` was ~19 KB, loaded every turn; a third of it were templates needed in one phase. | Templates moved to `docs/PACKAGE-TEMPLATES.md`, read on demand; repetitions removed. |
| Scoping was soft and produced five-role orchestras with costly cold starts. | Binding splitting rule (fresh view, parallelism, cheaper-model block, restricted tools); variant B as default. |
| Only `opus` and `haiku` existed — `haiku` too weak for real code, `opus` expensive for it. | `sonnet` added for demanding implementation. |
| A second gate in Phase 3 contradicted "the pull request is the review point". | Removed. Phases are now 0–4. |
| Existing packages predate these rules. | Refactor mode (`docs/REFACTOR-MODE.md`), one package per run and pull request. |

## Rework Guidelines

For every further change: grill the new requirement first (what changed, why), check its impact
on the splitting rule and the templates, then adjust `CLAUDE.md` or the docs and record the
decision here.

## Open Items

- [ ] Run a full consultation end to end and pick the result into a foreign repo
- [ ] Refactor `feature-crew` with refactor mode
- [ ] Refactor `software-architect` with refactor mode
