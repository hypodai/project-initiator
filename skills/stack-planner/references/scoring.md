# Scoring and decisions

## 1. Hard requirements first

Eliminate any candidate that fails a must-have: data residency, required
platform, compliance, a feature the product cannot live without, a budget
ceiling it breaks even at launch. Record the reason — eliminated candidates
still appear in `DECISIONS.md`, because "why not X" is often the first
question a reader asks.

## 2. Criteria and weights

Score the remaining candidates 1–5 on the criteria relevant to the layer:

| Criterion | Meaning |
|-----------|---------|
| Fit | how well it covers this project's specific needs |
| Cost | total at launch and at the growth horizon, not just the free tier |
| Time to ship | for *this* builder, including learning curve |
| Maturity | reliability, stability of API and pricing |
| Integration | how well it connects with the rest of the chosen stack |
| Exit | how hard it is to leave: data export, open standards, open source |
| Compliance | residency, DPA, certifications the project needs |
| Performance | only when the project has real performance demands |
| AI-tool friendliness | how well coding assistants work with it — matters for vibe coding |

Derive weights from the user's ranked priorities. A reasonable mapping:
rank 1 → weight 3, rank 2 → 2, rank 3 → 1.5, unranked → 1. "Simplicity /
few vendors" feeds Time to ship and Integration; "no lock-in" feeds Exit;
"data privacy" feeds Compliance.

Every score needs a reason with a source reference. A cell without a reason
is a guess — either find evidence or write `UNVERIFIED`.

## 3. Default-bias check

LLMs reach for the same tools almost regardless of the project. The current
reflex list (extend it when you notice a new one):

| Layer | Reflex picks |
|-------|--------------|
| Web framework | Next.js, React, Tailwind, shadcn/ui |
| Hosting | Vercel, Netlify |
| Database / BaaS | Supabase, Firebase, PostgreSQL-on-anything |
| Auth | Clerk, Auth0, NextAuth / Auth.js, Supabase Auth |
| Payments | Stripe |
| AI | OpenAI, LangChain |
| Mobile | React Native + Expo, Flutter |
| Email | Resend, SendGrid |
| Automation | Zapier |

Rules:

1. If the final pick for a layer is on this list, `DECISIONS.md` must show it
   beating at least two researched alternatives on the weighted scores, plus
   one sentence answering "why is this not just the default?" in terms of
   this project.
2. If your prior was a reflex pick and your candidate list is basically the
   reflex list, you haven't researched yet — go back to discovery.
3. The reverse also holds: don't reject a popular tool to look original. A
   contrarian pick needs the same quality of evidence.
4. Popularity counts only through its concrete effects: documentation,
   hiring pool, integrations, how well AI assistants know it. Put those in
   the matching criteria; "everyone uses it" is not a criterion.

## 4. Variants

Build up to three packages:

- **Balanced** — highest weighted score with the user's weights. This is the
  recommendation.
- **Top** — best quality and capability; cost matters only up to a sane
  ceiling. For users who ranked quality or scalability first, this may equal
  Balanced.
- **Budget** — minimum total cost across launch and growth, with no layer
  scoring below 3 on Fit.

Show how the variants differ layer by layer. If two variants come out
identical, say so instead of inventing differences.

## 5. Ties

Prefer fewer vendors, then the better exit path, then the more proven tool.

## Output shape (DECISIONS.md, per layer)

```
## <Layer>

**Decision:** <tool> (<variant differences, if any>)
**Prior before research:** <tool> — <confirmed / overturned, one line why>

| Candidate | Fit | Cost | Time | Maturity | Integr. | Exit | … | Weighted |
|-----------|-----|------|------|----------|---------|------|---|----------|

**Why it won:** … [S3][S7]
**Why not the others:** <one line each>
**Default-bias check:** <required if the pick is on the reflex list>
**Revisit when:** <the condition under which this should be reconsidered>
```
