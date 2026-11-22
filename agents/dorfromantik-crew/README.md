# Dorfromantik Crew

Builds the Dorfromantik scoring app — a Nextcloud app, installable as a PWA, for the base game and
the variant Sakura — step by step inside the repo it is copied into. Game content comes from
`docs/` of that repo; this package carries only the method.

## Included Agents

| Agent | Model | Task |
| --- | --- | --- |
| main session | opusplan | reads and plans (Opus, Plan mode), builds test-first (Sonnet, auto mode), fixes, opens the pull request |
| `reviewer` | opus, read-only | one review per step: rules from `docs/` against tests, invariants, TDD sequence |

## Starting

```bash
cd agents/dorfromantik-crew
claude
```

## How a Run Proceeds

Four steps, **one session per step**: progress tree (base game) → base game completed → Sakura →
Nextcloud and PWA. Each session starts in Plan mode on Opus, reads the repo and the step's docs,
asks about open rules and presents a step plan. After you switch to **auto mode** it builds on
Sonnet: a red commit, a green commit, documentation, then the independent review (at most three
rounds). It ends with a pull request stacked on the previous step and a note to start a new
session. Open rule questions go to `docs/offene-fragen.md`; your answers land in
`docs/entscheidungen.md` and outrank the prototype. Handover between sessions is
`.agents/progress.md`.

Merge the stacked pull requests in order, with a merge commit.

If `gh` is missing or the repo is not on GitHub, branch and commits stay local and each message
waits at `.agents/pull-request-step-N.md`.

## Prerequisites in the Target Repo

- Requirement docs in `docs/` and the prototype (`src/core`, `test/`, `web/`), with a project
  `CLAUDE.md`
- `node` for `npm test`; Docker for step 4 (Nextcloud test instance)

## Adopting into Another Repo

Copy this folder into the app repo — done. The required `grill-me` skill lives in
`.claude/skills/grill-me/` and is already included. The package is built around one project; for
a different one, adapt `docs/STEPS.md` and `docs/QUALITY-BAR.md`.

## Dependencies

| Skill | Origin | Purpose |
| --- | --- | --- |
| `grill-me` | bundled under `.claude/skills/grill-me/` | clarifying open rule questions with the user |
