# Setup

Follow in order. Each step says what it produces, so a person or an agent
can check it is done.

## 1. Accounts

| Service | Plan to start on | Why | Link |
|---------|------------------|-----|------|

## 2. Repository

{{git + GitHub steps; if gh wasn't available: create a private repo manually,
then `git remote add origin …` and `git push -u origin main`}}

- Protect `main`: require CI to pass before merging.
- Secrets never in the repo — {{where secrets live for this stack}}.

## 3. Scaffold

```bash
{{exact official generator command with options matching DECISIONS.md}}
```

Reference template (if found): {{link}}

## 4. Services

{{per service: create project, region, keys → where they go, connection check}}

## 5. CI and environments

{{enable .github/workflows/ci.yml steps, preview and production environments}}

## 6. AI coding setup

{{coding agent and model, how to start: open the repo and run the first task}}

## Done when

- [ ] {{checkable end state, e.g. "a preview deployment shows the scaffold and CI is green"}}
