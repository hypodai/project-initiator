# ADR-0002: Technology stack

- **Status:** accepted
- **Date:** 2026-10-16

## Context

A solo developer, very comfortable with Next.js and React, wants to ship an
MVP in two weeks: a landing page with a waitlist, then a tool where users
upload a CV and get AI feedback. Ranked priorities: speed to launch, then
few vendors, then (accepted default) data privacy. US hosting is fine and
budget is not a concern at this stage.

## Decision

- Client: Next.js 16 App Router + React — the builder's own stack.
- Hosting: Vercel Pro — native Next.js target; also bills AI tokens.
- AI access: Vercel AI Gateway + AI SDK — no markup, budgets, per-request
  zero data retention, model swappable by a string.
- AI model: decided by the T203 evaluation (ADR-0003); no verified quality
  difference between candidates.
- Data: Neon Postgres + Drizzle — plain Postgres, scale-to-zero, easy exit.
- Identity: Better Auth with Google sign-in — users stay in our database, no
  extra vendor.
- Email: deferred to T205 (ADR-0004).
- Dev workflow: GitHub + GitHub Actions + Claude Code, task branches + PRs.

Chosen variant: **Balanced**. Full comparison, rejected alternatives and
sources: [`docs/blueprint/DECISIONS.md`](../blueprint/DECISIONS.md).

## Consequences

- **Costs:** ≈ $85/month at launch, ≈ $425/month at ~3,000 MAU, mostly AI
  tokens — [COSTS.md](../blueprint/COSTS.md).
- **Lock-in:** low; the Vercel-specific configuration and the AI Gateway are
  the hardest parts to leave (the AI SDK can call providers directly).
- **Risks:** runaway AI spend, feedback quality, CV data handling —
  [RISKS.md](../blueprint/RISKS.md).
- **Revisit when:** Vercel usage charges exceed ≈ $100/month; AI cost exceeds
  ≈ $0.05/review; file storage, realtime or SSO become requirements.
