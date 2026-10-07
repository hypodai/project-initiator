# Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed
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
