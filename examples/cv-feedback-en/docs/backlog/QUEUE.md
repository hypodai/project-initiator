# Backlog

Priority: **P0** → **P1** → **P2**
Status: `ready` | `in_progress` | `blocked` | `done`

Each phase ends with a gate: the person confirms before the agent moves on.
Timeline target: Phase 0 on day 1, Phase 1 live by day 4, Phase 2 live by
day 14. Task files exist for the current phase; later phases get theirs when
the phase starts (docs profile `balanced`).

---

## Phase 0 — Setup (day 1)

**Gate:** accounts have MFA and spending caps, the scaffold builds, CI is
green and a Vercel preview deployment shows the scaffold.

| ID | Task | Priority | Status | Depends |
|----|------|----------|--------|---------|
| [T001](tasks/T001-accounts-and-guards.md) | Accounts, MFA, spending caps, repository and secrets | P0 | ready | — |
| [T002](tasks/T002-scaffold-and-ci.md) | Scaffold with create-next-app, test runner, first test, CI green | P0 | ready | T001 |
| [T003](tasks/T003-preview-deployments.md) | Vercel project, preview and production deployments, Neon dev branch | P0 | ready | T002 |

---

## Phase 1 — Landing page and waitlist (days 2–4)

**Gate:** the landing page is live on the production domain and real
signups land in the waitlist table; privacy policy and terms are published.

| ID | Task | Priority | Status | Depends |
|----|------|----------|--------|---------|
| T101 | Landing page + waitlist form (Server Action → `waitlist` table via Drizzle; honeypot, per-IP rate limit, unique email); name/USPTO/domain check for the product name | P0 | ready | T003 |
| T102 | Privacy policy and terms pages (what is collected, AI processing, deletion); flagged for the owner's legal review | P0 | ready | T101 |

---

## Phase 2 — CV feedback tool (days 5–14)

**Gate:** an invited user can sign in, upload a PDF and get feedback in
production; caps, deletion and the eval are in place; the launch checklist
is done.

| ID | Task | Priority | Status | Depends |
|----|------|----------|--------|---------|
| T201 | Better Auth with Google sign-in, restricted to the invite allowlist table | P0 | ready | T102 |
| T202 | Thinnest slice: upload PDF ≤ 4 MB → AI Gateway (`AI_MODEL`, per-request ZDR) → streamed structured feedback; nothing stored but the result; verify PDF input per provider, text-extraction fallback | P0 | ready | T201 |
| T203 | AI eval: 10 synthetic CVs, rubric, compare Claude Sonnet 5.5, GPT-5.6 Terra, Gemini 3.8 Flash, Claude Haiku 5.5; owner judges; ADR-0003 records the model | P0 | ready | T202 |
| T204 | Per-user daily cap (5) and global daily cap; review history; delete review / delete account; IDOR tests | P0 | ready | T202 |
| T205 | Choose the email provider (compare Resend, Postmark, Amazon SES — pricing, deliverability, caps); ADR-0004; send waitlist invites in batches (owner confirms before sending) | P1 | ready | T201 |
| T206 | Launch checklist: security headers, dependency and permissions review (medium-risk gate), Neon on Launch plan + one restore test, DPAs and licences checked, production smoke test | P0 | ready | T203, T204, T205 |

---

## Phase 3 — After launch (later)

| ID | Task | Priority | Status | Depends |
|----|------|----------|--------|---------|
| T301 | Add an error-tracking service (research first, ADR) | P2 | ready | T206 |
| T302 | Write down the differentiator in LANDSCAPE.md after 50 reviews; decide on pricing research | P2 | ready | T206 |
