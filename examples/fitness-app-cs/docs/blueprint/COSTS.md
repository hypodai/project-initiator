# Náklady

*Ceny zjištěny 2026-10-16 · částky v Kč a USD (kurz 21,732 Kč/USD, ČNB,
vyhlášen 15. 10. 2026 [S20])*

## Předpoklady použití

| Metrika | Vývoj | Spuštění | Růst | Proč |
|---------|-------|----------|------|------|
| Aktivní uživatelé měsíčně (MAU) | 1–10 | 300 | 2 000 | výchozí odpověď v rozhovoru |
| Platící předplatitelé | 0 | 30 | 200 | 10 % konverze `UNVERIFIED estimate` |
| Cena předplatného | — | 99 Kč/měs. | 99 Kč/měs. | mezi Hevy (2,99 $) a Strong (4,99 $) [S1] |
| Tržby měsíčně (hrubé) | 0 | 2 970 Kč (~137 $) | 19 800 Kč (~911 $) | platící × cena |
| Tréninky na uživatele / měsíc | — | 12 | 12 | 3× týdně |
| Úložiště na uživatele | — | ~100 kB | ~100 kB / rok | řádky tréninků a tepu (souhrny, ne surový průběh) `UNVERIFIED estimate` |
| Databáze celkem | < 10 MB | ~30 MB | ~200 MB | MAU × úložiště |
| AI volání | 0 | 0 | 0 | AI není v první verzi |
| E-maily na uživatele / měsíc | ~1 | ~1 | ~1 | potvrzení registrace, reset hesla (v ceně Supabase Auth — limity `UNVERIFIED`) |
| Buildy měsíčně | ~10–20 | ~5 | ~5 | development a preview buildy |

## Měsíční náklady — Balanced (doporučeno)

| Služba | Plán | Vývoj | Spuštění | Růst | Zdroj |
|--------|------|-------|----------|------|-------|
| Expo (framework), knihovny HealthKit / Health Connect, `expo-secure-store` | open source | 0 Kč | 0 Kč | 0 Kč | [S5][S6] |
| Apple HealthKit, Android Health Connect | součást systému | 0 Kč | 0 Kč | 0 Kč | — |
| Supabase `dev` | Free | 0 Kč | 0 Kč | 0 Kč | [S11] |
| Supabase `prod` | Free → Pro | 0 Kč | 543 Kč (25 $) | 543 Kč (25 $) + případné příplatky `UNVERIFIED` | [S11] |
| EAS | Free → Starter | 0 Kč | 0 Kč | 413 Kč (19 $) — OTA pro > 1 000 MAU | [S12] |
| RevenueCat | Free do 2 500 $ MTR | 0 Kč | 0 Kč | 0 Kč | [S10] |
| Apple Developer Program | 99 $/rok, rozpočítáno | 0–179 Kč | 179 Kč | 179 Kč | [S14] |
| Sentry | Free | 0 Kč | 0 Kč | 0 Kč (`UNVERIFIED`) | — |
| GitHub | Free (soukromý repozitář) | 0 Kč | 0 Kč | 0 Kč (`UNVERIFIED`) | — |
| **Celkem fixně** | | **0–179 Kč (0–8 $)** | **~722 Kč (~33 $)** | **~1 135 Kč (~52 $)** | |
| Provize obchodů | 15 % (Small Business Program / předplatné) | — | ~446 Kč | ~2 970 Kč | [S13] |
| **Čistý zbytek z tržeb po fixních nákladech** | | **−179 Kč** | **~1 802 Kč** | **~15 695 Kč** | |

Strop 500 Kč/měsíc je při spuštění překročen o ~222 Kč — kvůli Supabase Pro.
Pro se vyplatí od cca 7 předplatitelů (543 Kč ÷ 84 Kč čistého na
předplatitele po provizi). Do té doby zůstává `prod` na Free.

Claude Code (předplatné, které už máš) není v tabulce — počítá se jako
existující náklad.

## Měsíční náklady — Budget

| Služba | Vývoj | Spuštění | Růst |
|--------|-------|----------|------|
| Supabase (oba projekty Free) | 0 Kč | 0 Kč | 0 Kč |
| EAS Free (OTA jen pro 1 000 MAU, pak aktualizace přes obchody) | 0 Kč | 0 Kč | 0 Kč |
| Apple Developer | 0–179 Kč | 179 Kč | 179 Kč |
| **Celkem** | **0–179 Kč** | **179 Kč (~8 $)** | **179 Kč (~8 $)** |

Riziko: bez záloh, pozastavení po týdnu nečinnosti [S11]. S platícími
uživateli a zdravotními daty nedoporučeno.

## Měsíční náklady — Top

| Služba | Spuštění | Růst |
|--------|----------|------|
| Balanced | ~722 Kč | ~1 135 Kč |
| EAS Starter od začátku | +413 Kč | (už v Balanced) |
| Terra API | +8 671 Kč (399 $) [S8] | +8 671 Kč |
| Obnova k bodu v čase u Supabase | `UNVERIFIED` | `UNVERIFIED` |
| **Celkem** | **~9 806 Kč (~451 $) + PITR** | **~9 806 Kč + PITR** |

## Hranice free tierů

| Služba | Metrika | Hranice | Cena potom | Očekáváno |
|--------|---------|---------|------------|-----------|
| Supabase | nečinnost / zálohy | pauza po 1 týdnu, žádné zálohy | Pro 25 $ ≈ 543 Kč | první platící uživatel |
| Supabase | velikost DB | 500 MB (Free), 8 GB (Pro) | příplatky `UNVERIFIED` | Free ~ při 5 000 uživatelích (odhad) |
| Supabase | MAU | 50 000 (Free), 100 000 (Pro) | — | mimo horizont |
| EAS | OTA aktualizace | 1 000 MAU | Starter 19 $ ≈ 413 Kč | při růstu |
| EAS | buildy | 15 iOS + 15 Android / měs. | 1–4 $ za build navíc (placené plány) | intenzivní vývoj |
| RevenueCat | měsíční tržby | 2 500 $ ≈ 54 330 Kč | 1 % z celých tržeb | mimo horizont |

## Jednorázové náklady

| Položka | Cena | Zdroj |
|---------|------|-------|
| Google Play Console | 25 $ ≈ 543 Kč (jednorázově) | [S15] |
| Apple Developer Program | 99 $ ≈ 2 151 Kč ročně | [S14] |
| Testovací telefon (pokud nemáš iPhone nebo Android) | `UNVERIFIED` | — |
| Doména pro web se zásadami ochrany soukromí (volitelné) | `UNVERIFIED` | — |
| **Volitelné, doporučené:** revize GDPR (DPIA) a bezpečnosti před placeným spuštěním | `UNVERIFIED` — zjistit poptávkou v T201 | — |

## Kontrola útraty

| Služba | Upozornění | Tvrdý strop | Nastavit |
|--------|-----------|-------------|----------|
| Supabase | `UNVERIFIED` | ano — spending cap na Pro zapnutý ve výchozím stavu [S11] | nechat zapnutý |
| EAS | `UNVERIFIED` | Free plán nemá platby navíc [S12]; na Starter `UNVERIFIED` | limit útraty, pokud je k dispozici |
| RevenueCat | — | nepoužívá placení předem | — |
| Sentry | `UNVERIFIED` | Free plán | zůstat na Free |
| Apple / Google | — | fixní poplatky | — |

Služby bez ověřeného stropu jsou riziko R7 v [RISKS.md](RISKS.md).

## Jak náklady snížit

- `prod` na Supabase Free až do ~7 předplatitelů; potom Pro.
- OTA aktualizace omezit, aby EAS zůstal na Free; vydávat přes obchody.
- Ukládat ze zdravotních dat jen souhrny tréninku (průměrný / max. tep,
  délka), ne každou sekundu — drží DB hluboko pod 500 MB a zmenšuje rozsah
  citlivých dat.
- Roční předplatné se slevou zlepší cash-flow a sníží provizi po 1. roce
  (15 % [S13]).
