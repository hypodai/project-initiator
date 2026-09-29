# Layers

Any software project can be described as a subset of the layers below. There
are no fixed project types — map the idea onto layers, drop the ones it
doesn't need, and research only what remains. Two layers are always present:
**Dev workflow** and **Hosting / runtime** (even "no hosting" is a decision).

For each layer: what it covers, signals that it's needed, criteria that
matter specifically here, and the volatile facts you must verify in this run.

## Client

**Covers:** web front end, mobile (native / cross-platform), desktop,
embedded UI, chat surfaces (Slack/Teams bots), spreadsheet add-ins.
**Signals:** anyone interacts with it through a screen.
**Criteria:** platform coverage vs effort, access to device features,
offline support, performance on low-end devices, how well AI coding tools
handle the framework, SEO needs for web.
**Verify:** current major versions, platform support status, app store
requirements (minimum SDK / OS targets change yearly).

## Backend / API

**Covers:** business logic, APIs, background jobs, scheduled tasks, webhooks.
**Signals:** logic that must not run on the client, integrations, anything
secret.
**Criteria:** builder's language, cold starts vs always-on, long-running
jobs, queue/cron support, local development experience.
**Verify:** runtime limits (timeouts, memory, payload sizes), pricing model.

## Data

**Covers:** primary database, cache, search, analytics store, file/object
storage, vector store.
**Signals:** anything persisted.
**Criteria:** data shape (relational vs document vs time-series), reporting
needs, consistency, backups and point-in-time restore, export / exit path,
region.
**Verify:** free-tier limits, pausing of inactive free projects, storage and
egress pricing, backup retention per plan.

## Identity and access

**Covers:** sign-up/login, social and enterprise SSO, roles, multi-tenancy,
API keys.
**Signals:** users have accounts.
**Criteria:** who the users are (consumer vs B2B), SSO/SAML need and its price
tier, integration with the data layer's permission model, user data export.
**Verify:** MAU pricing, what's paywalled (SSO, MFA, organisations).

## Payments and billing

**Covers:** checkout, subscriptions, usage billing, marketplaces/payouts,
invoicing, tax/VAT.
**Signals:** money moves.
**Criteria:** supported countries, merchant-of-record vs own tax handling,
app-store in-app purchase rules, fees.
**Verify:** fees, country availability, current app store payment rules.

## AI / ML

**Covers:** LLM APIs, embeddings, OCR / document extraction, speech,
vision, local models, agent frameworks, evals.
**Signals:** "AI", "smart", "automatically understands".
**Criteria:** quality for this specific task, price per call at expected
volume, latency, data handling / residency, rate limits, provider
portability (gateway vs direct).
**Verify:** current model line-ups and prices (they change monthly),
deprecation dates, regional availability.

## Realtime and messaging

**Covers:** live updates, presence, chat, collaboration, push notifications,
email, SMS.
**Criteria:** concurrent connections, delivery guarantees, deliverability
(email), per-message costs.
**Verify:** connection and message pricing, sender verification rules.

## Content and admin

**Covers:** CMS, admin panels, back-office tools, internal dashboards.
**Signals:** non-developers edit content or manage records.
**Criteria:** editor experience, roles, localisation, whether a generated
admin is enough.

## Integrations

**Covers:** ERP, CRM, accounting, calendars, wearables/health platforms,
automation platforms (Zapier / Make / n8n), webhooks.
**Criteria:** official API quality, auth method, rate limits, whether an
integration platform beats custom code for this builder.
**Verify:** API availability and pricing, partner-programme requirements.

## Hardware

**Covers:** developer workstation, test devices, peripherals and sensors,
wearables, edge / IoT devices, self-hosted compute and GPUs, end-user device
minimums.
**Signals:** physical devices, local AI, mobile apps (test devices), iOS
builds (macOS), on-premise requirements.
**Criteria:** buy vs rent (cloud builds, device farms, cloud GPUs), power and
connectivity for devices, availability and lead times.
**Verify:** current prices and availability, VRAM needed for the chosen model
size and quantisation.

## Hosting / runtime

**Covers:** where each part runs — PaaS, serverless, containers, VPS, edge,
on-premise, app stores as distribution.
**Criteria:** fit to the backend's runtime needs, region, pricing at the
growth horizon (not just the free tier), operational burden for this
builder, exit path.
**Verify:** pricing, limits, regions, free-tier terms.

## Observability and operations

**Covers:** error tracking, logs, uptime monitoring, analytics, backups,
alerting.
**Criteria:** who responds to incidents, privacy of analytics (cookie
consent in the EU), cost at volume.

## Dev workflow (always present)

**Covers:** repository host, branching, CI/CD, environments (dev / preview /
production), secrets management, AI coding tool and model, testing
approach.
**Criteria:** builder profile — a non-developer needs guard rails (CI,
previews, small tasks), a team needs review flow.
**Verify:** current AI coding tool options and model line-up, CI free minutes.
