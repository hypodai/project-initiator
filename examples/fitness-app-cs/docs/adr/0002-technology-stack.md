# ADR-0002: Technologický stack

- **Stav:** přijato
- **Datum:** 2026-10-16

## Kontext

Mobilní aplikace (iPhone + Android) na záznam tréninků s daty z chytrých
hodinek, prodávaná jako předplatné v ČR/SK. Staví ji neprogramátor pomocí
Claude Code na Windows notebooku. Priority: 1. nízká cena (strop ~500 Kč
měsíčně), 2. rychlost spuštění, 3. jednoduchost / málo dodavatelů. Data
zahrnují zdravotní údaje (GDPR čl. 9), proto data v EU a vysoká úroveň
rizika.

## Rozhodnutí

- Aplikace: Expo (React Native, TypeScript) — AI nástroje TypeScript znají
  nejlépe, iOS buildy v cloudu bez Macu
- Data z hodinek: HealthKit + Health Connect v telefonu — zdarma, pokryje
  Apple Watch, Garmin, Samsung přes synchronizaci výrobců
- Předplatné: nákupy v obchodech přes RevenueCat — zdarma do 2 500 $ MTR,
  bez vlastního ověřování účtenek
- Backend, databáze: Supabase (Postgres + RLS, Edge Functions), region EU —
  relační data, ochrana zdravotních dat v databázi, snadný odchod
- Přihlášení: Supabase Auth — v ceně backendu, napájí RLS
- Buildy a distribuce: EAS + App Store + Google Play — 15 + 15 buildů zdarma
- Hlášení chyb: Sentry Free — předběžně, potvrdí ADR v T106
- Vývoj: GitHub, GitHub Actions, Dependabot, Claude Code

Zvolená varianta: **Balanced**. Úplné srovnání, zamítnuté alternativy
(Flutter, nativní vývoj, Terra, Sahha, Open Wearables, Adapty, Firebase,
Convex, Clerk) a zdroje: [`docs/blueprint/DECISIONS.md`](../blueprint/DECISIONS.md).

## Důsledky

- **Náklady:** vývoj 0–179 Kč/měs., spuštění ~722 Kč/měs., růst ~1 135 Kč/měs.
  + 15 % provize obchodů — [COSTS.md](../blueprint/COSTS.md)
- **Lock-in:** nejtěžší odchod od Supabase Auth (účty uživatelů)
  a RevenueCat; data v Postgresu jsou přenositelná
- **Rizika:** zdravotní údaje, schvalování v obchodech, silná levná
  konkurence, neúplná data z Garminu — [RISKS.md](../blueprint/RISKS.md)
- **Přehodnotit, když:** testeři chtějí plná data z Garminu (Terra / API
  Garmin), tržby přesáhnou 2 500 $/měs., databáze přesáhne 8 GB, nebo bude
  potřeba aplikace přímo na hodinkách
