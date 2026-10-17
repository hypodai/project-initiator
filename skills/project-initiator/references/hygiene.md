# Repository hygiene: docs profile, testing, git workflow, licence

How much process a new repository gets should match what the project is.
A weekend toy for yourself and a paid product with customers' data need
different amounts. More process also costs tokens: whatever the agent must
read, update or check at the end of every task is paid for again and again.
So the user chooses, knowing the trade, in step 8.

## 1. Docs profile

The research blueprint is written once at bootstrap in every profile — that
evidence is what this skill is for. The profile controls what exists
beyond it and what the agent must **maintain after every task**.

| | **minimal** | **balanced** | **complete** |
|--|-------------|--------------|--------------|
| Idea | the least that keeps an agent oriented and the project safe | a sensible middle for real projects | everything a team or a paying product would want |
| Fits | personal / hobby / throw-away prototypes | internal tools, most products, portfolio work | products with money, sensitive data, several contributors |
| **Always present** | `AGENTS.md`, `CLAUDE.md`, `README.md`, `CHANGELOG.md`, `docs/backlog/QUEUE.md`, ADR 0001–0002, `SECURITY.md`, CI skeleton, `dependabot.yml`, `.gitignore`, `.env.example` | same | same |
| Added | — | `THREAT-MODEL.md`; PR and issue templates; task file per task of the current phase; ADR template | everything in balanced, plus `OPERATIONS.md`, NFRs and success metrics in `PLAN.md`, a task file for every task, `CONTRIBUTING.md` (only if the repository is public and takes contributions), release checklist, a docs-keeper sub-agent |
| **Agent maintains after each task** | one `CHANGELOG` line; task status in `QUEUE.md` | + `README` when setup or behaviour changes; `ARCHITECTURE.md` when structure changes; an ADR for each architectural decision; `SECURITY.md` rules touched by the change | + `OPERATIONS.md`, `THREAT-MODEL.md` and `PLAN.md` success metrics reviewed at every phase gate; docs pass by the docs-keeper before each commit |
| Token cost | lowest — small, fast tasks | moderate | noticeable on every task; pays off when drift would hurt |
| ADR rule | only for hard-to-reverse choices | every architectural decision | every architectural decision, with alternatives |

Defaults by purpose: personal → **minimal** (offer balanced); internal and
open-source/portfolio → **balanced**; commercial → **balanced**, or
**complete** when payments, health or children's data, multi-tenancy or a
team are involved. Say what the default is and why; the user can mix (e.g.
balanced plus the threat model off). If the user's budget is tight, say that
tokens are part of it: complete costs noticeably more on every task, and
balanced plus the threat model is often the better trade.

Present it as a trade-off: *"complete keeps the documentation trustworthy
as the project grows, at the price of extra tokens and a slower pace on
every task; minimal keeps tasks cheap, and the documentation will drift
unless you refresh it occasionally."* Record the profile in `AGENTS.md`.

**Fill the template to the profile:** the *Documentation upkeep* section of
`AGENTS.md` lists exactly the "Agent maintains" row for the chosen profile;
delete files and sections from the other columns instead of leaving them
empty.

## 2. Testing

Why: in AI-built code the tests are the only reviewer that is never tired and
never skims. A builder who doesn't read the code needs something that fails
when the agent breaks it, and CI that refuses to merge when it does.

The *Testing* section of `AGENTS.md` carries:

- logic changes come with tests; a bug fix starts with a failing test that
  reproduces it
- the agent never weakens, deletes or skips a test to get green — if a test
  seems wrong, it says so and asks
- levels as the stack supports: unit tests for logic, integration tests where
  code meets the database or an external API, one end-to-end smoke test of
  the core flow (balanced and complete)
- no production data in tests; external services faked or sandboxed
- AI features: a small fixed evaluation set whose results are compared
  before changing a prompt or model
- the exact commands: `{{verify_commands}}` (the first scaffold task wires up
  the test runner and a first passing test)

Scale: **minimal** — tests for anything touching money, auth or data
integrity, plus CI green; **balanced/complete** — the full list.

## 3. Git workflow

Why: with an agent writing the changes, a branch plus a pull request is the
review point and the undo unit — CI runs before anything reaches `main`, and
a bad change reverts as one piece. For a one-person prototype that is
sometimes more ceremony than it is worth.

| Workflow | Fits | Rules |
|----------|------|-------|
| **A. main-only** | personal / hobby, minimal | commit straight to `main` after each task; CI must stay green; tag working states |
| **B. task branch + PR** (default for balanced and complete) | everything else | `main` protected (verify what the repository's plan allows); branch `txxx-short-name` per task; PR with the template; CI green; squash merge; the person merges (or explicitly delegates) |

**B needs a remote** to hold the pull requests and the branch protection. If
the user creates no remote yet, start with **A**, add a backlog task "move to
workflow B once the repository has a remote", and say so in `AGENTS.md` —
don't simulate PRs with local branches.

In both: commits `Txxx: summary` (the bootstrap commit is `T000: …`),
no force-push, no history rewrite (hard gate). **Complete** adds SemVer tags and a release checklist (changelog
moved from `[Unreleased]`, tag, deploy, verify, rollback path). Conventional
Commits are optional; only suggest them if release automation is planned.

## 4. Licence and visibility

Why: code on a public repository without a licence is, by default, not
reusable by anyone; and the licences of the project's dependencies can
restrict what the project itself may do. Two questions, asked in step 8:

1. **Will the repository be private or public?** Default private; never public
   unless the user asks (existing rule).
2. **If public or shared — what do you want others to be able to do?**
   Explain briefly: permissive licences (MIT, Apache-2.0 — anyone may reuse,
   including in closed products; Apache adds a patent grant), copyleft
   (GPL, AGPL — derived work must stay open), or "all rights reserved" (view
   but not reuse). Don't pick silently; put the user's choice into a `LICENSE`
   file with their name / organisation and year. For private commercial
   projects: no `LICENSE`, a "© owner, all rights reserved" line in the
   README, and the dependency licence check from `stack-fit.md`.

If the user isn't sure, say what each means for their situation and suggest
a default; the decision is theirs.

## 5. Contributing

`CONTRIBUTING.md` is for people outside the project who may send changes:
how to set up, what a good PR is, the code of conduct. In a private or
solo repository `AGENTS.md` already does that job for agents — skip the file.
Create it only if the repository is public **and** the user wants outside
contributions (template: `assets/templates/project/CONTRIBUTING.md`).
