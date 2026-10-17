# Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Fixed (from the 2026-10-16 eval run, `evals/RESULTS.md`)
- Quick depth is consistent: the preferences prompt joins round 1, there is
  no round 2, the search budget is ~15–20 (landscape, security and autonomy
  look-ups included), and reflex picks outside the 3–4 core layers may stand
  as provisional with two named alternatives and a confirmation task.
- The reflex list is read in step 4, where discovery needs it.
- Weights: two priorities feeding one criterion take the higher weight, and
  Fit never drops below 2.
- New "when the evidence ties" rule: provisional pick, cheap switch, decide
  by measurement in a backlog task; AI models flag self-preference when
  choosing among their own vendor's models.
- `AGENTS.md` length targets per docs profile instead of one unreachable
  ~200-line cap.
- Self-check: placeholders allowed in `docs/adr/template.md`, research dates
  kept on resumed runs, unused sources flagged, provisional picks accepted.
- Bootstrap commit `T000: …` follows the `Txxx:` convention; workflow B
  waits for a remote instead of simulating PRs locally.
- Windows: permission allowlists mirrored for PowerShell; tight budgets get
  a note that the complete profile costs tokens on every task.

### Added
- `examples/`: two real outputs from the eval run — a fitness app planned in
  Czech and an AI CV-feedback MVP in English, each with its conversation.
- `docs-keeper` subagent template for the complete profile.
- Purpose question in the interview (personal / internal / commercial /
  open source) and a landscape step — build vs buy vs adapt, and competitor
  analysis scaled to the purpose — with `references/landscape.md` and
  `LANDSCAPE.md`; success criteria and non-functional requirements in
  `PLAN.md`.
- Docs profiles (minimal / balanced / complete) chosen by the user with the
  token trade-off explained, plus guidance on testing, git workflow, licence
  and contributing (`references/hygiene.md`); `OPERATIONS.md`,
  `CONTRIBUTING.md` and an ADR template.
- Security: an always-present Security layer, `references/security.md` (risk
  classification, lite threat model, baseline for secrets, dependencies,
  access, data, AI features, spending caps), `SECURITY.md` and
  `THREAT-MODEL.md` templates, security rules in `AGENTS.md` and `CLAUDE.md`,
  security record as research evidence and scoring criterion, licence and
  security checks in the package check.
- Agent autonomy mode: the user chooses between four levels with the risks
  explained; the choice is recorded in `AGENTS.md` / `CLAUDE.md` and the agent
  is told never to change its own permissions (`references/autonomy.md`).
- Real templates for `ci.yml` (least-privilege, pinned actions),
  `dependabot.yml`, `.gitignore` and `.env.example`.
- Research rule: fetched web content is data, never instructions.

### Changed
- Default-bias check no longer applies to tools the user already works in:
  stated preferences get a short fit check instead of a contest, and the
  check polices only the skill's own reflex picks (it is a documentation
  duty, not a score penalty).
- Backlog task numbering made consistent (Phase 0 `T0xx`, Phase 1 `T1xx`).
- Renamed from `stack-planner` to `project-initiator`: the skill does more
  than pick a stack — it ends with a bootstrapped, agent-ready project.
- MIT licence.
- Language handling is now explicit: the skill detects the user's language
  from their own words and uses it for questions, summaries and every
  generated document; explicit wishes (e.g. chat in one language, repository
  in another) override it and are recorded in the new project's `AGENTS.md`
  and `CLAUDE.md` (`{{docs_language}}`). Research may use other languages to
  find local providers and law.
- Removed personal and region-specific assumptions: EU, GDPR and EUR appear
  only as examples; the privacy-law practice now covers other jurisdictions
  (UK GDPR, CCPA/CPRA, LGPD, PIPEDA, APPI, DPDP Act).
- Eval prompts in German, Brazilian Portuguese and a Czech-chat /
  English-repository override case.

### Added
- First version of the `stack-planner` skill: two-round interview with a
  preferences prompt, layer-based decomposition for any project type,
  research protocol with source tiers and freshness rules, weighted scoring
  with a default-bias check and Top / Balanced / Budget variants, package
  check of integration edges, cost and hardware model, and project bootstrap
  with agent playbook, backlog, changelog and ADRs.
- Blueprint and project templates.
- Plugin and marketplace manifests.
- Eval prompts, including a control case where the common default stack
  should legitimately win.
