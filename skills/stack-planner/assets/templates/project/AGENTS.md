# Agent playbook — {{project_name}}

This file guides AI coding agents (Claude Code, Cursor, Codex, …) working in
this repository. Sessions start when a person gives a task; there is no
autonomous background work.

## Product context

{{one_paragraph_idea: what it does, for whom, which problem it removes}}

- **First version includes:** {{in_scope}}
- **Not in the first version:** {{out_of_scope}}
- **Users:** {{users_and_scale}}
- **Blueprint:** `docs/blueprint/` — the research behind every decision.
  Read `PLAN.md` and `DECISIONS.md` before proposing architectural changes.

## Ground rules

1. **Backlog:** `docs/backlog/QUEUE.md` — work on one task at a time, the
   highest-priority `ready` one unless told otherwise.
2. **Commits:** after each finished task, one commit `Txxx: <summary>`;
   messages in English, focused on why.
3. **Docs:** every task adds a line to `CHANGELOG.md` under `[Unreleased]`;
   architectural decisions get an ADR in `docs/adr/`.
4. **Language:** talk to the user in {{conversation_language}}; write docs,
   backlog tasks, ADRs and the changelog in {{docs_language}}; code,
   identifiers and commit messages in English.

## Stack

Approved choices. Changing anything outside this table is a hard gate.

| Layer | Choice | Decision |
|-------|--------|----------|
{{stack_rows: | Layer | Tool | [DECISIONS.md#layer](docs/blueprint/DECISIONS.md#layer) |}}

## Behavioural guidelines

Adapted from Andrej Karpathy's observations on common LLM coding pitfalls.

### 1. Think before coding
- State assumptions explicitly in the plan or commit message.
- If a request is ambiguous in a way that changes the result, ask; otherwise
  choose the simplest reasonable interpretation and say which one.
- Push back when a simpler approach exists.

### 2. Simplicity first
- Write the minimum code that solves the task. No speculative features, no
  abstractions for single-use code, no configurability nobody asked for.
- If 200 lines could be 50, rewrite it.

### 3. Surgical changes
- Touch only what the task requires. Don't "improve" adjacent code,
  comments or formatting.
- Match the existing style. Remove only what your change made unused.

### 4. Goal-driven execution
- Turn the task into verifiable acceptance criteria first.
- Plan in 3–5 steps, implement, verify, then report.
- Prefer a failing test that the change makes pass.

## Industry practices

Rules specific to this project's domain (from `docs/blueprint/RISKS.md`):

{{industry_rules: concrete, project-specific rules, e.g. "Never log health
data", "All payment state changes come from verified webhooks"}}

## Session loop

```mermaid
flowchart TD
  Start[Task from the user] --> Read[Read QUEUE and the task file]
  Read --> Plan[Plan 3-5 steps]
  Plan --> Implement[Implement]
  Implement --> Verify[{{verify_commands}}]
  Verify --> Docs[CHANGELOG, ADR, docs]
  Docs --> Commit[Commit Txxx]
  Commit --> Report[Short summary to the user]
```

## What the agent does without asking

- Edit files in this repository for the active task
- Install dependencies the task needs, run {{verify_commands}}
- Local commits after a task
- Update the backlog, CHANGELOG, ADRs and docs
{{push_rule: e.g. "Push to the private origin (never force)" — or remove}}

## Hard gates — always ask first

| Action | Why |
|--------|-----|
| Force push, history rewrite, deleting branches or data | irreversible |
| Making the repository public or transferring it | ownership |
| Committing secrets; printing production keys | security |
| New paid service or paid API key | budget |
| Changing the stack outside the table above | scope |
| Running migrations or scripts against production data | data loss |
{{project_specific_gates: e.g. payment flows, health data, sending real emails/SMS}}

## Definition of Done

- [ ] Acceptance criteria of the task are met
- [ ] {{verify_commands}} pass
- [ ] Task status in `docs/backlog/QUEUE.md` is `done`
- [ ] Line in `CHANGELOG.md` under `[Unreleased]`
- [ ] ADR if an architectural decision was made
- [ ] No secrets in the repository
- [ ] Commit `Txxx: …`

## Documentation map

| What | Where |
|------|-------|
| Backlog | `docs/backlog/QUEUE.md` |
| Decisions | `docs/adr/` |
| Research and evidence | `docs/blueprint/` |
| Setup steps | `docs/blueprint/SETUP.md` |
| Changes | `CHANGELOG.md` |
