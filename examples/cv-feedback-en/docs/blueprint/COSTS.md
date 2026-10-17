# Costs

*Prices fetched 2026-10-16 · amounts in USD (the owner's currency) · figures
marked `est.` are `UNVERIFIED estimate`s derived from the assumptions below*

## Usage assumptions

| Metric | Build | Launch | Growth | Why |
|--------|-------|--------|--------|-----|
| Monthly active users (tool) | 1 (owner) | 300 | 3,000 | round-1 default; growth = one-year target |
| Waitlist signups | — | ~1,000 (cumulative) | — | round-1 default |
| Reviews per user / month | 50 test runs total | 3 → 900 | 3 → 9,000 | assumption, see PLAN.md |
| Tokens per review | 8k in + 1.5k out | same | same | 1–2 page CV + prompt; Claude models count ~30 % more [S15] — planning figure uses the higher count |
| Storage per user | — | < 50 KB of feedback text | same | CV files are never stored |
| Emails | 0 | ~1,000 invites (one-off) | ~3,000 / month | invites + notifications, `est.` |

## Monthly costs — Balanced (recommended)

| Component | Plan | Build | Launch | Growth | Source |
|-----------|------|-------|--------|--------|--------|
| Vercel hosting | Pro | $20 | $20 | $20 | [S3] |
| Vercel usage (active CPU $0.128/h, memory) | Pro | ~$0 | < $5 est. | < $10 est. | [S3][S4] |
| AI tokens via AI Gateway (planning: mid-tier model ≈ $0.040/review) | pay-as-you-go, no markup | $2 | $36 | $360 | [S5][S15][S16] |
| Neon Postgres | Free → Launch | $0 | $3–6 est. | ~$20 est. | [S9] |
| Better Auth, Drizzle ORM, Next.js | open source | $0 | $0 | $0 | [S12] |
| Google OAuth | free | $0 | $0 | $0 | `UNVERIFIED` |
| Email (planning: Resend, decided in T205) | Free → Pro | $0 | $20 | $20 | [S19] |
| GitHub + Actions | Free | $0 | $0 | $0 | `UNVERIFIED` |
| **Total** | | **≈ $22** | **≈ $85** | **≈ $425** | |

## Other variants

| Variant | Differences | Build | Launch | Growth |
|---------|-------------|-------|--------|--------|
| **Top** | Opus 5.5-class model (≈ $0.081/review [S15]) | ≈ $24 | ≈ $125 | ≈ $800 |
| **Budget** | Cloudflare Workers Paid $5 [S6, tier 5]; Haiku 5.5-class model (≈ $0.002/review [S15]); Neon Free until the growth horizon; invites spread over 10 days on a free email tier | ≈ $5 | ≈ $7 | ≈ $63 |

AI is 40 % of the Balanced bill at launch and 85 % at the growth horizon —
the model chosen in T203 decides the long-run cost far more than hosting.

## Free-tier cliffs

| Component | Metric | Threshold | Cost after | Expected at |
|-----------|--------|-----------|------------|-------------|
| Vercel Hobby | commercial use | not allowed on Hobby [S3] | Pro $20/mo | day 1 |
| AI Gateway free credits | models and rate limits | subset of models, lower limits [S5] | pay-as-you-go credits | T203 (eval) |
| Neon Free | compute / storage / restore | 100 CU-hours, 1 GB per project; 6-hour restore window [S9] | Launch: $0.106/CU-h, $0.35/GB-month, 7-day restore [S9] | launch (moved deliberately for backups) |
| Email (Resend Free, planning) | daily volume | 100 emails/day, 3,000/month [S19] | Pro from $20/mo [S19] | first invite wave |
| Gemini 3.8 Flash (if chosen) | introductory price ends | 2027-01-01 | $1.50 / $7.50 per MTok [S17] | 2.5 months after launch |

## Cost guards

| Service | Alerts | Hard cap | Set to |
|---------|--------|----------|--------|
| Vercel | email/web/SMS at budget thresholds [S3] | yes — pause all projects at 100 % [S3] | $60/month on-demand budget at launch |
| AI Gateway | budgets per team / project / key [S5] | budget limit [S5]; keep auto top-up **off** | $50/month at launch; per-key budget for the development key: $10 |
| App-level | — | per-user daily cap (5 reviews/day) and a global daily cap in code (T204) | — |
| Neon | spending notifications on Launch [S9] | hard cap `UNVERIFIED` | notification at $15/month |
| Email provider | decided in T205 | — | — |

## One-off costs

| Item | Cost | Source |
|------|------|--------|
| Domain | ≈ $10–20 / year | `UNVERIFIED estimate` |
| Professional review | not required for a free beta without payments; recommended before taking money (see RISKS.md) | — |

## How to lower costs

- Pick the cheapest model that passes the T203 rubric — Haiku 5.5 or
  GPT-5.6 Luna would cut the AI line by ~90 % [S15][S16].
- Cache the static part of the prompt (system prompt and rubric): cache hits
  on Claude Sonnet/Opus 5.5 cost 0.05× the input price [S15].
- Keep the per-user daily cap; drop it only with a paid plan in place.
