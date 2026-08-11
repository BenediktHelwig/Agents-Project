# Orchestration Design: feature-crew

## Problem

An agent orchestra existed in the `FoodDatabase` repository: five roles running a test-driven
workflow with review gates and a three-attempt escalation rule. The method was sound. The packaging
was not, and it had grown into the project it was built for — Blazor, SQLite, a TrueNAS deployment
diagram and hardcoded paths like `src/App/`.

The goal was a system whose **method is fixed** and whose **expertise is acquired**: dropped into
any project, it learns that project's language, frameworks, test runner and conventions, and works
inside them.

## What was wrong with the original

| Finding | Consequence |
| --- | --- |
| `orchestrator.md` was a subagent whose stated job was to interview the user | Subagents have no conversation partner. The role could not work as defined. |
| All five roles ran on `haiku`, including the orchestrator and the reviewer | Roles that judge had no judgement. A reviewer who misses defects is worse than no reviewer, because it manufactures confidence. |
| Every role carried an `## API-Endpoint` block with `POST /dev/implement-code` | Fiction. Roles are delegated to with the Agent tool; there is no HTTP anywhere. |
| Every specialist had a "specialised grill-me" question catalogue | Specialists have nobody to interview. Those catalogues belong to the agent's own work as a self-check. |
| References to `orchestration-config.json` and a `claude/agents/` directory | Neither exists in this repository's conventions. Coordination is delegation. |
| The project's stack was hardcoded throughout | Not adoptable into any other project. |

## Role Split

| Agent | Model | Why it exists |
| --- | --- | --- |
| `orchestrator` (the session) | opus | It talks to the user, so it cannot be a subagent. It derives the profile, plans, delegates and escalates. |
| `plan-writer` | haiku | The single writing hand for planning documents, so the judging roles need no write tools. |
| `test-agent` | haiku | The red phase, isolated so that nobody who writes tests also makes them pass. |
| `dev-agent` | haiku | The green phase. The specification exists; this is execution. |
| `review-agent` | opus | Judgement and the escalation decision. The one place where saving money costs the most. |
| `doc-agent` | haiku | Documentation in the project's own format, filling a shape that is already defined. |

### A profiler role was considered and rejected

An earlier draft had a sixth role that explored the target project and produced the profile. It was
dropped: profiling and planning are the same act of thought. The orchestrator cannot write an
assignment for the `test-agent` without knowing the test runner, so extracting that knowledge into
a separate role only to hand it straight back adds a delegation step and saves nothing. With the
`software-architect` documenting projects up front, the orchestrator reads `docs/architecture/`
rather than reverse-engineering source.

### Why the judging roles do not write

The `review-agent` has no `Write` and no `Edit`. This is not economy, it is containment: the role
that judges production code is incapable of changing it. A rule can be forgotten mid-context; a
missing tool cannot. It has a second effect — a role without write tools is fully functional in
Plan Mode, which is where review belongs.

For the orchestrator the same separation exists, but it is discipline rather than a locked door,
and the design says so plainly. `tools:` in the frontmatter constrains **subagents**; the
orchestrator is the session itself and holds whatever the harness grants it. It is instructed to
write nothing — planning documents go through the `plan-writer`, everything else through the
specialists — and to use the shell for git and reading only.

Git is the one place the orchestrator acts directly. It owns branch and commit, because splitting
git across five roles produces commits that contain half of somebody else's work.

## Model Choice

| Agent | Model | Rationale |
| --- | --- | --- |
| `orchestrator` | opus | Talks to the user, decides the stack for greenfield work, judges reports, decides on escalation |
| `plan-writer` | haiku | Transfers supplied content to disk without interpreting it |
| `test-agent` | haiku | Turns acceptance criteria into tests against a documented convention |
| `dev-agent` | haiku | The tests are the specification; passing them is execution |
| `review-agent` | opus | Judgement against a standard, and the decision to escalate |
| `doc-agent` | haiku | Fills a documentation shape the profile already defines |

Two models, not three. The line runs between deciding and executing.

## Permission modes are not a role property

The original design discussion assumed a mode could be set per role — "profiler in Plan Mode, dev
agent in Auto Mode". It cannot. The permission mode belongs to the session and is inherited by
every subagent; an agent's frontmatter carries `name`, `description`, `model` and `tools`, and
nothing else.

The enforceable equivalent is the tool list, which is why it carries the design here. There is
exactly **one mode gate** per session, before the first write, and the orchestrator never asks to
switch back — review only reads, and a defect must be re-delegatable immediately.

## Flow

**Onboarding, once per project:** the orchestrator reads `docs/architecture/`, derives the profile,
and has the `plan-writer` record it at `docs/PROJECT-PROFILE.md`. The user corrects it; those
corrections are authoritative from then on. Without architecture documentation the orchestrator
recommends the `software-architect` rather than guessing from source.

**Per feature:** clarify with grill-me → plan → `plan-writer` records the plan → red (`test-agent`,
reviewed) → green (`dev-agent`, reviewed) → documentation (`doc-agent`, reviewed) → pull request
message → delete the plan.

**Escalation:** three attempts per gate. Two quiet, the third announced, then the user decides. If a
defect survives two attempts, the assignment is rewritten rather than repeated.

## Persistence and resumability

| Artefact | Location | Lifetime |
| --- | --- | --- |
| Project profile | `docs/PROJECT-PROFILE.md` | committed, permanent |
| Feature plan | `.agents/plans/<feature>.md` | not committed, deleted on completion |
| Pull request message | `.agents/pull-request.md` | not committed |

The rule: **persist what is not cheaply reproducible.** A profile carries Opus reasoning and the
user's corrections. A feature plan carries the clarification conversation. A review report carries
neither — it can be regenerated from the code at any time, so it stays a report and never becomes a
file.

Each feature plan ends with a progress checklist cut per review gate. That checklist is the resume
point: a new session reads it and continues at the first unticked line. Cutting it per feature
instead would tell a resuming session nothing.

## Git

Branch before the first change, a commit per phase so the TDD sequence stays legible in the
history, and **never a push** — remote access needs a passphrase no agent has. The pull request
message is written to disk for the user to paste; review remarks return as an additional commit on
the same branch. Only the *format* — branch naming, commit style — comes from the profile.

## Deliberately Not Included

- **A profiler role** — see above.
- **Architecture documentation and diagrams.** The `software-architect` owns `docs/architecture/`.
  Two packages writing the same files would collide.
- **A fixed documentation format.** The original produced HTML pages and draw.io diagrams with
  status colours. Good for that project, wrong for a project that documents in Markdown.
- **A fixed branch and merge request convention.** `feat/<name>-orchestrated` is a foreign body in
  a team that runs `feature/TICKET-123`.
- **Review reports as files.** Reproducible from the code, so they stay reports.
- **`sonnet` for the analytical roles.** Considered, since the `software-architect` uses it. With
  profiling folded into the orchestrator, the remaining specialists work from a written
  specification, and the split falls cleanly between `opus` and `haiku`.
