# Setup

Follow in order. Each step says what it produces, so a person or an agent
can check it is done. Steps 1–2 are for the owner (accounts and
credentials); the agent takes over from step 3.

## 1. Accounts

| Service | Plan to start on | Why | Link |
|---------|------------------|-----|------|
| GitHub | Free (private repository) | code, CI | https://github.com |
| Vercel | Pro ($20/mo) | hosting + AI Gateway; Hobby is non-commercial [S3] | https://vercel.com/pricing |
| Neon | Free (Launch before real users) | Postgres | https://neon.com/pricing |
| Google Cloud | free | OAuth client for "Sign in with Google" | https://console.cloud.google.com |
| Domain registrar | owner's choice | production domain | — |
| Email provider | chosen in T205 | waitlist invites | — |

For every account that can ship, pay or destroy something (GitHub, Vercel,
Neon, Google Cloud, the domain registrar, the owner's email): **turn on MFA**
and set **billing alerts and a hard spending cap** before creating anything:

- Vercel → spend management: on-demand budget $60, enable "pause projects at
  100 %" [S3].
- Vercel → AI Gateway: team budget $50/month, auto top-up off, development
  key with its own $10 budget [S5].
- Neon → spending notification at $15/month once on Launch [S9].

## 2. Repository

The local repository already exists (bootstrap commit on `main`). No GitHub
repository was created. To create one manually:

1. On GitHub, create a new **private** repository named `cv-feedback`, with
   no README, licence or .gitignore.
2. `git remote add origin git@github.com:<your-account>/cv-feedback.git`
3. `git push -u origin main`

Then:

- Protect `main`: require the CI check to pass and a pull request before
  merging — verify what your plan allows for private repositories
  (`UNVERIFIED`); if unavailable, keep it as a convention in AGENTS.md.
- Turn on, as available for private repositories: secret scanning and push
  protection, Dependabot alerts and security updates (`UNVERIFIED` which are
  free on private repos — check Settings → Code security).
- Secrets never in the repo — production values live in Vercel environment
  variables; local values in `.env.local` (git-ignored).

## 3. Scaffold

Scaffold into a temporary folder and move the files in, because
`create-next-app` expects an empty or near-empty directory (`UNVERIFIED`
for the current version — try `.` first). Confirm the flags with
`npx create-next-app@latest --help` before running; the flag names below are
`UNVERIFIED` in this run.

```bash
npx create-next-app@latest app-tmp --typescript --eslint --app --src-dir --use-npm
# move everything from app-tmp/ to the repository root, keep our README.md,
# .gitignore (merge the generated entries) and docs/, then delete app-tmp/
npm install drizzle-orm better-auth ai
npm install -D drizzle-kit
```

Check each package name on npmjs.com before installing (typosquats are
real). Test runner: the builder's habit; if none, Vitest for unit tests and
Playwright for one smoke test (versions `UNVERIFIED`).

Reference: MakerKit's Next.js + Drizzle + Better Auth docs [S14].

## 4. Services

- **Neon:** create project `cv-feedback` in a US East region (to sit next to
  Vercel's default `iad1` [S4]). Create a `dev` branch for local and preview
  use. Put the dev connection string in `.env.local` as `DATABASE_URL`, and
  the main-branch string only in Vercel's Production environment variables.
  (Optional: connect Neon through the Vercel Marketplace — `UNVERIFIED`.)
- **Google OAuth:** create an OAuth client (web), authorised redirect URIs
  for `http://localhost:3000/api/auth/callback/google` (or the dev port
  you use) and the production domain. `GOOGLE_CLIENT_ID` /
  `GOOGLE_CLIENT_SECRET` — separate clients for development and production.
- **Better Auth:** generate `BETTER_AUTH_SECRET` locally (a random 32+ byte
  value), separate per environment; `BETTER_AUTH_URL` per environment.
- **AI Gateway:** create a development API key with its own budget; on
  Vercel, production uses the project's own credentials (OIDC —
  `UNVERIFIED`, check in T202). Buy a small credit balance before T203
  (free tier covers only some models [S5]).
- **Email:** decided and set up in T205.

## 5. CI and environments

- Enable the commented steps in `.github/workflows/ci.yml` (T002). Actions
  are pinned to commit SHAs: `actions/checkout` v7.0.1 [S21],
  `actions/setup-node` v7.1.0 [S22].
- Medium risk: add a static-analysis step (CodeQL or equivalent) — check
  whether it is free for private repositories on your plan (`UNVERIFIED`);
  if not, run `npm audit` plus ESLint security rules and note it in
  SECURITY.md.
- Link the GitHub repository to a Vercel project (T003): previews for pull
  requests, production from `main`.

## 6. AI coding setup

- Tool: Claude Code, opened in the repository root. Start with "Start with
  T001".
- Agreed autonomy mode: **level 2 — semi-auto** (2026-10-16). In Claude Code
  this corresponds to the `acceptEdits` permission mode — file edits are
  accepted automatically, other commands ask unless allow-listed [S20].
  Set it yourself: either set `"defaultMode": "acceptEdits"` in your own
  user settings (`~/.claude/settings.json`) [S20], or switch modes in the
  session (the mode switcher / `Shift+Tab` cycling — `UNVERIFIED` shortcut
  in this run, see https://code.claude.com/docs/en/permission-modes).
  During T001 (accounts, credentials) prefer the default (Manual) mode.
- Project permission settings: `.claude/settings.json` is included (owner
  agreed): an allowlist for `npm run *`, `npx tsc`, read-only and commit git
  commands, and deny rules for reading `.env` files [S20]. The agent must not
  edit it.

## Done when

- [ ] A Vercel preview deployment shows the scaffold and CI is green on a PR
- [ ] Neon dev branch reachable from local and preview; production string
      only in Vercel
- [ ] MFA, billing alerts and spending caps are on for every account above
