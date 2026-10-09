# Threat model — {{project_name}}

*Lite model, created {{date}}. Revisit at every phase gate and when an entry
point or a data category is added. Risk level: {{low / medium / high}}.*

## Assets

| Asset | Why it matters |
|-------|----------------|
| {{user data / API keys / money flow / availability / reputation}} | |

## Actors

| Actor | Capability and motivation |
|-------|---------------------------|
| Anonymous visitor / bot | |
| Logged-in user abusing access | |
| Leaked or stolen credential | |
| Compromised dependency or CI | |
| Prompt-injected agent *(if AI is involved)* | |

## Entry points

{{routes, forms, uploads, webhooks, admin pages, integration credentials, CI,
the owner's own accounts}}

## Top threats

| # | Threat | Likelihood | Impact | Mitigation (and where it lives) | Residual |
|---|--------|-----------|--------|----------------------------------|----------|

## Accepted risks

{{what remains, who accepted it, until when}}
