# Feature Crew

A test-driven delivery orchestra that adapts to the project it is dropped into. Its method is
fixed — tests first, implementation second, review after every phase. Its expertise is acquired:
language, frameworks, test runner and conventions are derived from the project itself.

It is the counterpart to the [`software-architect`](../software-architect/): that system designs,
this one builds what was designed.

## Agents

| Agent | Model | Task |
| --- | --- | --- |
| `orchestrator` (the session itself, this folder's `CLAUDE.md`) | opus | Talks to you, derives the project profile, plans, delegates, escalates |
| `plan-writer` | haiku | Writes the profile, feature plans and pull request messages |
| `test-agent` | haiku | Writes failing tests — the red phase |
| `dev-agent` | haiku | Makes them pass — the green phase |
| `review-agent` | opus | Judges tests, code and documentation; decides on escalation |
| `doc-agent` | haiku | Documents the feature in the project's own format |

The orchestrator is the session, not a subagent — it has to talk to you. The other five are
subagents it calls; you never start them yourself.

The `review-agent` has no write tools at all — the role that judges your code is incapable of
changing it. The orchestrator is the session itself and therefore holds every tool, but writes
nothing: planning documents go through the `plan-writer`, code and documentation through the
specialists. It uses the shell for git and for reading, nothing else.

## Starting it

```bash
cd agents/feature-crew
claude
```

`.claude/settings.json` puts the session on Opus in Plan Mode. You will be asked once per session
to switch to auto mode before anything is written; after that the work runs through in one go and
you review it as a whole in the pull request. The model switches happen on their own.

## Using it in another repository

Copy this folder into the target repository and start a session in it. That is all — the
`grill-me` skill ships with it under `.claude/skills/`.

On the first run it builds a project profile. It reads `docs/architecture/` for that, so it works
best on a project the `software-architect` has already documented. Check the profile and correct
it — your corrections are authoritative for every role from then on.

Then tell it what to build:

- *"Implement the following feature: …"*
- *"Continue the open plan."*

## What it writes into your project

| Artefact | Location | Lifetime |
| --- | --- | --- |
| Project profile | `docs/PROJECT-PROFILE.md` | committed, permanent |
| Feature plan | `.agents/plans/<feature>.md` | not committed, deleted when the feature is done |
| Pull request message | `.agents/pull-request.md` | not committed, for you to paste |

Add `.agents/` to the target project's `.gitignore` when you adopt the package.

Beyond that it writes exactly what you asked for: tests, implementation and documentation.

## Git

It creates a branch before the first change and commits after each phase, so the TDD sequence stays
visible in the history. At the end it opens a pull request — if `gh` is installed, authenticated,
and the project is on GitHub. Otherwise branch and commits stay local and you get the message at
`.agents/pull-request.md` to paste yourself.

You review the whole change as one diff and merge it. Remarks come back as an additional commit on
the same branch, so the pull request updates itself. **It never merges, never force-pushes, and
never pushes to a main branch.**

## Dependencies

| Skill | Origin | Purpose |
| --- | --- | --- |
| `grill-me` | shipped under `.claude/skills/grill-me/` | The orchestrator's feature clarification |
