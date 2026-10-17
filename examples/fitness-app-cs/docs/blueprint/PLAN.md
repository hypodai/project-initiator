# Plán — Fitness appka (pracovní název)

*Blueprint vytvořen 2026-10-16 · hloubka rešerše: quick*

## Nápad

Mobilní aplikace pro iPhone i Android, do které si rekreační sportovci
zapisují tréninky (silové v posilovně ručně, kardio a tep automaticky
z chytrých hodinek přes Apple Health / Health Connect) a vidí svůj pokrok.
Prodává se jako měsíční / roční předplatné v App Store a Google Play.
Staví ji neprogramátor pomocí Claude Code na Windows notebooku, s provozem za
pár stovek korun měsíčně.

**Účel:** komerční produkt · **Verdikt trhu:** hybrid (stavět vlastní
aplikaci, komoditní části koupit jako služby) — viz [LANDSCAPE.md](LANDSCAPE.md)

## První verze

**Obsahuje:**
- registrace a přihlášení, smazání účtu přímo v aplikaci
- ruční záznam tréninku (cviky, série, opakování, váha) a historie
- výslovný souhlas se zpracováním zdravotních údajů, pak import tréninků
  a tepu z Apple Health (iOS) a Health Connect (Android)
- jednoduchý přehled pokroku (objem, počet tréninků za týden)
- předplatné (měsíční a roční) s bezplatnou verzí s omezením
- export a smazání vlastních dat, zásady ochrany soukromí

**Mimo první verzi (později nebo nikdy):**
- přímé napojení na cloud Garminu, Polaru apod. — bez partnerské smlouvy
  a serveru navíc to nejde levně; data přicházejí přes Apple Health / Health
  Connect (viz omezení v [RISKS.md](RISKS.md))
- AI trenér, generování plánů — zvyšuje náklady i regulatorní riziko
- sociální funkce, sdílení, žebříčky — uživatelský obsah = moderace
- webová verze, aplikace přímo na hodinkách
- push notifikace (přidat ve fázi 2, pokud se ukážou potřeba)

## Kritéria úspěchu

*Předpoklady — uprav, pokud to vidíš jinak.*

- Do 3 měsíců od spuštění v obchodech **alespoň 30 platících předplatitelů**
  (to pokryje provoz ~7×, viz [COSTS.md](COSTS.md)).
- **40 % uživatelů zapíše trénink i ve 4. týdnu** po instalaci.
- Import z hodinek funguje u testerů s Apple Watch a s Garminem bez ručního
  zásahu.

## Nefunkční požadavky

| Oblast | Cíl |
|--------|-----|
| Dostupnost | backend 99 % měsíčně (dáno Supabase Pro; bez vlastní pohotovosti) |
| Výkon | uložení tréninku < 1 s na běžném telefonu; zápis funguje i bez signálu a synchronizuje se později (cíl fáze 1, ne podmínka T101) |
| Zařízení / OS | iOS 16.4+ a Android 8+ (API 26+) — minimum knihoven pro zdravotní data [S5]; aktuální minimální SDK obchodů ověřit v T002 (`UNVERIFIED`) |
| Přístupnost | čitelné písmo při zvětšení systémového textu, popisky pro čtečky obrazovky u hlavních tlačítek |
| Uchování a zálohy | denní zálohy 7 dní (Supabase Pro [S11]); smazání účtu = smazání dat do 30 dnů |

## Doporučený stack (Balanced)

| Vrstva | Volba | Důvod v jedné větě |
|--------|-------|--------------------|
| Mobilní aplikace | Expo (React Native, TypeScript) | jeden kód pro obě platformy, AI nástroje TypeScript znají nejlépe, iOS buildy v cloudu bez Macu [S3][S12] |
| Data z hodinek | Apple HealthKit + Android Health Connect přímo v telefonu | zdarma, pokrývá Apple Watch, Garmin, Samsung a další přes jejich synchronizaci [S6][S17][S18] |
| Předplatné | nákupy v App Store / Google Play přes RevenueCat | zdarma do 2 500 $ měsíčních tržeb, hlídá nároky a obnovy za tebe [S10] |
| Backend, databáze, přihlášení | Supabase (Postgres, Auth, Edge Functions, region EU) | jeden dodavatel pro data, účty i serverovou logiku, relační data, snadný odchod (Postgres) [S11][S19] |
| Buildy a distribuce | EAS Build / Submit / Update (Free) + App Store + Google Play | cloudové buildy pro iOS z Windows, 15 + 15 buildů měsíčně zdarma [S12] |
| Hlášení chyb | Sentry (Free) | potvrdí se v T106 (`UNVERIFIED`) |
| Vývoj | GitHub (soukromý repozitář), GitHub Actions, Dependabot, Claude Code | CI jako hlídač kódu, který nikdo nečte řádek po řádku |

Varianty a celé srovnání: [DECISIONS.md](DECISIONS.md).

## Náklady v kostce

*Kurz 21,732 Kč/USD (ČNB, 15. 10. 2026) [S20]*

| | Vývoj | Spuštění (300 aktivních, 30 platících) | Růst (2 000 aktivních, 200 platících) |
|---|---|---|---|
| Měsíčně (fixní) | 0–179 Kč (0–8 $) | ~722 Kč (~33 $) | ~1 135 Kč (~52 $) |
| Provize obchodů | — | 15 % z tržeb | 15 % z tržeb |
| Jednorázově | Google Play 543 Kč (25 $); Apple 2 151 Kč/rok (99 $) | | |

Při spuštění je fixní provoz **nad předpokládaným stropem 500 Kč/měsíc**
(kvůli Supabase Pro). Pokryje ho zhruba 7 předplatitelů po 99 Kč. Do prvních
platících uživatelů stačí Supabase Free a jsi na 0–179 Kč.

Podrobnosti: [COSTS.md](COSTS.md). Hardware: [HARDWARE.md](HARDWARE.md).

## Bezpečnostní klasifikace

**Data:** osobní údaje + zvláštní kategorie (zdravotní: tep, tréninky);
platební údaje ne — platby řeší obchody · **Expozice:** veřejný internet
(aplikace v obchodech, API Supabase) · **Dosah útoku:** uniklý `service_role`
klíč nebo chyba v RLS = zdravotní data všech uživatelů; ukradený účet
Apple/Google vývojáře = vydání podvržené aplikace · **Úroveň rizika:
vysoká** (zdravotní údaje + peníze) — viz `SECURITY.md` a
[THREAT-MODEL.md](THREAT-MODEL.md).

## Hlavní rizika

1. Zdravotní údaje podle GDPR (čl. 9) zpracovává kód napsaný AI — výslovný
   souhlas, DPIA, region EU, testy RLS a **profesionální revize před
   spuštěním placené verze** — viz [RISKS.md](RISKS.md)
2. Schvalování v obchodech (HealthKit, Health Connect, předplatné, smazání
   účtu, štítky soukromí) může spuštění zdržet o týdny
3. Přeplněný trh s levnou silnou konkurencí (Hevy 2,99 $/měs., Strong
   4,99 $/měs. [S1]) — zatím bez jasného odlišení; data z Garminu jsou přes
   Apple Health neúplná [S17]

## Priority použité pro váhy

1. nízká cena → Cena ×3
2. rychlost spuštění → Rychlost ×2
3. jednoduchost / málo dodavatelů → Integrace ×1,5 (a dál se promítá do
   Rychlosti, která už má váhu 2)
Ostatní kritéria ×1.

## Uvedené preference

Žádné (odpověď „ok“ na výchozí „žádné preference“). Daný je jen Claude Code
jako nástroj a Windows notebook jako počítač — obojí stack respektuje
(iOS buildy v cloudu přes EAS).

## Nastavení projektu

| Nastavení | Volba | Poznámka |
|-----------|-------|----------|
| Docs profil | complete | placený produkt se zdravotními daty; dražší na tokeny, dokumentace nezastará |
| Režim autonomie | úroveň 2 — semi-auto | nastavuje vlastník v Claude Code |
| Git workflow | B — větev na úkol + PR | po vytvoření GitHub repozitáře; do té doby lokální větve a merge |
| Repozitář | soukromý (zatím jen lokální git) · bez licence, © vlastník | |
| Jazyk dokumentace | čeština | konverzace: čeština; kód, názvy souborů a commity anglicky |

## Předpoklady

| Téma | Předpokládaná hodnota | Proč | Kdy znovu zvážit |
|------|----------------------|------|------------------|
| Měsíční strop | 500 Kč | „pár stovek korun“ | jakmile jsou tržby |
| Jednorázový rozpočet | do ~3 000 Kč + případný testovací telefon | poplatky obchodů | před T001 |
| Cena předplatného | 99 Kč/měs., 790 Kč/rok | mezi Hevy a Strong [S1] | po beta testu |
| Uživatelé | 300 aktivních / 30 platících po spuštění; 2 000 / 200 za rok | výchozí odpověď v rozhovoru | po 3 měsících provozu |
| Trh a data | ČR/SK, data v EU, bez uživatelů pod 16 let | výchozí odpověď | expanze mimo EU |
| Hodinky | jen přes Apple Health / Health Connect | rozpočet vylučuje agregátory | když testeři s Garminem chtějí plná data |
| Status vývojáře | fyzická osoba (OSVČ) v ČR, Small Business Program Apple (15 % [S13]) | malé tržby; limit programu `UNVERIFIED` | daně a DPH ověřit s účetní (`UNVERIFIED`) |
| Offline | zápis bez signálu je žádoucí, ne podmínka | výchozí odpověď | po zpětné vazbě testerů |

## Další krok

Otevři projekt v Claude Code a začni úkolem **T001**.
