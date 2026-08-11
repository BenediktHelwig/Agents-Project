# Agents

A collection of specialized Claude Code agents. Each agent lives self-contained under
`agents/<agent-name>/` and can be adopted individually into another repository.

The starting point is the **Consulting Agent**: a consultant who gathers requirements
through an interview process and designs and generates finished multi-agent systems from
them. The agents it generates flow back into this repo — each as its own folder
under `agents/`.

---

## Structure

```
.
├── .claude/
│   └── skills/            # Skills available to all agents in this repo
│       └── grill-me/
├── agents/                # One folder per agent – each self-contained
│   └── consulting-agent/
│       ├── CLAUDE.md      # The agent definition (persona, workflow, output format)
│       ├── README.md      # Purpose, usage, dependencies
│       └── docs/          # Origin context, design decisions
├── CLAUDE.md              # Conventions of this repo (applies to all agents)
├── .gitignore
└── .gitattributes
```

## Existing Agents

| Agent | Purpose | Dependencies |
| --- | --- | --- |
| [`consulting-agent`](agents/consulting-agent/) | Designs and generates multi-agent systems for any domain | Skill `grill-me` |
| [`software-architect`](agents/software-architect/) | Documents existing architectures, designs new ones, generates draw.io UML and ADRs | Skill `grill-me` (bundled) |
| [`feature-crew`](agents/feature-crew/) | Builds features test-driven in any project, adapting to its stack and conventions | Skill `grill-me` (bundled) |

## Using an Agent

Claude Code loads the `CLAUDE.md` from the directory it is started in. To work with an
agent, start the session in its folder:

```bash
cd agents/consulting-agent
claude
```

The skills from the repo root's `.claude/skills/` are available in that case, because Claude
Code includes the directories above the working directory.

> **Note:** Starting `claude` at the repo root deliberately does *not* load the Consulting
> Agent, but the repo conventions from [`CLAUDE.md`](CLAUDE.md). This keeps the repo neutral
> when multiple agents exist side by side.

### How a run proceeds

Every agent starts on Opus in Plan mode and stays there while it clarifies the assignment with
you — nothing is written yet. Once the plan is settled it asks you once to switch to **auto
mode**, then does the work in one go: a branch before the first change, a commit after each
phase, and a pull request at the end.

You review the whole change as a single diff in that pull request and merge it yourself. Remarks
come back as an additional commit on the same branch, so the pull request updates itself.

If `gh` is missing or the project is not on GitHub, branch and commits stay local and the agent
writes the pull request message to a file for you to paste. No agent ever merges, force-pushes,
or pushes to a main branch.

## Adopting an Agent into Another Repo

Every agent is scoped so it can be extracted with a single copy operation:

1. Copy the `agents/<agent-name>/` folder into the target repo.
2. Copy the skills named under *Dependencies* in the agent's README to
   `.claude/skills/<skill-name>/` in the target repo.
3. Done — `cd` into the agent folder in the target repo and start `claude`.

There are no repo-wide build steps, no path magic, and no shared state
between agents. Everything is Markdown and JSON.

## Adding a New Agent

Conventions and minimum requirements are in [`CLAUDE.md`](CLAUDE.md).
