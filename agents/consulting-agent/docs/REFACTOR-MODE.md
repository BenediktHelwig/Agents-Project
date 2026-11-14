# Refactor Mode

Read when the client names an existing package to rework. The ground rules and the splitting
rule from `CLAUDE.md` apply unchanged. **One package per run, one pull request per package.**

## Phase 0: Startup Check
As in `CLAUDE.md`.

## Phase 1: Diagnosis, then Grill-me

1. **Read the whole package** — `CLAUDE.md`, `README.md`, `.claude/` (agents, settings, skills),
   `docs/`. Read the root `README.md` row too.
2. **Check every role** against the splitting rule: which criterion does it meet? None → it is
   a candidate to fold into the main agent.
3. **Check conformance** with [`PACKAGE-TEMPLATES.md`](PACKAGE-TEMPLATES.md):
   - orchestrator under `.claude/agents/` instead of the package `CLAUDE.md`
   - models outside `opus | sonnet | haiku`, or a model that does not fit the role
     (e.g. `haiku` writing real code)
   - missing or reworded mode gate, missing git/pull-request section, missing `settings.json`
   - a reviewer with write tools
   - `docs/BUILD-SPEC.md` left over from the old build flow — a full duplicate of the package,
     to be deleted
   - `grill-me` not byte-identical to the root copy
4. **Present the findings** as a table — finding, consequence, your recommendation — then grill
   only on the deviations. Behavior the package has today stays unless the client agrees to
   change it.

## Phase 2: Target Composition
Summarize the target: roles kept, folded, removed, models changed — and per subagent the
criterion. Then the auto-mode handover from `CLAUDE.md`, verbatim, and wait.

## Phase 3: Rework

1. **Branch first:** `git checkout -b refactor/<package-name>`.
2. Change and delete files in one response where possible. Folded roles move their
   responsibilities and self-checks into the main agent's `CLAUDE.md` or into the subagent that
   takes them over — nothing silently disappears.
3. Update the package `README.md`, `docs/ORCHESTRATION-DESIGN.md` (including *Deliberately Not
   Included* for folded roles) and, if the purpose changed, the root `README.md` row.

## Phase 4: Sign-off, Commit, Pull Request
As in `CLAUDE.md`, with the checklist from `PACKAGE-TEMPLATES.md`. Commit prefix is the
refactored package's name, e.g. `feature-crew: fold test and dev agents into one implementer`.
The pull request body lists every finding and what was done with it.
