# Transcript — eval 4, with skill (project-initiator, quick depth)

Simulated conversation. Messages from the assistant are shown as sent; the
user's answers are simulated (marked **[simulated]**).

---

## User (original request)

> Solo dev, very comfortable with Next.js and React. I want to ship an MVP in
> two weeks: landing page with waitlist, then a simple tool where users upload
> a CV and get AI feedback. Top priority is speed to launch, then few vendors.
> US hosting is fine, budget isn't a concern at this stage.

---

## Assistant — step 1 intake + step 2 round 1

Got it: **a web MVP for job seekers — a landing page that collects a
waitlist, followed by a tool where a signed-in user uploads a CV and gets AI
feedback — built by you in Next.js within two weeks, optimised for speed and
few vendors.** Correct me if any part of that is off.

I'll use **quick** depth: one round of questions, then about 10–15 web
searches on the layers that matter most (hosting, database, auth, AI),
roughly 15–20 minutes. Everything else gets a stated default you can change
later.

A few questions — answer "ok" to accept the defaults, or e.g. "2: …, rest ok":

1. **Who it's for** — who uploads the CV?
   *[Default: job seekers using it for themselves, mainly in the US, English
   CVs — not recruiters or employers screening candidates]*
2. **Purpose** — a product to sell, or a side project / portfolio piece?
   *[Default: commercial product — validate demand with the waitlist first;
   no payments in the two-week MVP]*
3. **Scale** — how many people at launch, and in a year?
   *[Default: ~1,000 waitlist signups and ~300 active users in month one;
   ~3,000 monthly active users in a year; public]*
4. **Data and constraints** — CVs contain personal data. Is it OK to send
   the CV to an AI provider under no-training / zero-retention terms, and
   to not keep the file after the feedback?
   *[Default: yes to both; users mainly in the US; no regulated industry]*
5. **Third priority** — you named speed, then few vendors. What comes third?
   *[Default: data privacy — because users hand you their CVs]*
6. **Preferences** — anything else you already use and like, have accounts
   with, or want to avoid (hosting, database, auth, AI provider, email)?
   "None" is fine. *[Default: none beyond Next.js / React]*

## User **[simulated]**

> ok

---

## Assistant — step 3 (quick depth: no round 2)

Thanks. Quick depth skips the second round, so I'm going with defaults for
the rest and will list them as assumptions in `PLAN.md`: Google sign-in only
in the MVP (invite-only for waitlist members), PDF uploads up to 4 MB, about
three reviews per user per month, no payments yet. Layers in play: client,
hosting, data, identity, AI, email, observability, security, dev workflow.

Starting the research now.

---

## Assistant — step 4 progress note (landscape)

Landscape check done: it's a crowded space (Resume Worded, Jobscan, Teal,
SkillSyncer, plus "just paste it into a chatbot"), but since you're building
a product to sell, buying isn't an option — verdict **build**. The thing to
find during the MVP is the differentiator. Moving on to the stack.

---

## Assistant — step 8 summary and settings

**Recommended package (Balanced)**

- Client: **Next.js 16 + React** — your stack, kept as is
- Hosting: **Vercel Pro** — no adapter for Next.js; Hobby doesn't allow
  commercial use
- AI access: **Vercel AI Gateway + AI SDK** — no token markup, budgets,
  zero data retention per request, AI billed on your Vercel bill
- AI model: **decided by a 10-CV evaluation in week 2** (Claude Sonnet 5.5,
  GPT-5.6 Terra, Gemini 3.8 Flash, Claude Haiku 5.5) — I found no verified
  quality difference, only price differences
- Data: **Neon Postgres + Drizzle** — free to start, plain Postgres, easy exit
  (Supabase was a close second)
- Auth: **Better Auth with Google sign-in** — a library, users stay in your
  database, no extra vendor
- Email: **picked at the end of week 2** when you send invites (Resend,
  Postmark and SES compared then)

Variants: **Top** = same plus an Opus-class model; **Budget** = Cloudflare
Workers + a Haiku/Luna-class model (~$7/month at launch, but an adapter
layer to learn).

**Three biggest risks**
1. Runaway AI spend — gateway budget, Vercel hard pause, 5 reviews/user/day,
   invite-only sign-in.
2. Feedback not better than a chatbot — fixed eval set before launch.
3. CV data mishandled — file never stored, no CV text in logs, delete button.

**Monthly cost:** ≈ $85 at launch (~300 users), ≈ $425 at ~3,000 MAU — about
85 % of that is AI tokens. ≈ $22 during the build.
**Hardware:** nothing beyond your current computer.
**Documents:** written in English (file names, task IDs and commit messages
stay English by convention).

**Before I create the project — one message, defaults in brackets:**

1. **Location** — *[`./cv-feedback`]*
2. **git** — initialise a local repository? *[yes]*
3. **GitHub** — create a **private** GitHub repository and push? *[yes, if
   `gh` is signed in]*
4. **Docs profile** — how much documentation the agent maintains:
   - *minimal* — cheapest per task; docs drift unless refreshed
   - *balanced* — README, architecture and ADRs kept current, threat model,
     PR templates; moderate token cost
   - *complete* — plus operations docs, metrics reviews, docs pass before
     every commit; noticeably more tokens per task
   *[balanced — a commercial product, no payments or team yet]*
5. **Autonomy mode** — how freely the coding agent runs (you set this in the
   tool; I only record it):
   - *1 Supervised* — asks before every edit and command
   - *2 Semi-auto* — edits accepted automatically, only allow-listed commands
     (lint, test, build, git commit) run without asking
   - *3 Auto in a sandbox* — broad auto-approval inside a container/VM with no
     production credentials
   - *4 Full auto on your machine* — no prompts at all; not recommended
   The risks grow with each level: text the agent reads (web pages, package
   READMEs, a CV) can try to steer it; mistakes happen at machine speed; on
   your own machine it can read `.env` files and keys; a loop can run up a
   bill; auto-installs pull strangers' code. Git history, a protected `main`,
   spending caps and keeping production keys off the machine soften this.
   *[level 2 — semi-auto; Manual mode while setting up accounts in T001]*
6. **Project permission file** — add `.claude/settings.json` with an
   allowlist for the verification commands and deny rules for `.env` files?
   *[yes]*
7. **Git workflow** — *A* commit straight to `main`, or *B* task branch + PR
   with CI before merging? *[B — matches the balanced profile]*
8. **Licence** — private repository, so no licence file; README says "all
   rights reserved". *[ok]*

## User **[simulated]**

> ok — but put it in the eval folder (`…\eval-4-with-skill\project\`), git
> init yes, **no GitHub repository**.

*(Per the eval instructions: location = the output folder, git = yes,
GitHub = no; everything else = defaults.)*

---

## Assistant — step 11 handoff

Done. The project is in
`./cv-feedback`
with one local commit on `main`
(`chore: bootstrap project from project-initiator blueprint`). No GitHub
repository was created — `docs/blueprint/SETUP.md` §2 has the three manual
steps when you want one (private).

**What's there:** `AGENTS.md` (agent playbook), `CLAUDE.md`, `SECURITY.md`,
`README.md`, `CHANGELOG.md`, the backlog in `docs/backlog/QUEUE.md`
(Phase 0 → 3, task files for Phase 0), ADR-0001/0002, CI skeleton with
SHA-pinned actions, Dependabot, `.env.example`, `.claude/settings.json`, and
the research in `docs/blueprint/` (plan, landscape, decisions, stack fit,
architecture, costs, setup, risks, threat model, sources).

**First step:** open the folder in Claude Code and say **"Start with
T001"**. T001 is yours more than the agent's: MFA on GitHub, Vercel, Neon,
Google Cloud, the registrar and your email; Vercel's hard spend pause; the AI
Gateway budget. Then T002 scaffolds with `create-next-app` and T003 wires up
previews. Landing page target: day 4.

**Setting the autonomy mode (level 2, semi-auto) in Claude Code:** it maps
to the `acceptEdits` permission mode. Either put `"defaultMode":
"acceptEdits"` in your user settings (`~/.claude/settings.json`), or switch
modes inside a session (mode switcher; see
https://code.claude.com/docs/en/permission-modes). Use the default Manual
mode for T001. The project's `.claude/settings.json` already allow-lists the
verification and commit commands and blocks reading `.env` files — the agent
is told never to edit it.

**Worth revisiting once you have real numbers:**
- the AI model — T203 decides it; re-run the eval at each new model release
  or if cost passes ≈ $0.05/review
- email provider — T205
- Vercel if usage charges pass ≈ $100/month; Supabase if you need file
  storage, realtime or row-level security; Clerk/WorkOS if B2B customers ask
  for SSO
- the differentiator — write it into `LANDSCAPE.md` after the first 50
  reviews

You can change the docs profile or the autonomy mode any time by telling the
agent — it updates `AGENTS.md`; the mode itself you change in the tool.
