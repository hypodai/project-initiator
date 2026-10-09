# Landscape: build, buy or adapt

Before choosing a stack, find out whether a stack is needed at all. The
cheapest project is the one that reuses something that already exists, and
for a commercial product the competition decides whether the idea is worth
the build. This runs at the start of step 4, before layer research.

## Scale it by purpose

The purpose comes from interview question 2. It sets how deep to go — and
how much to care about competitors.

| Purpose | Landscape depth |
|---------|-----------------|
| **Personal / hobby / learning** | Light: 3–5 searches for "is there already a ready-made app or open-source project that does this". Competitors don't matter. Building to learn, to enjoy it or to own it is a complete reason on its own — mention what exists, never talk the user out of it. |
| **Internal tool** (for an organisation the user works in) | Build vs buy vs configure what the organisation already licenses (Microsoft 365, Google Workspace, an existing ERP/CRM, a low-code platform). No competitor analysis; cost of ownership matters. |
| **Commercial product** | Full: direct competitors, substitutes (including spreadsheets and doing it by hand), pricing benchmarks, what the user's version does differently, who would pay and why. Name and trademark collision check. |
| **Open source / portfolio** | Similar projects (activity, stars trend, maintenance status), what would make this one distinct, name collisions, licence landscape. |

## The four options

For each, state what it is for *this* project and what the user gives up:

- **Buy** — an existing SaaS or app used as is.
- **Adapt** — configure or extend something existing: an open-source project
  to self-host, a no-code/low-code platform, an automation tool (Zapier /
  Make / n8n), the organisation's current suite.
- **Build** — custom software (the rest of this skill).
- **Hybrid** — buy or adapt the commodity parts, build only the part that
  is the actual point.

## Evidence to collect

- Candidates found by search (also in the user's language and region —
  local products rarely rank in English results), 3–6 at most.
- Per candidate: what it covers of the user's must-haves, what it lacks,
  price today, limits, lock-in and export, maintenance status. Same rules as
  `research.md` — dated, sourced, prices from this run.
- For a commercial product additionally: how competitors position and price
  themselves, recurring complaints in their reviews (an opening for the
  user), and signs that the space is crowded or abandoned. Don't invent
  market sizes or user numbers; cite a source or leave it out.
- Rough cost of ownership over three years for Buy / Adapt versus Build
  (money plus the user's time), using the budget and builder profile from
  the interview.

## Verdict and checkpoint

Give one of: **buy**, **adapt**, **build**, **hybrid** — with the deciding
reasons tied to the user's ranked priorities.

- If **build** or **hybrid**: continue with layer research, and carry the
  differentiator into `PLAN.md`.
- If **buy** or **adapt** clearly wins on the user's own priorities: stop and
  say so in chat *before* spending the layer research — one short message
  with the options, the reason, and the question "continue with a custom
  build anyway?". The user's reasons for building anyway (learning, control,
  portfolio, a feature the products lack) are legitimate; record them in
  `LANDSCAPE.md` and continue. This is advice, not a gate you hold shut.

## Success criteria

Ask nothing new — derive from the interview and state as assumptions in
`PLAN.md`: what would make the user say "this worked" (personal: "I use it
weekly"; internal: "the team stopped doing X by hand"; commercial: first
paying customers, a conversion or retention number the user would be happy
with). Keep to two or three measurable lines. They drive the first version's
scope and tell the agent later what matters.

## Output

`LANDSCAPE.md` from `assets/templates/blueprint/LANDSCAPE.md`, with sources in
`SOURCES.md`. For a light personal check, a half-page is enough.
