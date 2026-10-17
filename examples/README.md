# Examples

Two real outputs of the skill, copied unchanged from the eval run of
2026-10-16 (see [`evals/RESULTS.md`](../evals/RESULTS.md)). Each folder is the
repository the skill bootstrapped, plus `CONVERSATION.md` — every message the
skill sent, in order.

| Folder | Request | Language | Result |
|--------|---------|----------|--------|
| [`fitness-app-cs/`](fitness-app-cs/) | A non-coder on Windows wants a subscription fitness app that syncs with smartwatches, on a budget of a few hundred CZK a month | Czech — the whole repository is in Czech because the user wrote in Czech | Expo · HealthKit + Health Connect · RevenueCat · Supabase (EU) · EAS builds · Sentry; launch cost ≈ 722 CZK / month, flagged as over the user's budget; health data treated as GDPR special-category data with a professional review before launch |
| [`cv-feedback-en/`](cv-feedback-en/) | A solo Next.js developer wants an AI CV-feedback MVP in two weeks | English | The user's Next.js stack kept; Vercel · AI Gateway + AI SDK · Neon + Drizzle · Better Auth; the AI model deliberately left to a measured evaluation task; launch cost ≈ $85 / month |

Where to look first: `docs/blueprint/PLAN.md` (the plan on one page),
`docs/blueprint/DECISIONS.md` (every choice with the rejected candidates and
why), `docs/blueprint/COSTS.md`, and `AGENTS.md` (what the coding agent will
follow).

## How these were made — read before relying on them

- **Simulated user.** The runs were non-interactive: the "user" answered
  "ok" to every suggested default. A real conversation would have produced a
  more specific plan.
- **Quick depth.** The lightest research level, about 18 web look-ups per
  run. Standard depth compares more candidates on every layer.
- **Prices age.** Every price and limit was checked on 2026-10-16 and is
  dated in `SOURCES.md`. Re-check before you rely on one.
- **Earlier skill version.** The examples predate the fixes in `CHANGELOG.md`
  under "Fixed": the bootstrap commit was still `chore: …`, and `AGENTS.md`
  runs a little over today's length targets.
- **Not real projects.** No code was written; the backlogs start at `T001`.
  Nothing here is legal or security advice — the documents say so where it
  matters.
