# Bootstrap the project

The goal: the user opens the new folder in Claude Code (or another agent),
says "start with T001", and the agent already knows the stack, the rules of
the house, the backlog and how to document its work.

## Confirm first

Before writing anything, confirm with the user (step 8 of the workflow):

- **Location** — default `./<project-slug>` under the current directory.
  If the folder exists and isn't empty, ask before touching it.
- **git** — initialise a repository? (default: yes)
- **GitHub** — create a private repository? Only if `gh auth status` shows an
  authenticated account and the user says yes. Never `--public` unless the
  user explicitly asks. If `gh` isn't available or authenticated, put the
  manual steps in `SETUP.md` instead.

## Structure

```
<project>/
├── AGENTS.md                 # the agent playbook (main instructions)
├── CLAUDE.md                 # short pointer for Claude Code
├── README.md                 # what it is, status, where docs live
├── CHANGELOG.md              # Keep a Changelog
├── .gitignore                # fitted to the chosen stack
├── .github/
│   ├── pull_request_template.md
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── workflows/ci.yml      # skeleton for the chosen stack
│   └── dependabot.yml        # for the chosen package ecosystems
└── docs/
    ├── blueprint/            # everything this skill researched
    │   ├── PLAN.md
    │   ├── DECISIONS.md
    │   ├── STACK-FIT.md
    │   ├── ARCHITECTURE.md
    │   ├── HARDWARE.md
    │   ├── COSTS.md
    │   ├── SETUP.md
    │   ├── RISKS.md
    │   └── SOURCES.md
    ├── backlog/
    │   ├── QUEUE.md
    │   └── tasks/T001-….md …
    └── adr/
        ├── README.md
        ├── 0001-record-architecture-decisions.md
        └── 0002-technology-stack.md
```

## No hand-written application code

Don't scaffold the app yourself. Official generators (`create-*`, framework
CLIs, platform templates) change often and produce better, current output.
Put the exact generator command, with the options matching the decisions,
into `SETUP.md` and into the first scaffolding task in the backlog. The CI
workflow is a skeleton with the steps commented until the scaffold exists.

## Filling the templates

Templates live in `assets/templates/`:

| Template | Goes to |
|----------|---------|
| `blueprint/*.md` | `docs/blueprint/` |
| `project/AGENTS.md`, `CLAUDE.md`, `README.md`, `CHANGELOG.md` | project root |
| `project/QUEUE.md` | `docs/backlog/QUEUE.md` |
| `project/TASK.md` | one copy per task in `docs/backlog/tasks/` |
| `project/adr/*` | `docs/adr/` |
| `project/github/*` | `.github/` (stored without the dot so packaging keeps it) |

Replace every `{{…}}` placeholder;
delete sections that don't apply rather than leaving them empty. The
templates are in English; write the documents in the agreed documentation
language (see "Language" in `SKILL.md`) — translate prose and headings, keep
the anchors agents rely on (`Definition of Done`, `Hard gates`, task IDs)
recognisable, e.g. as `Definition of Done (<translation>)`. Fill
`{{conversation_language}}` and `{{docs_language}}` so later agent sessions
keep the same languages.

### AGENTS.md

The playbook is the most important file — it shapes every future session.
Fill it from the blueprint:

- **Product context** — the one-sentence idea, users, what's in and out of
  the first version (from `PLAN.md`).
- **Stack table** — one row per layer from `DECISIONS.md`, with a link to the
  decision. "Changing the stack outside this table is a hard gate."
- **Behavioural guidelines** — keep the four principles from the template
  (think before coding, simplicity first, surgical changes, goal-driven
  execution). They are adapted from Andrej Karpathy's observations on LLM
  coding pitfalls and work for any stack.
- **Industry practices** — from `RISKS.md` and the mapping below. Write them
  as concrete rules for this project, not generic advice.
- **Hard gates** — keep the generic ones; add project-specific ones
  (touching payment flows, production data, health data, sending real
  emails/SMS, paid API keys).
- **Session loop and Definition of Done** — keep; adjust the verification
  commands (`typecheck`, `test`, `build`) to the chosen stack.

### Backlog

Derive the first tasks from `SETUP.md` and `PLAN.md`, one outcome per task,
each with acceptance criteria. A typical start:

- `T001` accounts, repository and secrets in place
- `T002` scaffold with the official generator, first CI run green
- `T003` deployment pipeline to a preview environment
- `T004` the thinnest end-to-end slice of the core feature
- `T005+` remaining first-version features in priority order

Group them into phases in `QUEUE.md` (Phase 0 setup, Phase 1 first version,
…) with a gate sentence per phase.

### ADRs

`0001` records the practice itself. `0002` summarises the stack decisions and
links to `docs/blueprint/DECISIONS.md` — the ADR is the decision, the
blueprint is the evidence. Later decisions get their own ADRs.

### CHANGELOG

Start with an `[Unreleased]` entry noting the bootstrap from the blueprint.

## Industry practices mapping

Pick what applies and turn it into project rules in `AGENTS.md` and risks in
`RISKS.md`. Requirements change — verify the current state of each regulation
you cite and link the source. This is guidance for the builder, not legal
advice; say so in `RISKS.md`.

| When the project… | Practices to include |
|-------------------|---------------------|
| handles any personal data | the privacy law of every country where users live — e.g. GDPR (EU/EEA), UK GDPR, CCPA/CPRA (California), LGPD (Brazil), PIPEDA (Canada), APPI (Japan), DPDP Act (India): data minimisation, lawful basis, privacy policy, processor agreements (DPA), required data region, export and deletion on request |
| handles health or biometric data | special-category data under GDPR (explicit consent, DPIA), HIPAA if US patients, local health-data rules elsewhere, encryption at rest and in transit, no health data in logs or analytics |
| takes payments | never touch raw card data (hosted checkout / provider SDK keeps PCI scope minimal), webhook signature verification, idempotency, app-store in-app purchase rules for digital goods in mobile apps |
| targets children | COPPA (US), age-appropriate design / GDPR rules for minors, no behavioural ads |
| has AI features | EU AI Act transparency duties where applicable, prompt-injection defences, human review for consequential decisions, evaluation set for quality, cost limits per user |
| is a web app | OWASP Top 10, WCAG 2.2 AA accessibility, security headers, dependency updates |
| is a mobile app | app store review guidelines, privacy labels / data safety forms, permission minimisation, crash reporting |
| is public-sector or sells to it | accessibility obligations (e.g. EN 301 549 in the EU, Section 508 in the US), procurement / security certifications |
| is B2B SaaS | tenant isolation tests, audit logs, SSO readiness, SOC 2 / ISO 27001 path if customers will ask |
| runs devices / IoT | secure boot and OTA updates, no default passwords, EU Cyber Resilience Act and radio rules where relevant |
| automates finance or accounting | audit trail, four-eyes approval, reconciliation, retention periods required by local law |

## After writing

1. `git init -b main`, stage everything, commit
   `chore: bootstrap project from stack-planner blueprint` — if the user agreed.
2. If agreed and authenticated: `gh repo create <name> --private --source . --push`.
3. Run the self-check from `SKILL.md` step 10.
