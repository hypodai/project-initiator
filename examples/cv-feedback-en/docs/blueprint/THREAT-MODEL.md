# Threat model — CV Feedback (working title)

*Lite model, created 2026-10-16. Revisit at every phase gate and when an entry
point or a data category is added. Risk level: medium.*

## Assets

| Asset | Why it matters |
|-------|----------------|
| Users' CV content and stored feedback | personal data; a leak hurts users and ends the product |
| Waitlist emails | personal data; the launch audience |
| AI Gateway credits / Vercel billing | a leaked key or abuse spends real money |
| Google OAuth client secret, Better Auth secret, DB connection string | full account takeover / data access |
| Availability during launch week | the waitlist converts only once |

## Actors

| Actor | Capability and motivation |
|-------|---------------------------|
| Anonymous visitor / bot | spam the waitlist, probe endpoints, try to reach the AI without signing in |
| Logged-in user abusing access | burn AI credits with many uploads, read another user's feedback by guessing IDs |
| Leaked or stolen credential | spend AI credits, read the database |
| Compromised dependency or CI | exfiltrate secrets at build time |
| Prompt-injected model (text hidden in a CV) | manipulate that user's feedback; no tools to abuse |
| Prompt-injected coding agent | run commands, leak `.env` during development |

## Entry points

Waitlist form (Server Action), `/api/auth/*` (Better Auth), CV upload Route
Handler, review history and delete actions, Vercel and GitHub dashboards,
CI workflow, the owner's own Google/GitHub/Vercel accounts.

## Top threats

| # | Threat | Likelihood | Impact | Mitigation (and where it lives) | Residual |
|---|--------|-----------|--------|----------------------------------|----------|
| 1 | AI credits drained by abuse or a leaked key | medium | high | sign-in + invite allowlist, 5/day per user, global daily cap (T204); gateway budget, Vercel hard pause (SETUP §1) | capped loss ≤ budget |
| 2 | IDOR: user reads someone else's review | medium | high | every query filters by `userId` from the session; test in T204 | low |
| 3 | Malicious or oversized upload | medium | medium | PDF magic-byte check, 4 MB cap, never written to disk or storage, never executed (T202) | low |
| 4 | Waitlist spam / fake signups | high | low | honeypot field, per-IP rate limit, unique email constraint (T101) | some noise |
| 5 | CV content leaks via logs or the AI provider | medium | high | no CV text or feedback in logs; per-request ZDR routing [S5] (T202) | depends on provider terms (T206) |
| 6 | Secret committed or read by the coding agent | low | high | `.gitignore`, `.claude/settings.json` deny rules, push protection (T001) | low |
| 7 | Compromised dependency | low | high | lockfile, Dependabot, `npm audit` in CI, SHA-pinned actions | medium |
| 8 | Prompt injection in a CV | medium | low | model has no tools; output rendered as text, never HTML (T202) | low |

## Accepted risks

- Single point of failure on Vercel (hosting + AI access) — accepted by the
  owner for the MVP, until real users depend on uptime.
- No error-tracking service in the MVP — accepted until Phase 3.
