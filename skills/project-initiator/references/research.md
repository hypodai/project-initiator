# Research protocol

The value of this skill lives here. A recommendation is only as good as the
evidence behind it, and the evidence has to be current.

## Before searching: record the prior

For each layer, write one line in your working notes: *"Prior: X, because
…"* — the pick you'd make from memory. This is not the answer; it is the
hypothesis research must confirm or overturn. It makes the default-bias check
in `scoring.md` honest, because you can see afterwards whether research
changed anything.

## Candidate discovery

Find candidates by searching, not by listing what you remember. Aim for 3–5
per layer at standard depth.

Useful query shapes (adapt to the project, add the current year):

- `best <category> for <use case> <year>`
- `<prior> alternatives`, `<prior> vs <candidate>`
- `<category> comparison <constraint>` — e.g. "hosted in <region>", "self-hosted",
  "open source", "for non-developers", "cheap"
- developer surveys and rankings for the category (State of JS / HTML /
  React Native, Stack Overflow Developer Survey, DB-Engines, ThoughtWorks
  Technology Radar)
- curated lists (`awesome-<category>` on GitHub)
- community threads: `<category> recommendation site:reddit.com`,
  Hacker News discussions

Deliberately include, when relevant:

- at least one open-source or self-hostable option
- at least one regional or local provider when the user has data-residency
  or local-market needs (e.g. EU hosting, a local payment method, a national
  accounting system) — search in the user's language too, local options
  rarely rank in English results
- at least one option that is **not** in the reflex list in `scoring.md`

## Evidence per candidate

Collect what matters for the layer (see `layers.md`), typically:

| Evidence | Where |
|----------|-------|
| Price and free-tier limits **today** | official pricing page |
| Hard limits (timeouts, quotas, regions) | official docs |
| Maturity: first release, latest release, cadence | changelog, GitHub releases |
| Activity: commits in last 90 days, issue response, contributors | GitHub |
| Adoption trend | npm / PyPI download trends, survey rankings over years |
| Official integrations with other candidates | both vendors' docs |
| Recent events: pricing changes, licence changes, acquisitions, major outages, shutdown notices | news, status pages, community threads |
| Sentiment: recurring praise and complaints | reviews, Reddit, HN, GitHub issues |
| AI-tool friendliness: how well coding assistants handle it | docs quality, llms.txt, MCP servers, community reports |

## Source tiers

Weigh sources by tier. When sources conflict, record both and prefer the
higher tier.

1. **Primary:** official docs, pricing pages, changelogs, status pages.
2. **Measured:** GitHub activity, package download stats, benchmark repos
   with published methodology.
3. **Surveys:** large developer surveys and radars.
4. **Community:** Reddit, HN, forums, GitHub issues — look for patterns
   across many posts, not single anecdotes; prefer the last 12 months.
5. **Editorial:** blog comparisons and listicles — lowest weight. Mark
   vendor-authored comparisons as biased; discount affiliate listicles.

Review platforms (G2, Capterra, app store ratings) belong in tier 4 and are
noisy. Report what they show with sample size and date; never let one
aggregate star rating decide a layer.

## Freshness

- Prices, limits and free tiers: fetched in **this run**, or labelled
  `UNVERIFIED`.
- Everything else: prefer sources from the last 12 months; older sources are
  allowed for stable facts but must carry their date.
- Always note the access date. Use the actual current date from your
  environment, not one you assume.

## Trendy vs proven

Classify each finalist:

- **Proven:** several years in production use, stable API, predictable
  pricing history.
- **Rising:** fast adoption growth, active development, shorter track
  record.
- **Hype:** attention outpaces evidence of production use.

Rising is fine — often it is the best choice — but say so, and weigh it
against the builder's tolerance for breaking changes. A non-developer
building with AI is usually better served by proven tools that AI assistants
know well.

## Citations

Every fact that could change goes into `SOURCES.md` as:

```
[S12] <title> — <url> — accessed YYYY-MM-DD — tier N
```

and is referenced inline as `[S12]` in the blueprint documents.

## Stopping

Stop researching a layer when the top candidates are clearly separated on
the user's weighted criteria, or when more evidence keeps pointing the same
way. Don't burn searches confirming what is already clear; spend them on
close calls and on integration edges (`stack-fit.md`).

## Parallel research (deep depth)

If you can spawn subagents, split layers into groups (e.g. client + hosting,
data + identity, AI + integrations). Give each subagent a self-contained
brief:

- the one-sentence idea, the user's ranked priorities, hard constraints and
  stated preferences
- its layers and the evidence table above
- this file's source tiers and freshness rules
- the required return format: per layer, candidates with evidence, source
  lines in the `[S#]` format, and open questions

Merge the results yourself — de-duplicate sources, renumber, resolve
conflicts. Scoring and decisions stay with you, not the subagents.

## Without web access

Tell the user before starting that current data can't be verified. Offer to
continue in a clearly labelled memory-only mode where every price, limit and
version is marked `UNVERIFIED` and `SOURCES.md` says so at the top.
