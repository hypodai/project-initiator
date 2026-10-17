# CV Feedback (working title)

A web product for job seekers: a landing page that collects a waitlist, then
a simple tool where an invited user uploads their CV (PDF) and gets
structured AI feedback on clarity, impact and structure — with the three
things to fix first.

**Status:** planning — bootstrapped from a project-initiator blueprint on
2026-10-16.

## Stack

- Next.js 16 (App Router), React, TypeScript
- Vercel Pro (hosting) + Vercel AI Gateway (model access, budgets)
- AI model: chosen by evaluation in T203
- Neon Postgres + Drizzle ORM
- Better Auth with Google sign-in (invite-only)
- Email: chosen in T205
- GitHub + GitHub Actions

Why these choices: [`docs/blueprint/DECISIONS.md`](docs/blueprint/DECISIONS.md).

## Getting started

Follow [`docs/blueprint/SETUP.md`](docs/blueprint/SETUP.md), then work
through the backlog in [`docs/backlog/QUEUE.md`](docs/backlog/QUEUE.md).

## Working with an AI agent

Open the repository in Claude Code (or another agent) and start with the
first task, e.g. "Start with T001". The agent's rules are in
[`AGENTS.md`](AGENTS.md).

---

© 2026 the repository owner. All rights reserved.
