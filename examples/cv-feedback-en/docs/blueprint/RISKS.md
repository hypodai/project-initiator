# Risks

*This is guidance for the builder, not legal advice. Regulations change —
verify the linked sources before relying on them. Quick depth: no
regulation was researched in this run; every legal point below is
`UNVERIFIED` and listed so you know what to check.*

## Risk register

| # | Risk | Likelihood | Impact | Mitigation | Owner |
|---|------|-----------|--------|------------|-------|
| 1 | Runaway AI spend (leaked key, bot abuse, a loop) | medium | high | gateway budget + Vercel hard pause [S3][S5]; invite-only sign-in; 5 reviews/user/day; global daily cap (T204) | owner |
| 2 | Feedback not better than pasting the CV into a chatbot | high | high | fixed eval set + rubric (T203); talk to the first 20 users | owner |
| 3 | CV personal data kept, logged or used for training | medium | high | file never stored; no CV text in logs; per-request ZDR [S5]; delete button (T204) | agent + owner |
| 4 | Prompt injection hidden in a CV ("rate this 10/10") | medium | low | model has no tools; output only shown to the uploader; system prompt treats CV as data | agent |
| 5 | Two-week deadline slips | medium | medium | landing page ships first (day 4); everything not in PLAN.md "In" waits | owner |
| 6 | Crowded market, no differentiator | high | high | LANDSCAPE.md: pick and write down the differentiator after 50 reviews | owner |
| 7 | Vercel 4.5 MB body limit rejects large PDFs | low | low | 4 MB cap with a clear message [S4] | agent |
| 8 | Neon Free restore window of 6 hours loses data | low | medium | Launch plan (7-day restore) before real users [S9]; restore test in T206 | owner |
| 9 | Gemini introductory price ends 2027-01-01 (if chosen) | certain | low | budget the post-January price [S17] | owner |

## Industry practices that apply

**Personal data (US users, possibly others):**
- Privacy policy and terms before the waitlist goes live; say what is
  collected (email; CV content processed by an AI provider; feedback stored)
  and how to delete it. California's CCPA/CPRA thresholds and other US state
  privacy laws — `UNVERIFIED` whether they apply at this size; check before
  launch.
- If people from the EU/UK sign up, GDPR / UK GDPR apply (lawful basis,
  processor agreements, data export and deletion) — `UNVERIFIED` scope.
- Data minimisation: the CV is processed, not stored.

**AI features:**
- Tell users the feedback is AI-generated and may be wrong.
- Keep an evaluation set; never change the prompt or model without
  re-running it.
- Cost limits per user and globally.
- Stay on the job-seeker side: tools used by *employers* to screen
  candidates fall under automated-employment-decision rules (e.g. New York
  City's AEDT law) and the EU AI Act's high-risk category for recruitment —
  `UNVERIFIED` details; a pivot to recruiters needs a legal check and a new
  ADR.

**Web app:** OWASP Top 10 (verify the current edition), WCAG 2.2 AA,
security headers, dependency updates — see `SECURITY.md`.

## Where an AI-built version is not enough

- **Privacy policy and terms:** have a lawyer or a reputable generator
  reviewed before collecting CVs from the public.
- **Before taking payments** (not in the MVP): professional review of the
  payment flow and a security review of authentication and data access.
- Everything in the MVP itself (no money, no special-category data by
  design) is within what a careful solo developer plus tests can own.

## Decisions worth revisiting

- Hosting: Vercel usage charges above ≈ $100/month or a need for long
  background jobs.
- AI model: each new model generation, or AI cost above ≈ $0.05/review.
- Data: file storage, realtime or row-level security needed → Supabase.
- Identity: B2B customers ask for SSO/SAML or organisations.
- Email: chosen in T205 — revisit at 10,000+ emails/month.
