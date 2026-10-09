# Setup

Follow in order. Each step says what it produces, so a person or an agent
can check it is done.

## 1. Accounts

| Service | Plan to start on | Why | Link |
|---------|------------------|-----|------|

For every account that can ship, pay or destroy something (repository host,
hosting / cloud, domain registrar, payment provider, AI provider, owner
email): **turn on MFA** and set **billing alerts and a hard spending cap**
before creating anything.

## 2. Repository

{{git + GitHub steps; if gh wasn't available: create a private repo manually,
then `git remote add origin …` and `git push -u origin main`}}

- Protect `main`: require CI to pass before merging — verify what the plan
  and visibility allow; if unavailable, say so and keep CI as a convention.
- Turn on: secret scanning and push protection, Dependabot alerts and
  updates, private vulnerability reporting (public repos) — as available.
- Secrets never in the repo — {{where secrets live for this stack}}.

## 3. Scaffold

```bash
{{exact official generator command with options matching DECISIONS.md}}
```

Reference template (if found): {{link}}

## 4. Services

{{per service: create project, region, keys → where they go (least scope,
separate development and production keys), connection check}}

## 5. CI and environments

{{enable .github/workflows/ci.yml steps — pin actions to SHAs; add the
secret-scanning / static-analysis step for medium and high risk; preview and
production environments}}

## 6. AI coding setup

{{coding agent and model; how to start: open the repo and run the first task.
The agreed autonomy mode and the exact steps to set or change it in this
tool (looked up in its current docs); optional project permission settings
(deny rules for .env etc.) if the owner agreed}}

## Done when

- [ ] {{checkable end state, e.g. "a preview deployment shows the scaffold and CI is green"}}
- [ ] MFA, billing alerts and spending caps are on for every account above
