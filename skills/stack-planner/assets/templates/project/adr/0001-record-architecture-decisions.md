# ADR-0001: Record architecture decisions

- **Status:** accepted
- **Date:** {{date}}

## Context

Decisions made now will be questioned later — by the team, by a future
agent session, or by a developer taking over the prototype. Without a record,
the reasoning is lost and decisions get re-litigated or silently reversed.

## Decision

Architecturally significant decisions are recorded as short ADRs in
`docs/adr/`, numbered sequentially: context, decision, consequences.
Evidence and comparisons live in `docs/blueprint/`; the ADR links to them.

A new ADR is written when changing the stack or a library in it, the data
model, the hosting or an external service, authentication, or anything a
future reader would ask "why did we do it this way?" about.

## Consequences

- A few minutes of writing per significant decision.
- Superseded ADRs stay in place with status `superseded by NNNN`.
