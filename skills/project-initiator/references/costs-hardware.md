# Costs and hardware

## Costs

People underestimate running costs in two places: the moment a free tier
ends, and usage-based items (AI tokens, SMS, bandwidth) that scale with
success. The cost model exists to show both before they happen.

### Horizons

- **Build** — until the first real users.
- **Launch** — the user count from the interview (or the stated assumption).
- **Growth** — the one-year target, or 10× launch if none was given.

### Categories

- **One-off:** developer accounts (app stores), domains, hardware,
  certificates, paid templates.
- **Recurring fixed:** plans and seats.
- **Usage-based:** compute, database, storage, bandwidth/egress, AI tokens,
  emails, SMS, push, maps, transcription.
- **Optional people cost:** professional review before production, if
  `RISKS.md` recommends it; for high-risk projects (`security.md`) a security
  review or penetration test.

### Method

1. Write the usage assumptions as a table first: monthly active users,
   sessions per user, requests per session, storage per user, AI calls per
   user and tokens per call, emails per user. Everything else is computed
   from it, so a reader can change one assumption and follow the effect.
2. Price each component from its pricing page fetched in this run.
3. Mark **free-tier cliffs**: the metric and threshold at which the bill
   jumps, and the new amount.
4. Compute each horizon for each variant.
5. Show amounts in the user's currency and in USD, with the exchange-rate
   date.

Anything not backed by a source fetched today is labelled `UNVERIFIED
estimate`. Never round an unknown into a confident-looking number.

### Cost guards

For every usage-billed service, record whether it supports billing alerts and
a hard cap (verify on its pricing / billing docs) and the amount to set — a
leaked key or a runaway loop is the typical way a small project gets a large
bill. Services without a cap are a risk to note in `RISKS.md`. The setup task
(`SETUP.md`) enables them before anything else is created.

### AI costs

Estimate per feature: calls per user per month × (input tokens × input price
+ output tokens × output price). Note cheaper models that would do for the
task, and caching or batching discounts if the provider offers them.

## Hardware

Hardware only appears when it matters, but check each item:

- **Developer workstation** — OS constraints (iOS builds need macOS or a
  cloud build service), RAM for emulators, containers or local models, disk.
- **Test devices** — the minimum OS versions and at least one low-end device
  for mobile; device farms as a rental alternative.
- **Peripherals and sensors** — wearables, BLE devices, cameras, scanners,
  POS terminals: compatibility with the chosen platform APIs, price,
  availability.
- **Edge / IoT** — boards, power, enclosures, connectivity (Wi-Fi, LTE,
  LoRa), over-the-air update path.
- **Self-hosted compute** — CPU/RAM for services; for local AI, VRAM needed
  for the model size and quantisation (verify current figures), versus renting
  cloud GPUs.
- **End users** — minimum device requirements the product imposes.

For every item decide buy vs rent vs not needed, with price and source.

## Output

- `COSTS.md` — assumptions table, per-component table per variant and
  horizon, free-tier cliffs, one-off costs, sources.
- `HARDWARE.md` — what to buy or rent, why, price, alternatives. If nothing
  is needed beyond an ordinary computer, say so in one line.
