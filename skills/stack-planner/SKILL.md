---
name: stack-planner
description: Turns a software idea into an evidence-backed project plan and an agent-ready repository. Runs a short discovery interview, researches the current market for every layer of the stack with dated sources, scores candidates against the user's own priorities instead of defaulting to the usual LLM picks, checks that the chosen components work together as a package, maps hardware needs and costs, then bootstraps a GitHub-structured project with AGENTS.md, CLAUDE.md, backlog, changelog and ADRs. Use this whenever someone wants to build an app, SaaS, internal tool, automation, AI agent, device or any other software project and asks what stack to use, how to start, how to set it up for vibe coding or Claude Code, what it will cost or what hardware they need — even if they only describe the idea ("I want to make a fitness app", "chci automatizovat schvalování faktur") and never say "tech stack".
---

# Stack Planner

A plain LLM answer to "what stack should I use for X" is fast and generic: no
questions asked, the same handful of popular tools every time, prices from
memory, and nothing about whether the pieces actually fit together. This skill
exists to do the opposite — work like a technology consultant who asks the
right questions, checks the market as it is *today*, justifies every choice
against the user's priorities, and hands over a repository an AI agent can
start building in immediately.

## Principles

These explain *why* the workflow looks the way it does. When a situation isn't
covered below, decide by these.

- **Evidence over memory.** Your training data is stale and skewed toward
  whatever was popular when it was collected. Anything that changes — prices,
  free tiers, limits, versions, deprecations, acquisitions, licence changes,
  availability in a region — must come from a search done in this run, with a
  source and date. If you have no web access, say so before starting and label
  every such fact `UNVERIFIED`; never present remembered prices as current.
- **Defaults must be earned.** Some tools get recommended by every LLM
  regardless of the project. They are not banned — often they are genuinely
  good — but they have to win a fair comparison like everything else. See
  `references/scoring.md`.
- **Fit beats fame.** The user's ranked priorities decide the weights. A tool
  that is "the best" in general can be the wrong choice for a non-developer on
  a hobby-sized monthly budget.
- **The package matters more than the parts.** Five individually excellent
  components that integrate badly are a worse stack than five good ones with
  official integrations between them.
- **Honesty over salesmanship.** Say what you don't know. Say when an AI-built
  prototype is not enough — payments, health or financial data, security-
  critical systems — and recommend professional review before production.
  Never invent numbers, reviews or benchmarks.
- **Ask only what changes the answer.** A question earns its place only if
  different answers would lead to a different recommendation. Everything else
  becomes a stated assumption.

## Language

The skill works in any language. Everything in this skill — instructions,
example questions, templates — is written in English only because it has to
be written in something; none of that wording is meant to reach the user
as-is.

- **Detect** the language from the user's own words in the request (not from
  the OS, locale, file names or quoted material). If the request mixes
  languages, use the one the user writes their sentences in.
- **Use it for everything the user reads:** questions and their defaults,
  progress notes, the summary in step 8, and every document written into the
  new project — blueprint, `AGENTS.md`, `CLAUDE.md`, `README.md`, backlog
  tasks, ADRs, `CHANGELOG.md`. Translate the templates' prose and headings;
  don't paste their English wording.
- **Explicit wishes win.** If the user asks for a different language — for the
  conversation, for the documents, or both (a common case: chat in their own
  language, repository in English for an international team) — follow it
  from that point on. In step 8, state the documents' language in one line so
  the user can change it without an extra round of questions.
- **Stays English by convention:** file and folder names, code identifiers,
  task IDs, commit messages, and the anchor headings agents look for
  (`Definition of Done`, `Hard gates`). Say so once; if the user wants
  commits in their language too, do that.
- **Research** in whatever language finds the best sources — usually English
  for global tools, plus the user's language for local providers, local law
  and local prices. Report the findings in the user's language; source titles
  stay as published.
- **Locale details** follow the user's language and region: currency (plus
  USD, see `references/costs-hardware.md`), number and date formats.
- **Record the choice.** The new project's `AGENTS.md` and `CLAUDE.md` name
  the conversation and documentation language, so every later agent session
  keeps it.

## Depth

Infer depth from the request; default to **standard**. The user can ask for
another level at any point.

| Depth | Interview | Research | Typical effort |
|-------|-----------|----------|----------------|
| quick | round 1 only, defaults for the rest | 2–3 candidates on the 3–4 layers that matter most | ~10 searches |
| standard | both rounds | 3–5 candidates on every relevant layer, package check | ~30–50 searches |
| deep | both rounds | standard + compatibility probe on every integration edge, review sentiment, all variants fully costed, parallel subagents | 80+ searches |

Tell the user which depth you are using and roughly how long it will take
before you start researching.

## Workflow

### 1. Intake

Read the idea. Restate it in one sentence so the user can correct a
misunderstanding before it propagates. Set the depth.

### 2. Interview — round 1

Read `references/interview.md`. Ask the core questions in **one message**,
each with a suggested default the user can accept with "ok". Skip anything the
user already answered.

### 3. Preferences and round 2

After round 1 is answered:

1. Map the project onto layers (`references/layers.md`). Only layers the
   project actually has are in play.
2. Send **one message** that first invites preferences and specific
   requirements (tools they like or refuse, accounts they already have,
   languages they know, company policies, compliance), then asks the targeted
   follow-ups for the detected layers — at most about eight, chosen by how much
   the answer would change the stack.

Stated preferences are honoured, but if one clearly conflicts with a
requirement or priority, say so plainly and show the trade-off rather than
silently overriding or silently complying.

After this round, stop asking. Anything still unknown becomes an explicit
assumption in `PLAN.md`.

### 4. Research

Follow `references/research.md`. For each layer, write down your **prior** —
the tool you would have picked without research — before searching. Then
discover candidates through search, not memory, and collect evidence. At deep
depth, research layer groups in parallel with subagents if you can spawn them.

### 5. Decide

Follow `references/scoring.md`: filter by hard requirements, weight by the
user's priorities, score with evidence, run the default-bias check, and build
the variants (Top / Budget / Balanced).

### 6. Check the package

Follow `references/stack-fit.md`. If an integration edge turns out to be a
blocker, go back to step 5 for that layer rather than papering over it.

### 7. Hardware and costs

Follow `references/costs-hardware.md`.

### 8. Present and confirm

In chat, keep it short:

- the recommended package, one line per layer, and how the variants differ
- the three biggest risks
- monthly cost at launch and at the growth horizon
- the hardware the user needs to buy or rent, if any
- the language the project documents will be written in

Then ask where to create the project (default `./<project-slug>`), whether to
initialise git, and whether to create a **private** GitHub repository. Never
create a public repository unless the user explicitly asks for one.

### 9. Bootstrap the project

Follow `references/bootstrap.md`. Blueprint documents come from
`assets/templates/blueprint/`, agent and project documents from
`assets/templates/project/`. Do not hand-write application scaffolding —
official generators do that better and stay current; put their commands into
`SETUP.md` and the first backlog task instead.

### 10. Self-check

Before handing over, verify:

- every component in `ARCHITECTURE.md` appears in `DECISIONS.md`, `SETUP.md`
  and `COSTS.md`, and nothing appears in those that is missing from the
  architecture
- every volatile fact (price, limit, version) carries a source reference that
  exists in `SOURCES.md`, dated today, or is labelled `UNVERIFIED`
- every layer in `DECISIONS.md` shows the rejected candidates and why
- every reflex pick passed the default-bias check in writing
- mermaid diagrams are syntactically valid
- the backlog's first tasks match `SETUP.md` order
- `AGENTS.md` contains the stack, the industry-specific practices from
  `RISKS.md`, and the hard gates relevant to this project
- every document is in the agreed language, with no untranslated template
  prose left over

Fix what fails; don't just report it.

### 11. Handoff

Tell the user where everything is, what to do first (usually: open the project
in Claude Code and start with `T001`), and which decisions are worth
revisiting once they have real users or real numbers.

## Reference files

| File | Read when |
|------|-----------|
| `references/interview.md` | steps 2–3 |
| `references/layers.md` | step 3, and whenever unsure whether a layer applies |
| `references/research.md` | step 4 |
| `references/scoring.md` | step 5 |
| `references/stack-fit.md` | step 6 |
| `references/costs-hardware.md` | step 7 |
| `references/bootstrap.md` | step 9 |
