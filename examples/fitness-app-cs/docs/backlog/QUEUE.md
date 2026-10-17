# Backlog

Priorita: **P0** → **P1** → **P2**
Stav: `ready` | `in_progress` | `blocked` | `done`

Každá fáze končí bránou: vlastník potvrdí, než agent pokračuje. Na každé
bráně (vysoké riziko): kontrola nových závislostí a oprávnění, průchod
`THREAT-MODEL.md`, `OPERATIONS.md` a kritérií úspěchu v `PLAN.md`.

---

## Fáze 0 — Nastavení

**Brána:** účty s MFA a stropy útraty, repozitář a CI zelené, development
build běží na iPhonu i Androidu a čte ze Supabase `dev`, knihovny pro
zdravotní data ověřené na skutečných zařízeních.

| ID | Úkol | Priorita | Stav | Závisí na |
|----|------|----------|------|-----------|
| [T001](tasks/T001-accounts-and-security.md) | Účty, MFA, stropy útraty, repozitář | P0 | ready | — |
| [T002](tasks/T002-scaffold-and-ci.md) | Kostra aplikace Expo, testy, CI zelené | P0 | ready | T001 |
| [T003](tasks/T003-eas-builds.md) | EAS development a preview buildy na telefonech | P0 | ready | T002 |
| [T004](tasks/T004-supabase-projects.md) | Supabase dev a prod v EU, migrace, RLS | P0 | ready | T002 |
| [T005](tasks/T005-health-libraries-spike.md) | Ověřit knihovny HealthKit a Health Connect | P0 | ready | T003 |

---

## Fáze 1 — První verze

**Brána:** hlavní tok (přihlášení → souhlas → import z hodinek a ruční
zápis → historie → předplatné v sandboxu) funguje end-to-end na preview
buildu na obou platformách; testy RLS zelené.

| ID | Úkol | Priorita | Stav | Závisí na |
|----|------|----------|------|-----------|
| [T101](tasks/T101-thin-slice-workout-log.md) | Nejtenčí průřez: přihlášení e-mailem, zápis tréninku, historie | P0 | ready | T004 |
| [T102](tasks/T102-account-management.md) | Sign in with Apple / Google, smazání účtu v aplikaci | P0 | ready | T101 |
| [T103](tasks/T103-health-consent-and-import.md) | Výslovný souhlas a import z Apple Health / Health Connect | P0 | ready | T005, T101 |
| [T104](tasks/T104-subscriptions-revenuecat.md) | Předplatné přes RevenueCat (sandbox) a webhook | P0 | ready | T101 |
| [T105](tasks/T105-privacy-export-store-labels.md) | Zásady soukromí, export dat, štítky soukromí, odlišení | P1 | ready | T103, T104 |
| [T106](tasks/T106-error-reporting.md) | Hlášení chyb (Sentry) bez osobních údajů — ADR | P1 | ready | T003 |
| [T107](tasks/T107-progress-overview.md) | Přehled pokroku | P1 | ready | T101 |

---

## Fáze 2 — Před spuštěním

**Brána:** profesionální revize hotová a nálezy opravené, beta testeři
potvrdili hlavní tok, Supabase `prod` na Pro se zálohou a vyzkoušenou
obnovou, aplikace schválená v obou obchodech.

| ID | Úkol | Priorita | Stav | Závisí na |
|----|------|----------|------|-----------|
| [T201](tasks/T201-professional-review.md) | Bezpečnostní a GDPR revize, DPIA, test obnovy | P0 | ready | Fáze 1 |
| [T202](tasks/T202-beta-testing.md) | Beta: TestFlight a uzavřený test Google Play | P0 | ready | T201 |
| [T203](tasks/T203-store-release.md) | Název, ochranná známka a vydání do obchodů | P0 | ready | T202 |
