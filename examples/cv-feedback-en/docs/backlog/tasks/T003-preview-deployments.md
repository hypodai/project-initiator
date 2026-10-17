# T003 — Vercel project, preview and production deployments, Neon dev branch

**Phase:** 0 | **Priority:** P0 | **Status:** ready | **Depends:** T002

## Goal

Every pull request gets a Vercel preview that talks to the Neon `dev`
branch; `main` deploys to production with production-only secrets.

## Steps

1. The owner links the GitHub repository to a new Vercel project (Vercel
   dashboard → Add New → Project) — Git integration behaviour `UNVERIFIED`
   in this run, check previews appear on PRs.
2. Environment variables in Vercel: Preview and Development scopes get the
   Neon `dev` string and the development OAuth client; Production gets the
   Neon main-branch string, the production OAuth client and its own
   `BETTER_AUTH_SECRET`.
3. Check the function region is `iad1` (default [S4]) and the Neon project
   is in US East.
4. Add a minimal Drizzle config and a `/api/health` Route Handler that runs
   `select 1` against the database; one integration test for it.
5. Open a PR; confirm the preview URL returns `ok` from `/api/health`.

## Acceptance criteria

- [ ] A PR shows a Vercel preview link; `/api/health` returns `ok` there
- [ ] Production env vars are set only in the Production scope
- [ ] `npm run lint`, `npx tsc --noEmit`, `npm test`, `npm run build` pass

## Notes

- Region alignment and the Marketplace caveat:
  [STACK-FIT.md](../../blueprint/STACK-FIT.md).
- Source references: [SOURCES.md](../../blueprint/SOURCES.md).
