# Security

## What this repository is

`project-initiator` is a set of instructions and templates for an AI coding
agent. It contains no executable code, no dependencies and no network
service. The security questions that matter are about what the skill makes
an agent do and what it writes into new projects.

## Reporting a problem

Please use **GitHub private vulnerability reporting** (the *Security* tab →
*Report a vulnerability*) rather than a public issue. You can expect a first
reply within a week.

Worth reporting, for example:

- a way for web content fetched during research to steer the agent
  (prompt injection) despite the rule that fetched content is data, never
  instructions
- a template that weakens a generated project's security — permissive CI
  permissions, unpinned actions, secrets that would be committed, an
  allowlist that grants more than the chosen autonomy level
- instructions that lead the agent to run destructive commands, change its
  own permissions or publish anything without the user's decision

Ordinary mistakes in the advice — an outdated price, a weak stack choice —
are welcome as normal issues.

## What the skill does and does not promise

The generated `SECURITY.md`, `THREAT-MODEL.md` and agent rules are a
**baseline** for a new project, not an audit. The skill says so in the
documents it writes and recommends a professional review before a project
handles real personal, health or payment data.

## Supported versions

Only the latest release receives fixes.
