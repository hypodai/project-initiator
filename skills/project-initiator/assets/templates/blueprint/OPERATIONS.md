# Operations — {{project_name}}

*Used from the first deployment on; `complete` docs profile.*

## Environments

| Environment | URL | Deploys from | Data | Keys |
|-------------|-----|--------------|------|------|

## Deploying and rolling back

- Deploy: {{steps or "automatic from main after CI"}}
- Verify after deploy: {{smoke checks}}
- Roll back: {{exact steps, and what cannot be rolled back — e.g. a migration}}

## Release checklist

- [ ] CI green; `CHANGELOG.md` `[Unreleased]` moved under the new version
- [ ] Version tag ({{SemVer}})
- [ ] Deploy, verify, announce

## Backups and restore

| Data | Backup | Frequency | Retention | Last restore test |
|------|--------|-----------|-----------|-------------------|

## Monitoring and alerts

| What | Tool | Alert goes to | Threshold |
|------|------|---------------|-----------|
| Errors | | | |
| Uptime | | | |
| Cost / spending cap | | | |

## Incidents

Who is on point: {{name or role}}. First steps: stop the damage (revoke keys,
disable the feature), tell affected people, restore, write down what
happened. Details of key revocation: `SECURITY.md`.

## Routine upkeep

| Task | How often | Who |
|------|-----------|-----|
| Review dependency update PRs | weekly | |
| Rotate keys | {{interval}} | |
| Test a restore | {{interval}} | |
| Re-check pricing and limits of paid services | every 6 months | |
