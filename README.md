# project-initiator

A Claude Code skill that turns a software idea into an evidence-backed plan
and an agent-ready repository.

Ask a plain LLM "what stack should I use for a fitness app" and you get the
same few popular tools, prices from memory, and nothing about whether the
pieces fit together. `project-initiator` works like a technology consultant
instead:

1. **Interviews you** — one round of core questions, an invitation to share
   your preferences, then follow-ups only for the parts your project actually
   has. Every question comes with a suggested default.
2. **Researches the market as it is today** — 3–5 candidates per layer found
   by search, not memory; prices and limits verified in the same run; every
   fact dated and sourced.
3. **Decides against your priorities** — weighted scoring, visible in the
   output, with a built-in check against the tools every LLM recommends by
   reflex. Popular tools are allowed; they just have to win.
4. **Checks the stack as a package** — official integrations, known issues
   between components, regions, egress costs, lock-in, vendor count.
5. **Maps hardware and costs** — what to buy or rent; monthly costs at build,
   launch and growth, including where free tiers end.
6. **Bootstraps the project** — a GitHub-structured folder (optionally a
   private GitHub repo) with `AGENTS.md`, `CLAUDE.md`, `SECURITY.md`,
   backlog, changelog, ADRs and the full research in `docs/blueprint/`, so an
   AI agent can start on task `T001` straight away.

Along the way it also:

- **checks whether you need to build at all** — buy / adapt / build, and for
  commercial products the competition; personal projects skip the market
  analysis
- **keeps the tools you already like** — your stack is checked against your
  requirements, not forced to win a contest
- **lets you choose the process weight** — a minimal, balanced or complete
  documentation profile, with the token cost of each explained
- **builds security in** — secrets, dependencies, access, spending caps and
  a lite threat model, written into the agent's instructions
- **asks how autonomous the coding agent should be**, explains the risks of
  each level and records your choice for the agent to follow (you switch the
  mode yourself in your tool)

## Install

As a Claude Code plugin from a local clone:

```
/plugin marketplace add ./project-initiator
/plugin install project-initiator@project-initiator
```

Or copy `skills/project-initiator/` into `~/.claude/skills/`.

Web search must be available for current data. Without it the skill says so
and labels every price and version as unverified.

## Use

Just describe what you want to build:

> I want to make a fitness app for phones that tracks workouts and syncs with
> smartwatches. I can't code, I use Claude Code.

Ask for `quick` or `deep` depth if you want less or more research than the
default.

Write in any language — the interview, the summary and every generated
document follow the language you use. Want to chat in one language and get
the repository in another (say, for an international team)? Just say so.

## What you get

```
<project>/
├── AGENTS.md, CLAUDE.md, README.md, CHANGELOG.md, SECURITY.md
├── .gitignore, .env.example
├── .github/                PR and issue templates, CI skeleton, dependabot
└── docs/
    ├── blueprint/          PLAN, LANDSCAPE, DECISIONS, STACK-FIT,
    │                       ARCHITECTURE, HARDWARE, COSTS, SETUP, RISKS,
    │                       SOURCES (+ THREAT-MODEL, OPERATIONS by profile)
    ├── backlog/            QUEUE.md + task files
    └── adr/                0001 practice, 0002 technology stack, template
```

What is included beyond the core depends on the documentation profile you
choose (`minimal`, `balanced`, `complete`).

## Repository layout

| Path | What |
|------|------|
| `skills/project-initiator/SKILL.md` | the workflow |
| `skills/project-initiator/references/` | interview, layers, landscape, research protocol, scoring, package check, costs and hardware, security, hygiene (profiles, testing, git, licence), agent autonomy, bootstrap |
| `skills/project-initiator/assets/templates/` | blueprint and project templates |
| `evals/evals.json` | test prompts used to compare runs with and without the skill |

## Status

Early version (0.1.0). Test runs and a worked example are in progress.
Formerly called `stack-planner`.

## Licence

[MIT](LICENSE)
