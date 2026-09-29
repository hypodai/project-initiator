# Interview

Goal: learn everything that changes the recommendation in at most two
messages. The user came here so they would *not* have to ping-pong with an AI.

## Rules

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
2. **Where it runs** — web, iOS, Android, desktop, a physical device, inside
   another tool (Slack, Teams, Excel, ERP), API only.
3. **Scale** — users at launch and in a year; internal team or public.
4. **Who builds and maintains it** — a non-developer with AI tools, a
   developer (which languages?), a team, an agency later?
5. **Budget** — one-off for building, and monthly for running. Is there a hard
   ceiling?
6. **Timeline and ambition** — first usable version by when; prototype to
   validate an idea, or something that must run in production?
7. **Constraints** — sensitive data (health, money, children, personal data),
   where data must live (EU?), regulated industry, company IT rules, offline
   use.
8. **Priorities** — rank these (top three is enough): low cost · speed to
   launch · simplicity / few vendors · scalability · no lock-in ·
   performance · best-in-class quality · data privacy.

Example of the expected format:

> **3. Scale** — how many users at launch, and in a year?
> *[Default: a few hundred at launch, up to ~5,000 in a year, public]*

## Preferences prompt (start of round 2)

Ask this after round 1 is answered, as the first part of the round-2 message.
Its purpose is to surface things the user wouldn't think are relevant.

> Before I research: do you have any preferences or specific requirements?
> For example tools or services you already pay for or have accounts with,
> things you want to avoid, a language or framework you know, company
> policies, a preferred cloud or region, or anything you've tried before that
> did or didn't work. "None" is a perfectly good answer.

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
- Can data leave the EU / be sent to an AI provider? Any need to run models
  locally?
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

## After round 2

Stop asking. Write remaining unknowns as assumptions in `PLAN.md` under
"Assumptions", each with the value you chose and why, so the user can correct
them later and the agent in the new repo knows what to revisit.
