# Package check

Individually strong picks can make a weak stack. This step checks the stack as
one system.

## 1. Integration map

List every edge — every pair of components that exchange data, identity,
events or deployments. Examples: auth ↔ database permissions, backend ↔
database driver, hosting ↔ backend runtime, payments ↔ webhooks endpoint,
AI provider ↔ data residency, mobile app ↔ push service, CI ↔ hosting.

## 2. Check each edge

For each edge, find out:

- Is there an **official** integration, SDK or guide from either vendor?
- Known problems: search `<A> <B> issue`, `<A> with <B>`, GitHub issues of
  both projects, recent community threads.
- Identity model compatibility (JWT claims, OIDC, row-level security with a
  third-party auth provider).
- Runtime compatibility (language, Node vs edge vs serverless limits, native
  modules).
- Region alignment — keeping data in the required region is pointless if one
  component processes it somewhere else.
- Data transfer / egress costs between providers.
- Rate limits that one side can hit because of the other.

Verdict per edge:

- ✅ documented and in common use
- ⚠️ works with caveats — write the caveat and its workaround
- ❌ blocker — go back to scoring and replace one side

## 3. Reference implementations

Look for official templates, starters or example repos that combine three or
more of the chosen components. One maintained by a vendor is the strongest
evidence that the combination works; cite it and put it in `SETUP.md` if it
helps the builder start.

## 4. Whole-package checks

- **Vendor count** — how many accounts, bills and dashboards. For a solo
  non-developer, every extra vendor is a real cost.
- **Single points of failure** — what takes everything down with it.
- **Total lock-in** — which layers would be hard to leave together (e.g.
  BaaS that bundles database + auth + storage).
- **Skill fit** — how many languages and paradigms the builder must learn.
- **Local development** — can the whole thing run locally or in emulators?
- **Observability coverage** — can the builder see errors from every part?
- **Backup and export** — can all important data be exported without the
  vendor's help?
- **Compliance chain** — every processor of personal data has a DPA and the
  required region; list them as sub-processors.
- **Cost coupling** — usage in one component that drives cost in another
  (e.g. every page view calling an AI API).
- **Licence chain** — do the components' licences fit the project's intended
  licence and business model (strong copyleft such as GPL/AGPL in a closed
  product, source-available or "fair-code" terms that restrict commercial
  use or hosting, paid tiers needed for commercial use)? Verify the licence
  text, not the badge.
- **Security chain** — one account compromise that reaches everything (a
  single GitHub / cloud / registrar login without MFA), shared credentials
  between environments, components with a poor advisory record
  (`research.md`).

## Output (STACK-FIT.md)

- the integration map as a mermaid diagram
- the edge table with verdicts and sources
- reference implementations found
- whole-package findings, most important first
