# Eval results

## Run 2026-10-16 — evals 1 and 4, quick depth

Two prompts, each run **with the skill** (following `SKILL.md`, simulated user
answering "ok" to every suggested default, local git only) and as a
**baseline** (same model, no skill, no web research, one chat reply). Raw
outputs live in `evals/runs/2026-10-16/` (git-ignored). Both skill runs hit
an API rate limit after research and were resumed without new searches.

| | Eval 1 — fitness app (Czech) | Eval 4 — CV-feedback MVP (English, control) |
|---|---|---|
| Web lookups | 19 (13 search, 6 fetch) | 18 (6 search, 12 fetch) — over the ~10–15 quick budget |
| Files generated | 46, one local bootstrap commit | 30, one local bootstrap commit |
| Language | all documents Czech; file names, code, task IDs, commit English ✅ | all English ✅ |
| Stack | Expo · HealthKit + Health Connect · RevenueCat · Supabase (EU) · EAS · Sentry | Next.js 16 · Vercel · AI Gateway + AI SDK · Neon + Drizzle · Better Auth; AI model left to an eval task |
| Launch cost | ≈ 722 CZK / month + 15 % store fee — flagged as **over** the user's ~500 CZK ceiling | ≈ $85 / month, AI tokens ~40 % (85 % at growth) |

### Against `expected_output`

**Eval 1** — all six expectations met: interview asks platforms and watches;
iOS builds without a Mac via a cloud build service; health data treated as
GDPR special-category data; app-store purchase rules for subscriptions; costs
include developer accounts and free-tier cliffs; professional review before
real health data and payments.

**Eval 4** — control case passed: the user's Next.js stack was kept with a fit
check, not forced into a contest; Vercel won against two scored
alternatives with a written default-bias sentence; Supabase lost narrowly to
Neon on the user's weights. **Partial:** minor layers (email, UI kit,
observability) could not get two researched alternatives within the quick
budget.

### With skill vs baseline

The baselines were good: both picked sensible stacks close to the skill's
(eval 1 baseline: Expo, HealthKit / Health Connect, RevenueCat, Supabase;
eval 4 baseline: Next.js, Vercel, Supabase, Claude). The skill's value was
not a different stack but:

- **verified, dated prices** — the eval 1 baseline estimated ~250 CZK/month;
  the skill's sourced model came to ~722 CZK and said plainly that it breaks
  the user's budget
- **visible alternatives and reasons** — every layer in `DECISIONS.md` shows
  rejected candidates and why
- **competition and build-vs-buy** — eval 1 named cheap strong incumbents
  (Hevy, Strong) as a top risk; the baselines did not look
- **honest gaps** — eval 4 refused to pick an AI model without measured
  quality and turned it into a backlog task, noting the author model's
  possible self-preference
- **a working repository** instead of a chat answer — agent playbook,
  security baseline, threat model, CI, backlog, ADRs

Cost of that value: roughly 220–260k tokens per skill run against ~50k for a
baseline.

### Defects found in the skill → S021

1. Quick depth conflicts with the default-bias rule (≥ 2 researched
   alternatives for every reflex pick) on minor layers. Both runs.
2. Quick depth says "round 1 only", but the preferences prompt is defined as
   the start of round 2 — unclear whether it survives. Both runs.
3. `research.md` (step 4) needs the reflex list from `scoring.md`, which is
   listed for step 5 only. Both runs.
4. `AGENTS.md` target "well under ~200 lines" is unreachable for the
   complete profile or high-risk projects (201 and 219 lines). Both runs.
5. Self-check "no `{{…}}` anywhere" contradicts `adr/template.md`, which is
   a template by design.
6. Weight mapping: two ranked priorities can feed the same criterion, and
   Fit — the most important criterion — always ends at weight 1.
7. No "decide by measurement" outcome in `scoring.md` when candidates tie on
   everything that is published (e.g. AI model quality).
8. Bootstrap commit message (`chore: …`) conflicts with the `Txxx: …`
   convention; git workflow B assumes a remote that may not exist.
9. The complete profile mentions a docs-keeper subagent with no template.
10. Self-check "dated today" is ambiguous when research and bootstrap fall on
    different days.
11. Quick budget (~10 searches) is too tight once landscape, security and
    autonomy lookups are added — both runs needed ~18.
