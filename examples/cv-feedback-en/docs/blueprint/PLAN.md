# Plan — CV Feedback (working title)

*Blueprint created 2026-10-16 · research depth: quick*

## The idea

A web product for job seekers: a landing page that collects a waitlist, then
a simple tool where a signed-in user uploads their CV (PDF) and gets
structured AI feedback on it (clarity, impact, structure, what to fix first).
It removes the "is my CV any good?" guesswork without paying a coach or
pasting the CV into a general chatbot with a home-made prompt.

**Purpose:** commercial product (validate demand first, monetise later) ·
**Landscape verdict:** build — see [LANDSCAPE.md](LANDSCAPE.md)

## First version

Two-week MVP, shipped in two steps.

**In:**
- Week 1 — landing page with value proposition and a waitlist form (email
  only), live on a production domain.
- Week 2 — sign-in (Google) for invited waitlist members; upload one PDF CV
  (≤ 4 MB); streamed, structured AI feedback; history of own reviews with a
  delete button; per-user daily limit.

**Out (later or never):**
- Payments / subscriptions — no pricing until demand is proven (add a
  Payments layer and research it then).
- Job-description matching / ATS scoring — the crowded competitor feature
  ([LANDSCAPE.md](LANDSCAPE.md)); later, if users ask.
- CV rewriting / generation, cover letters, Word (.docx) uploads.
- Recruiter or employer use (screening candidates) — would change the legal
  risk class ([RISKS.md](RISKS.md)); never without a new ADR.
- Mobile apps.

## Success criteria

Assumptions the owner can correct:

1. The landing page is live by day 4 and the CV tool by day 14.
2. ≥ 500 waitlist signups within 4 weeks of the landing page going live.
3. ≥ 40 % of invited waitlist members run at least one review, and ≥ 20 % of
   those run a second one within 14 days.

## Non-functional requirements

| Area | Target |
|------|--------|
| Performance | First feedback tokens visible within ~10 s of upload (streamed) |
| Supported browsers / devices | Current evergreen browsers, mobile-responsive landing page |
| Accessibility | WCAG 2.2 AA for landing page and upload flow |
| Data retention | CV files are never stored; review results kept until the user deletes them or the account |

## Recommended stack (Balanced)

| Layer | Choice | One-line reason |
|-------|--------|-----------------|
| Client / framework | Next.js 16 (App Router) + React | the builder's own stack — fastest to ship |
| Hosting / runtime | Vercel Pro | native Next.js target, and it also bills the AI calls (one vendor fewer) |
| AI access | Vercel AI Gateway + AI SDK | no token markup, budgets, per-request zero data retention, swap models by a string |
| AI model | decided by an eval in T203 (candidates: Claude Sonnet 5.5, GPT-5.6 Terra, Gemini 3.8 Flash, Claude Haiku 5.5) | research found no verified quality difference |
| Data | Neon Postgres + Drizzle ORM | plain Postgres, free to start, scale-to-zero, easy exit |
| Identity | Better Auth (Google sign-in) | library, users live in our own database, no extra vendor |
| Email | decided in T205 (planning candidate: Resend) | only needed for waitlist invites at the end of week 2 |
| Security | platform controls + CI audit (see `SECURITY.md`) | medium risk: personal data and public uploads |
| Dev workflow | GitHub + GitHub Actions + Claude Code | standard, low setup |

Variants and full comparison: [DECISIONS.md](DECISIONS.md).

## Costs at a glance

| | Build | Launch (~300 active users) | Growth (~3,000 MAU) |
|---|---|---|---|
| Monthly (USD) | ≈ $22 | ≈ $85 | ≈ $425 |
| One-off | domain ≈ $10–20/yr (`UNVERIFIED estimate`) | — | — |

AI tokens are the dominant, usage-driven cost. Details: [COSTS.md](COSTS.md).
Hardware: none needed beyond the builder's current computer.

## Security classification

**Data:** personal data (CVs: names, contact details, employment history;
may incidentally reveal age, nationality, health or other sensitive traits) ·
**Exposure:** public internet with file uploads · **Blast radius:** one
leaked AI/hosting key = someone else spending money; a DB leak = every
user's stored feedback · **Risk level:** medium — see `SECURITY.md` and
[THREAT-MODEL.md](THREAT-MODEL.md).

## Top risks

1. Runaway AI spend (leaked key, abuse, loop) — AI Gateway budget, Vercel
   hard spend limit, per-user daily cap, sign-in required — see [RISKS.md](RISKS.md)
2. Feedback quality not good enough to beat "paste it into a chatbot" — fixed
   eval set and model comparison before launch (T203)
3. Personal data mishandled (CV kept, logged, or used for training) — CV never
   stored, no CV text in logs, zero-data-retention routing, deletion button

## Priorities used for weighting

1. Speed to launch → *Time to ship* weight 3
2. Simplicity / few vendors → *Integration* weight 2 (and part of Time to ship)
3. Data privacy (suggested default, accepted) → *Compliance* weight 1.5

All other criteria weight 1. Budget "not a concern at this stage" — cost is
kept at weight 1 but no candidate was eliminated on cost.

## Stated preferences

| Preference | Handling |
|------------|----------|
| Next.js and React (very comfortable) | kept; fit check in DECISIONS.md, no forced comparison |
| US hosting is fine | no data-residency filter; US regions chosen to keep latency low |
| Budget not a concern | Balanced does not trade quality for cost; Budget variant shown anyway |

## Project settings

| Setting | Choice | Notes |
|---------|--------|-------|
| Docs profile | balanced | commercial product without payments yet |
| Autonomy mode | level 2 — semi-auto | set by the owner in the tool |
| Git workflow | B — task branch + PR | squash merge, owner merges |
| Repository | local git only for now (no GitHub repo created) · no licence, all rights reserved | create a private GitHub repo manually (SETUP.md §2) |
| Documentation language | English | conversation: English |

## Assumptions

| Topic | Assumed value | Why | Revisit when |
|-------|---------------|-----|--------------|
| Users | job seekers using it for themselves, mainly US, English CVs | request mentions users uploading their own CV | non-US or non-English users sign up |
| Scale | ~1,000 waitlist signups and ~300 active users in month 1; ~3,000 MAU in a year | round-1 default accepted | real numbers after week 4 |
| Reviews | 3 per active user per month; ~8k input + 1.5k output tokens per review | typical 1–2 page CV plus prompt (`UNVERIFIED estimate`) | first 100 real reviews (AI Gateway observability) |
| Sign-in | Google only in the MVP; invite-only (waitlist allowlist) | fewest moving parts, no email needed for auth | users ask for email login → add magic link after T205 |
| Uploads | PDF only, ≤ 4 MB, one at a time | Vercel 4.5 MB function body limit [S4] | users need .docx or larger files |
| Money | free beta, no payments | validate first | conversion test planned |
| AI data handling | CV text may be sent to an AI provider under zero-data-retention routing | accepted default in round 1 | a customer or law requires otherwise |
| Builder machine | an ordinary laptop that already runs Next.js dev | solo Next.js developer | — |

## Next step

Open the project in Claude Code and start with T001.
