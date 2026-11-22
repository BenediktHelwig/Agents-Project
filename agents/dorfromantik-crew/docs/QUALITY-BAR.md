# Quality Bar

The one standard the main session and the `reviewer` share. Where the project `CLAUDE.md` of the
target repository is stricter, it wins.

## 1. Architecture invariants

1. **Raw data only.** Persisted are score sheets (Wertungsblätter) and the player's decisions
   (fork choices, branch choices, blossom spending, manual fulfilments). Results, the tree, the
   path position and automatic unlocks (Freischaltungen) are always recomputed from them. That is
   what allows a closed score sheet to be corrected afterwards. Derived data in storage is a defect.
2. **Pure core.** Scoring, path logic and unlocks live in a core without dependencies — no
   Nextcloud, no DOM, no UI framework, no I/O. It must run unchanged inside the Nextcloud app.
3. **Finished campaigns are read-only** — view and delete only. Correcting a closed score sheet is
   a deliberate action with a warning, followed by recomputation.
4. **The variant drives the configuration.** Chosen when the campaign is created (base game or
   Sakura), it determines score sheet, graph, highscore list and achievement list.
5. **Path logic** (progress tree): at forks the player chooses, the app crosses on automatically;
   fields already crossed out cost nothing; the path continues beyond the cloud.
6. **The docs are the specification.** `docs/entscheidungen.md` → requirement docs → prototype. A
   rule the docs leave open or contradict is an open question, never a guess.
7. **The prototype does not regress.** Its existing tests stay green; it is extended, not rewritten.

## 2. Tests

- Every rule from the docs has at least one test, named after the rule, asserting it **as the docs
  state it** — not as the code happens to behave.
- Tests come first: a red commit whose failures are missing behaviour (not syntax errors or missing
  imports), then the green commit. Test files do not change after the red commit except through a
  `fix tests per review` commit that answers one defect.
- Edge cases of path logic are tested explicitly: fork with both branches, field already crossed,
  path past the cloud, nothing left to unlock, correction of a closed sheet.
- Tests are independent of each other and deterministic. Runner and command come from the
  project (`npm test`, `node:test`); no new test dependency in the core.
- Step 4: integration tests run against the Nextcloud container from `docker compose`.

## 3. Code

- Core: plain JavaScript, ES modules, JSDoc for types, no dependencies.
- Identifiers in English; game terms (Spielnummer, Freigespielt, Eure Erfolge) in comments and UI
  in German.
- Follow the conventions the code already shows. No unrelated refactoring, no scope creep.

## 4. Documentation

- The project `CLAUDE.md` "Stand" and "Roadmap" sections stay true after every step. In step 1 the
  roadmap is aligned to the order in `STEPS.md`.
- Anything non-obvious about a rule's implementation is explained where a maintainer will look
  for it. Documentation describes the code that exists, not the plan.
- Decisions the user made go to `docs/entscheidungen.md` of the target repo — rule, decision,
  reason, date.
