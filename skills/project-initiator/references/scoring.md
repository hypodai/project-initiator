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
| Security record | known advisories, how fast they were fixed, secure defaults, 2FA / audit features — a weak record lowers the score, a strong one is a reason to prefer it |

Derive weights from the user's ranked priorities. A reasonable mapping:
rank 1 → weight 3, rank 2 → 2, rank 3 → 1.5, unranked → 1. "Simplicity /
few vendors" feeds Time to ship and Integration; "no lock-in" feeds Exit;
"data privacy" feeds Compliance.

Two adjustments keep the weights honest:

- When two ranked priorities feed the same criterion (e.g. "speed to launch"
  and "simplicity" both feed Time to ship), the criterion takes the **higher**
  weight, not the sum — otherwise one concern is counted twice.
- **Fit never goes below weight 2.** Nobody ranks it because it is the point
  of the whole exercise; a tool that doesn't cover the project's needs can't
  win on price or popularity.

Every score needs a reason with a source reference. A cell without a reason
is a guess — either find evidence or write `UNVERIFIED`.

## 3. Stated preferences and the default-bias check

Two different situations, treated differently.

### The user already works with a tool

Familiarity is evidence, not bias. A builder who knows and enjoys a platform
ships faster, debugs faster and keeps it running longer — that is exactly
what the *Time to ship* criterion measures. So when the user named a tool
(or already has an account, a subscription, a codebase on it):

- It stays in the running unless it fails a hard requirement from step 1.
  Don't make it re-earn its place against alternatives the user didn't ask
  about.
- Score it normally; its familiarity shows up under *Time to ship*, not as a
  penalty or a bonus anywhere else.
- `DECISIONS.md` shows a short fit check (does it meet the must-haves, what
  are its known limits at the growth horizon, the cliff to watch) and, only
  if it would materially change cost, risk or exit, **one** alternative worth
  knowing. No forced two-alternative comparison, no "why not the default".
- If it clashes with a requirement, say so plainly and show the trade-off
  (see the preferences rule in `SKILL.md` step 3) — then follow the user's
  decision.

This applies equally to popular tools (Vercel, Supabase, Stripe, Next.js…)
and to niche ones. The goal is never to steer away from popular tools; it is
to make sure a pick is a *choice* and not a reflex.

### The skill is choosing on its own

LLMs reach for the same tools almost regardless of the project. For layers
where the user expressed no preference, apply the check below to your own
picks. It is a documentation duty, not a score penalty: being on the list
costs a candidate nothing, it only means the comparison must be shown. The
current reflex list (extend it when you notice a new one):

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
5. **Quick depth.** Only the 3–4 core layers are researched, so rule 1 can't
   hold everywhere. On the other layers a reflex pick may stand as
   **provisional**: name two alternatives with one line each, write
   "not researched — quick depth" instead of scores, and add a backlog task
   to confirm the pick before that layer is built. Say in the step-8 summary
   which layers are provisional. Core layers still get the full comparison.

### When the evidence ties

Sometimes candidates come out level on everything that can be looked up and
the difference only shows in use — AI model quality for this task is the
typical case, latency under real load another. Don't invent scores to break
the tie. Make a **provisional pick** by the tie rules below (fewer vendors,
better exit, more proven), keep the switch cheap (a gateway, an adapter, one
config value), and add a backlog task that decides by measurement — e.g. a
small fixed evaluation set. Write it in `DECISIONS.md` as "decided by
measurement in Txxx".

If you are an AI model and the candidates include models from your own
vendor, say so in `DECISIONS.md`: self-preference is a known bias, and the
measurement task is the remedy.

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
**Default-bias check:** <required if the skill chose a reflex-list pick; for a pick the user asked for, write "kept at the user's request" plus the short fit check>
**Revisit when:** <the condition under which this should be reconsidered>
```
