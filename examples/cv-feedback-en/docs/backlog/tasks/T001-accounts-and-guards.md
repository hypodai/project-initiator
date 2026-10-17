# T001 — Accounts, MFA, spending caps, repository and secrets

**Phase:** 0 | **Priority:** P0 | **Status:** ready | **Depends:** —

## Goal

Every account that can ship, pay or destroy something exists, is protected
with MFA and has a spending cap — before any resource is created. This is
the owner's task (credentials); the agent guides and checks, and should run
in the default (Manual) permission mode for it.

## Steps

1. MFA on: GitHub, Vercel, Neon, Google Cloud, domain registrar, owner email.
2. Vercel: upgrade the team to Pro; spend management on-demand budget $60
   with "pause projects at 100 %" enabled ([SETUP.md §1](../../blueprint/SETUP.md)).
3. Vercel AI Gateway: team budget $50/month, auto top-up off; create a
   development API key with a $10 budget.
4. Neon: create project `cv-feedback` in US East; create a `dev` branch.
5. Google Cloud: create a project and two OAuth clients (development,
   production) — consent screen in "testing" until launch.
6. GitHub: create the private repository manually and push
   ([SETUP.md §2](../../blueprint/SETUP.md)); check which of branch
   protection, secret scanning and push protection the plan offers for
   private repositories, enable what is available, and note the result in
   `SECURITY.md`.
7. Fill `.env.local` from `.env.example` with development values only.

## Acceptance criteria

- [ ] MFA is on for all six accounts (owner confirms)
- [ ] Vercel hard pause and AI Gateway budgets are set (owner confirms with
      screenshots kept outside the repository)
- [ ] `.env.local` exists, is git-ignored, and `git status` shows no env file
- [ ] `SECURITY.md` states which GitHub protections are enabled

## Notes

- No production values on the laptop: the Neon main-branch string and the
  production OAuth secret go straight into Vercel's Production environment.
- Caveats: [STACK-FIT.md](../../blueprint/STACK-FIT.md) — Vercel↔Neon
  Marketplace billing is unverified; a direct Neon account is fine.
