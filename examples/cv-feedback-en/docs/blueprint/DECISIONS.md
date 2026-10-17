# Decisions

*Weights: Fit 1 · Cost 1 · Time to ship 3 · Maturity 1 · Integration 2 ·
Exit 1 · Compliance 1.5 (from the ranked priorities: speed → Time,
few vendors → Integration, data privacy → Compliance) · scores 1–5 ·
weighted = Σ score × weight, maximum 52.5 · sources in [SOURCES.md](SOURCES.md)*

Quick depth: 2–3 candidates on the layers that matter most (hosting, data,
identity, AI). Smaller layers are marked as not compared.

## Variants

| Layer | Balanced (recommended) | Top | Budget |
|-------|------------------------|-----|--------|
| Client | Next.js 16 | same | same |
| Hosting | Vercel Pro | same | Cloudflare Workers Paid + OpenNext |
| AI access | Vercel AI Gateway | same | direct provider API |
| AI model | chosen by T203 eval (planning figure: a mid-tier model, ≈ $0.04/review) | Claude Opus 5.5 class (≈ $0.08/review) | Claude Haiku 5.5 / GPT-5.6 Luna class (≈ $0.002–0.004/review) |
| Data | Neon (Launch plan from launch) | same | Neon Free |
| Identity | Better Auth | same | same |
| Email | T205 | same | T205 (free tier) |

Top differs from Balanced only in the model tier — infrastructure is already
the top choice for this builder. Pick **Budget** only if cost becomes a
constraint: it saves ≈ $15/month on hosting and most of the AI bill, at the
price of an adapter layer for Next.js and a separate AI vendor. Pick **Top**
if the T203 eval shows the larger model's feedback is clearly better —
"budget isn't a concern" makes that a cheap upgrade at launch volume.

---

## Client / framework

**Decision:** Next.js 16 (App Router) with React — kept at the user's request.
**Prior before research:** Next.js — confirmed (the user's own stack).

**Fit check (no forced comparison — familiarity is the strongest
time-to-ship evidence):**
- Must-haves met: public SEO-friendly landing page, server-side upload
  handling, streamed AI responses, all in one codebase.
- Limits at the growth horizon: none relevant for ~3,000 MAU.
- Cliff to watch: request body size — on Vercel a function accepts at most
  4.5 MB [S4], so CV uploads go through a Route Handler with a 4 MB cap.
  (Next.js Server Actions have their own, smaller default body limit —
  `UNVERIFIED`, check `serverActions.bodySizeLimit` in T202; use a Route
  Handler for the upload either way.)
- Alternative worth knowing: none that would materially change cost, risk
  or exit.

**Styling / UI kit:** the builder's own habit — not researched, not
prescribed. (create-next-app's Tailwind option is fine if no habit exists.)
**Default-bias check:** kept at the user's request.
**Revisit when:** never for the MVP.

---

## Hosting / runtime

**Decision:** Vercel Pro (Budget variant: Cloudflare Workers Paid + OpenNext)
**Prior before research:** Vercel — confirmed, for project-specific reasons below.

| Candidate | Fit | Cost | Time | Maturity | Integr. | Exit | Compl. | Weighted |
|-----------|-----|------|------|----------|---------|------|--------|----------|
| **Vercel Pro** | 5 | 3 | 5 | 5 | 5 | 3 | 4 | **47** |
| Cloudflare Workers + OpenNext | 4 | 5 | 3 | 3 | 3 | 4 | 3 | 35.5 |
| Netlify Pro | 4 | 3 | 4 | 4 | 3 | 3 | 3 | 36.5 |

**Why it won:** Next.js runs on Vercel with no adapter, so the builder's
existing knowledge transfers 1:1 (Time 5). Vercel also bills AI tokens
through the AI Gateway with no markup [S5], which removes a separate AI
vendor account — the few-vendors priority. Spend management includes a
hard limit that pauses projects at 100 % of budget [S3]. Pro is required
anyway: Hobby is for personal, non-commercial use [S3], and per-request
zero data retention on the AI Gateway is a Pro feature [S5]. Pro is
$20/month; on Pro, active CPU is billed from the first hour ($0.128/h) and
waiting on AI calls does not count as active CPU [S3][S4].
**Why not the others:**
- Cloudflare Workers + OpenNext — cheapest ($5/mo Workers Paid, tier-5
  source [S6]) and the adapter is reported stable since Next.js 16.2
  [S6], but it adds an adapter layer and a different runtime to learn
  during a two-week sprint; Compliance 3 because its AI/data-retention
  options were not researched.
- Netlify Pro — $20/mo with a credit model [S7]; no advantage over Vercel
  for Next.js and its spending-cap behaviour was not stated on the pricing
  page [S7].

**Default-bias check:** Vercel is a reflex pick. It is not just the default
here because the two top priorities — speed for a Next.js developer and few
vendors — are exactly where it scores (no adapter, AI billing included); on
cost, the one criterion the user deprioritised, it loses to Cloudflare.
**Revisit when:** monthly Vercel usage charges exceed ≈ $100, or long
background jobs are needed.

---

## AI access

**Decision:** Vercel AI Gateway, called through the AI SDK
**Prior before research:** direct OpenAI API — overturned (gateway removes a vendor and makes the model swappable).

| Candidate | Fit | Cost | Time | Maturity | Integr. | Exit | Compl. | Weighted |
|-----------|-----|------|------|----------|---------|------|--------|----------|
| **Vercel AI Gateway** | 5 | 5 | 5 | 4 | 5 | 4 | 4 | **49** |
| Direct provider API (Anthropic / OpenAI / Google) | 5 | 5 | 4 | 5 | 3 | 3 | 4 | 42 |

**Why it won:** provider list price with zero markup and no platform fee
[S5]; spend budgets per team, project or key [S5]; per-request zero data
retention on Pro at no extra cost [S5]; the model is a string, so the T203
eval can compare providers without new accounts.
**Why not the others:**
- Direct provider API — one more account, key and bill per provider tried;
  only advantage is provider-specific options (e.g. Anthropic US-only
  inference at 1.1× price [S15]), which "US hosting is fine" does not need.

**Caveats:** the gateway's free tier covers only a subset of models and has
lower rate limits; buying credits moves the team to the paid tier [S5].
Team-wide ZDR costs $0.10 per 1,000 requests — use the free per-request
option instead [S5].
**Default-bias check:** not a reflex pick.
**Revisit when:** a provider feature is needed that the gateway does not pass
through.

---

## AI model

**Decision:** not decided by research — **chosen by the T203 evaluation**
before launch. Development default (T202): the cheapest candidate,
Claude Haiku 5.5, configured through `AI_MODEL`.
**Prior before research:** an OpenAI model — open.

| Candidate | Price in / out per MTok | ≈ cost per review* | Notes |
|-----------|------------------------|--------------------|-------|
| Claude Sonnet 5.5 | $2 / $10 [S15] | $0.040 | Claude 4.7+ tokenizer produces ~30 % more tokens for the same text [S15] — included |
| GPT-5.6 Terra | $2 / $12 [S16] | $0.034 | |
| Gemini 3.8 Flash | $0.75 / $3.75 now, $1.50 / $7.50 from 2027-01-01 [S17] | $0.012 → $0.023 | price from a tier-5 source; free tier may train on inputs, paid tier does not [S18] |
| Claude Haiku 5.5 | $0.10 / $0.50 [S15] | $0.002 | cheapest; quality for nuanced critique unknown |
| GPT-5.6 Luna | $0.20 / $1.20 [S16] | $0.004 | |
| Claude Opus 5.5 (Top) | $4 / $20 [S15] | $0.081 | |

\* 8k input + 1.5k output tokens per review — `UNVERIFIED estimate`.

**Why no winner yet:** every candidate is reachable through the same gateway,
so Time, Integration, Exit and Compliance are equal; the only verified
difference is price, and the user ranked cost below quality-relevant
speed. Feedback quality for CVs — the criterion that actually decides — has
no trustworthy published measurement, so it is measured in T203 on a fixed
set of 10 synthetic CVs, judged by the owner against a rubric.
**Default-bias check:** OpenAI is a reflex pick and was deliberately not
chosen by default. Note also that this blueprint was written by a Claude
model: treat any preference for Claude models with suspicion — the eval
decides, not the author.
**Revisit when:** a new model generation ships, or AI cost exceeds ≈ $0.05
per review.

---

## Data

**Decision:** Neon Postgres (Free during build, Launch plan from launch) with
Drizzle ORM
**Prior before research:** Supabase — overturned (narrowly, see the table).

| Candidate | Fit | Cost | Time | Maturity | Integr. | Exit | Compl. | Weighted |
|-----------|-----|------|------|----------|---------|------|--------|----------|
| **Neon** | 5 | 5 | 4 | 4 | 4 | 5 | 3 | **43.5** |
| Supabase | 5 | 4 | 4 | 5 | 4 | 4 | 3 | 42.5 |
| Convex | 4 | 4 | 3 | 4 | 4 | 2 | 3 | 35.5 |

**Why it won:** plain Postgres with scale-to-zero and a permanent free plan
(100 CU-hours, 1 GB per project) [S9]; the Launch plan is pay-as-you-go
($0.106/CU-hour, $0.35/GB-month) with a 7-day restore window and spending
notifications [S9]; exit is a `pg_dump`. Better Auth stores its tables in it
through Drizzle [S12][S14], so there is no separate auth vendor.
**Why not the others:**
- Supabase — a close second and a fine choice. Free projects pause after
  one week of inactivity [S8] — a real risk for a waitlist page between
  launches — so it would be Pro at $25/month from day one [S8]; its bundled
  auth/storage would replace Better Auth but tie users to the platform.
- Convex — reactive document backend with first-class Next.js support
  [S10, tier 5]; a new data model to learn during a sprint and a proprietary
  API (Exit 2).

**Eliminated by hard requirements:** none.
**Default-bias check:** "PostgreSQL-on-anything" and Supabase are reflex
picks. Neon beats Supabase only on cost-to-start and exit, and Convex on
time-to-ship; the deciding project-specific reason is that the MVP needs
nothing but a few relational tables (waitlist, users, reviews), so a bare
database plus an auth library is the smallest surface.
**Not researched at quick depth:** Neon's own auth (listed on its pricing
page with 60k MAU free [S9]); billing Neon through the Vercel Marketplace
(`UNVERIFIED` — would merge two bills into one).
**Revisit when:** file storage, realtime or row-level security are needed —
then Supabase becomes the better bundle.

---

## Identity and access

**Decision:** Better Auth, Google sign-in, invite-only (waitlist allowlist)
**Prior before research:** Clerk — overturned.

| Candidate | Fit | Cost | Time | Maturity | Integr. | Exit | Compl. | Weighted |
|-----------|-----|------|------|----------|---------|------|--------|----------|
| **Better Auth** | 5 | 5 | 4 | 3 | 5 | 5 | 5 | **47.5** |
| Clerk | 4 | 4 | 5 | 5 | 3 | 3 | 3 | 41.5 |
| Auth.js v5 | 4 | 5 | 3 | 2 | 4 | 5 | 5 | 40.5 |

**Why it won:** an open-source library — users and sessions live in our own
Postgres, so no extra vendor, no MAU pricing, and no personal data at a
third party [S12]. Active: v1.7 released 2026-08-17 with frequent patch
releases, tested with Drizzle [S12]. Social sign-in and magic links are
supported, so email login can be added after T205 without a migration.
Classified **rising** (launched 2024).
**Why not the others:**
- Clerk — fastest UI (Time 5) and free to 50k monthly retained users, but
  MFA, passkeys and removing Clerk branding are paid ($25/mo Pro) [S11], and
  it adds a vendor that holds every user's identity.
- Auth.js v5 — still published as `next-auth@beta` [S13]; maturity risk for
  a new project.

**Default-bias check:** Clerk and Auth.js are reflex picks; neither was
chosen. Better Auth is not on the reflex list.
**Revisit when:** B2B customers need SSO/SAML or organisations — compare
Better Auth's plugins with Clerk/WorkOS then.

---

## Email (deferred)

**Decision:** deferred to T205 (end of week 2), when waitlist invites are sent.
**Why deferred:** nothing in week 1 needs email — the waitlist stores the
address, and sign-in is Google-only. Quick depth researched only one
candidate, Resend (Free 3,000 emails/month but 100/day; Pro from $20/month
[S19]), which is a reflex pick and cannot pass the default-bias check
without alternatives. T205 compares at least Resend, Postmark and Amazon
SES before choosing. Planning cost in COSTS.md uses Resend Pro.

---

## Observability and operations

**Decision:** Vercel runtime logs plus AI Gateway observability (included on
all plans [S3]); no separate error tracker in the MVP.
**Not compared** at quick depth. **Revisit when:** the first real users
arrive — add an error tracker in Phase 3.

---

## Security

**Decision:** platform controls first — Vercel environment variables for
secrets, Vercel spend hard limit [S3], AI Gateway budgets [S5], Better Auth
for sessions, Dependabot + `npm audit` in CI. Details in `SECURITY.md`.
**Not compared** (baseline from the skill's security guidance).

---

## Dev workflow

**Decision:** GitHub (private) + GitHub Actions + Claude Code; workflow B
(task branch + PR). **Not compared** at quick depth — GitHub plan features
for private repositories (branch protection, secret scanning) are
`UNVERIFIED` and checked in T001.
