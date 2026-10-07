# stack-planner — working on this repository

This repo *is* a skill; the product is the text in `skills/stack-planner/`.

- Chat with the maintainer in whatever language they write in; everything
  in the repo (skill, docs, commits) stays in English — the skill is meant
  to be published and used in any language.
- Keep the skill personal- and locale-neutral: no names, no assumed
  country, currency or regulation. Regional specifics (EU, GDPR, EUR…) only
  as one example among several.
- Keep `SKILL.md` lean (under ~300 lines). Detail belongs in `references/`,
  loaded only in the step that needs it.
- Explain *why* in instructions instead of stacking MUSTs — the model follows
  reasoning better than rules.
- Every change to the skill: a line in `CHANGELOG.md`, and re-run the evals
  in `evals/evals.json` when behaviour could change (with and without the
  skill, compare).
- Backlog: `docs/backlog/QUEUE.md`, one task at a time, commit `Sxxx: …`.
- Don't make the repo public or add a licence without the maintainer's
  explicit decision.
