# Fitness appka (pracovní název)

Mobilní aplikace pro iPhone i Android, do které si rekreační sportovci
zapisují tréninky — silové ručně, tep a kardio automaticky z chytrých
hodinek přes Apple Health / Health Connect — a sledují svůj pokrok.
Prodává se jako měsíční / roční předplatné v App Store a Google Play.

**Stav:** plánování — založeno z blueprintu project-initiator 2026-10-16.

## Stack

- Aplikace: Expo (React Native, TypeScript)
- Data z hodinek: Apple HealthKit + Android Health Connect
- Předplatné: nákupy v obchodech přes RevenueCat
- Backend, databáze, přihlášení: Supabase (Postgres + RLS, Auth, Edge Functions), region EU
- Buildy a distribuce: EAS, App Store, Google Play
- Hlášení chyb: Sentry (potvrdit v T106)
- Vývoj: GitHub, GitHub Actions, Dependabot, Claude Code

Proč právě tohle: [`docs/blueprint/DECISIONS.md`](docs/blueprint/DECISIONS.md).

## Jak začít

Postupuj podle [`docs/blueprint/SETUP.md`](docs/blueprint/SETUP.md), pak
pracuj podle backlogu v [`docs/backlog/QUEUE.md`](docs/backlog/QUEUE.md).

## Práce s AI agentem

Otevři repozitář v Claude Code (nebo jiném agentovi) a začni prvním úkolem,
např. „Začni úkolem T001“. Pravidla agenta jsou v [`AGENTS.md`](AGENTS.md).

---

© 2026 vlastník projektu. Všechna práva vyhrazena.
