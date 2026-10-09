# Interview

Goal: learn everything that changes the recommendation in at most two
messages. The user came here so they would *not* have to ping-pong with an AI.

## Rules

- Ask in the user's language (see "Language" in `SKILL.md`). The questions,
  defaults and quoted prompts below are English reference wording — translate
  the substance naturally, don't paste them.
- Ask in one message per round. Number the questions so the user can answer
  "1: …, 3: ok, rest ok".
- Every question carries a suggested default in brackets. Pick the default
  from what you already know about the idea, not a generic one.
- Skip anything already answered in the original request.
- Plain language. A non-developer must be able to answer every question.
- If an answer is vague ("cheap", "lots of users"), turn it into a number
  yourself and state it as an assumption instead of asking again.

## Round 1 — core questions

Adapt the wording to the idea; keep the substance.

1. **What and for whom** — what it does, who uses it, which problem it
   removes. *(Only if the request didn't already say.)*
2. **Purpose** — a personal / hobby / learning project, an internal tool for
   an organisation, a commercial product you want to sell, or an open-source
   / portfolio project? *(This sets how much competitor research is worth
   doing and how much process the repository needs: a weekend project for
   yourself and a paid product are different jobs. Infer the default from
   the request.)*
3. **Where it runs** — web, iOS, Android, desktop, a physical device, inside
   another tool (Slack, Teams, Excel, ERP), API only.
4. **Scale** — users at launch and in a year; internal team or public.
5. **Who builds and maintains it** — a non-developer with AI tools, a
   developer (which languages?), a team, an agency later?
6. **Budget** — one-off for building, and monthly for running. Is there a hard
   ceiling?
7. **Timeline and ambition** — first usable version by when; prototype to
   validate an idea, or something that must run in production?
8. **Constraints** — sensitive data (health, money, children, personal data),
   where users are and where data must live (a country or region such as the
   EU, US, UK…), regulated industry, company IT rules, offline use.
9. **Priorities** — rank these (top three is enough): low cost · speed to
   launch · simplicity / few vendors · scalability · no lock-in ·
   performance · best-in-class quality · data privacy.

Example of the expected format:

> **3. Scale** — how many users at launch, and in a year?
> *[Default: a few hundred at launch, up to ~5,000 in a year, public]*

## Preferences prompt (start of round 2)

Ask this after round 1 is answered, as the first part of the round-2 message.
Its purpose is to surface things the user wouldn't think are relevant.

> Before I research: do you have any preferences or specific requirements?
> For example platforms and tools you already work in and like (hosting,
> backend, database, framework — keep whatever suits you, I'll check it
> fits rather than argue you out of it), services you already pay for or
> have accounts with, things you want to avoid, a language you know, company
> policies, a preferred cloud or region, or anything you've tried before that
> did or didn't work. "None" is a perfectly good answer.

Stated tools are honoured (see "Stated preferences" in `scoring.md`): they are
checked against the must-haves, not forced to beat alternatives.

## Round 2 — targeted follow-ups

After mapping layers (`layers.md`), pick at most ~8 questions from the bank
below, ranked by how much the answer would change the stack. A question
that wouldn't change anything is not worth the user's time.

### Client apps
- Mobile: must it feel fully native, or is a cross-platform app fine? Do you
  need phone features (camera, GPS, Bluetooth, background tasks, widgets)?
- Must it work offline and sync later?
- Web: is search-engine visibility important (public marketing pages), or is
  everything behind a login?

### Data
- What are the main things the app stores, and roughly how much per user?
- Are there relationships and reporting (orders, invoices, analytics), or
  mostly documents / free-form data?
- Existing data to migrate or systems to read from?

### Accounts and access
- Who logs in: consumers, business customers, employees? Company single
  sign-on (Microsoft / Google Workspace) needed?
- Roles and permissions beyond "user / admin"? Multiple organisations
  (multi-tenant)?

### Money
- How does it make money: one-off, subscription, usage-based, marketplace
  with payouts to others?
- Sold inside mobile app stores? (App store rules change the payment options.)
- Which countries and currencies; do you need invoices / VAT handling?

### AI features
- What should the AI do: chat, extraction from documents, generation,
  classification, agents that take actions?
- Can data leave its country or region / be sent to an AI provider? Any need
  to run models locally?
- Expected volume (requests per user per day)?

### Realtime, notifications, content
- Live updates between users (chat, collaboration, live dashboards)?
- Push notifications, email, SMS?
- Uploads (photos, video, documents) — how large, how many?
- Who edits content — developers, or non-technical people through an admin?

### Integrations
- Which existing systems must it talk to (ERP, CRM, accounting, calendars,
  wearables, payment terminals)? Do they have APIs?

### Hardware and devices
- Any physical devices involved (sensors, wearables, cameras, scanners,
  single-board computers, POS)? Who buys them — you or your users?
- What computer will the builder use (Windows / macOS / Linux, RAM)? iOS apps
  normally need macOS or a cloud build service.
- Should AI models or services run on your own hardware?

### Operations
- Who fixes it at 2 a.m. if it breaks? Any uptime expectation?
- Backups and export requirements, audit logs?

### Security
Ask only what the earlier answers left open — most of it can be inferred from
the data, the audience and the money involved (`security.md`).
- Will it be reachable from the public internet, or only from a team / device
  / home network?
- What is the worst realistic day: leaked user data, someone else spending
  your money, a falsified record, the service down? Which of these would
  hurt most?
- Will users upload files, enter free text that others see, or will the
  system fetch URLs or read emails/documents on their behalf?
- Does anyone besides you get admin access (employees, contractors, an
  agency)?

## After round 2

Stop asking. Write remaining unknowns as assumptions in `PLAN.md` under
"Assumptions", each with the value you chose and why, so the user can correct
them later and the agent in the new repo knows what to revisit.
