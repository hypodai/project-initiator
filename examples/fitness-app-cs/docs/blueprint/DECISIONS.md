# Rozhodnutí

*Váhy: Fit (pokrytí potřeb) ×1 · Cena ×3 · Rychlost (čas do spuštění pro
tohoto stavitele) ×2 · Vyzrálost ×1 · Integrace ×1,5 · Odchod (exit) ×1 ·
Přívětivost pro AI nástroje ×1 · u backendu navíc Soulad (compliance) ×1.
Skóre 1–5, „Vážené“ = součet skóre × váha. Zdroje v [SOURCES.md](SOURCES.md).*

*Hloubka quick: plně rešeršovány vrstvy aplikace, data z hodinek, předplatné
a backend. U ostatních vrstev jsou skóre bez vlastního zdroje označena
`UNVERIFIED`.*

## Varianty

| Vrstva | Balanced (doporučeno) | Top | Budget |
|--------|------------------------|-----|--------|
| Mobilní aplikace | Expo | Expo | Expo |
| Data z hodinek | HealthKit + Health Connect | + Terra API (500+ zařízení, plná data z Garminu) | HealthKit + Health Connect |
| Předplatné | RevenueCat + nákupy v obchodech | stejné | stejné |
| Backend, databáze | Supabase Free → Pro od prvních platících | Supabase Pro od začátku + obnova k bodu v čase (cena `UNVERIFIED`) | Supabase Free i po spuštění (bez záloh) |
| Přihlášení | Supabase Auth | stejné | stejné |
| Buildy a distribuce | EAS Free → Starter při růstu | EAS Starter od začátku | EAS Free; nad 1k MAU aktualizace jen přes obchody |
| Hlášení chyb | Sentry Free | Sentry placený plán (`UNVERIFIED`) | jen hlášení pádů v konzolích obchodů |
| Vývoj | GitHub + Actions + Claude Code | stejné | stejné |

**Kdy jinou variantu:** *Budget* jen do prvních platících uživatelů — Free
plán Supabase nemá zálohy a po týdnu nečinnosti se pozastaví [S11], což
u placené aplikace se zdravotními daty není přijatelné. *Top* dává smysl,
až testeři s Garminem / Polarem budou chtít plná data z tréninků (GPS,
průběh tepu) a tržby unesou ~8 700 Kč měsíčně za Terra [S8].

---

## Mobilní aplikace

**Rozhodnutí:** Expo (React Native, TypeScript)
**Prior před rešerší:** Expo — potvrzen; rozhodla přívětivost pro AI nástroje
a cloudové iOS buildy z Windows.

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|----|--------|
| Expo (React Native) | 5 | 5 | 5 | 4 | 5 | 4 | 5 | **50,5** |
| Flutter | 4 | 4 | 3 | 5 | 4 | 4 | 3 | 40 |
| Nativně (Swift + Kotlin) | 5 | 2 | 1 | 5 | 4 | 4 | 3 | 31 |

**Proč vyhrál:** TypeScript má v AI modelech nejvíc trénovacích dat, takže
Claude Code v něm píše spolehlivější kód než v Dartu [S3][S4]. Pro HealthKit
i Health Connect existují knihovny s Expo pluginem [S5][S6]. EAS staví iOS
verzi v cloudu, takže Mac není potřeba, a Free plán má 15 iOS + 15 Android
buildů měsíčně [S12].
**Proč ne ostatní:**
- Flutter — rychlejší start a UI na míru [S3], ale Dart = méně kvalitní
  výstup AI nástrojů; iOS build bez Macu přes další cloudovou službu
  (`UNVERIFIED`).
- Nativně — dva kódy ve dvou jazycích a Swift vyžaduje Mac; pro
  neprogramátora s rozpočtem v stovkách korun nereálné.

**Vyřazeno tvrdými požadavky:** —
**Kontrola výchozí volby (default-bias):** Expo i Flutter jsou na seznamu
reflexních voleb. Expo porazilo dvě rešeršované alternativy (Flutter, nativní
vývoj) s odstupem 10,5 a 19,5 bodu. *Proč to není jen výchozí volba:* tady
rozhodují dvě konkrétní věci — stavitel neumí programovat a staví na Windows.
Expo je jediná varianta, kde AI píše v nejznámějším jazyce **a** iOS build
nepotřebuje Mac.
**Kdy znovu zvážit:** když bude potřeba aplikace přímo na hodinkách (watchOS /
Wear OS), nebo když výkon animací přestane stačit.

## Data z hodinek

**Rozhodnutí:** Apple HealthKit (iOS) + Health Connect (Android) přímo
v telefonu. Knihovny: `react-native-health-connect` (Android, v4.1.3 s Expo
pluginem [S6]) a pro iOS `@kingstinct/react-native-healthkit`
(`UNVERIFIED` — npm stránka v tomto běhu vrátila 403; vyzrálost ověřit
v T005). Záloha: `expo-healthkit` od appeeky (jedno API pro obě platformy,
v0.4.1 z 2026-10-01 — *rising*, mladý projekt [S5]).
**Prior před rešerší:** totéž (HealthKit + Health Connect) — potvrzen; agregátory
vypadly kvůli ceně.

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|----|--------|
| HealthKit + Health Connect v telefonu | 4 | 5 | 4 | 3 | 5 | 5 | 3 | **45,5** |
| Open Wearables (self-hosted) | 4 | 3 | 1 | 2 | 3 | 5 | 2 | 28,5 |
| Terra API | 5 | 1 | 4 | 4 | 4 | 3 | 3 | vyřazeno |
| Sahha | 3 | 1 | 4 | 3 | 4 | 3 | 3 | vyřazeno |

**Proč vyhrál:** bez měsíčního poplatku a bez serveru navíc. Apple Watch
zapisuje přímo do Apple Health; Garmin Connect posílá do Apple Health kroky,
tep, spánek i tréninky [S17] a do Health Connect kroky, tep, spánek a váhu
[S18]. Jedno napojení tak pokryje většinu hodinek.
**Proč ne ostatní:**
- Open Wearables — open-source a zdarma [S9], ale vyžaduje provozovat vlastní
  server, a to neprogramátor nemá kdo spravovat.
- Přímé API Garmin Connect — vyžaduje partnerský program a vlastní backend
  (`UNVERIFIED`, nerešeršováno — quick); kandidát pro variantu Top později.

**Vyřazeno tvrdými požadavky:** Terra (od 399 $ ≈ 8 671 Kč/měs. [S8]),
Sahha (299 $ ≈ 6 498 Kč/měs. [S8]), ROOK (399 $/měs. [S8]) — všechny
mnohonásobně nad stropem 500 Kč/měs.
**Kontrola výchozí volby:** volba není na seznamu reflexních voleb.
**Kdy znovu zvážit:** když testeři s Garminem budou chtít průběh tepu a GPS
z tréninků — přes Apple Health jde u tréninků jen minimum a maximum tepu
[S17].

## Předplatné

**Rozhodnutí:** nákupy v aplikaci (App Store / Google Play) spravované přes
RevenueCat.
**Prior před rešerší:** RevenueCat — potvrzen.

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|----|--------|
| RevenueCat | 5 | 5 | 5 | 5 | 4 | 3 | 4 | **48** |
| Adapty | 5 | 5 | 4 | 4 | 4 | 3 | 3 | 44 |
| Přímo StoreKit / Play Billing (vlastní ověřování) | 4 | 5 | 2 | 3 | 3 | 5 | 2 | 37,5 |

**Proč vyhrál:** zdarma do 2 500 $ měsíčních tržeb, potom 1 % [S10]; očekávané
tržby po roce ~911 $ jsou hluboko pod hranicí. Odpadá vlastní ověřování
účtenek na serveru — nejrizikovější část placení. Vyzrálost a kvalita Expo
SDK jsou `UNVERIFIED` (z paměti: dlouho používaná služba), ověřit v T104.
**Proč ne ostatní:**
- Adapty — zdarma až do 5 000 $ [S10], ale silné hlavně v paywall
  experimentech, které první verze nepotřebuje; méně příkladů pro AI
  (`UNVERIFIED`). Rozumná náhrada, kdyby RevenueCat zdražil.
- Vlastní ověřování — zdarma, ale serverovou logiku obnov, refundací
  a podvodů by psala AI bez revize; zbytečné riziko.
- Stripe / webová platba mimo obchod — v EU od 1. 10. 2026 možné, ale
  i tak se platí provize Apple [S13] a k tomu vlastní DPH a fakturace; pro
  neprogramátora nepoměr.

**Vyřazeno tvrdými požadavky:** —
**Kontrola výchozí volby:** RevenueCat na seznamu reflexních voleb není
(Stripe ano a nebyl zvolen).
**Kdy znovu zvážit:** tržby přes 2 500 $/měs. (pak porovnat 1 % s Adapty) nebo
změna pravidel obchodů pro platby mimo aplikaci.

## Backend a databáze

**Rozhodnutí:** Supabase (Postgres + Edge Functions + Storage), region EU.
Free při vývoji a betě, **Pro (25 $ ≈ 543 Kč/měs.) od prvního platícího
uživatele**.
**Prior před rešerší:** Supabase — potvrzen, ale s tvrdým varováním
o ceně Pro vůči rozpočtu.

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | Soulad | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|--------|----|--------|
| Supabase | 5 | 3 | 5 | 4 | 5 | 5 | 4 | 5 | **49,5** |
| Firebase | 4 | 4 | 4 | 5 | 4 | 2 | 4 | 4 | 45 |
| Convex | 4 | 4 | 4 | 3 | 4 | 3 | 3 | 4 | 43 |

**Proč vyhrál:** tréninky jsou relační data (trénink → cviky → série) a
přehledy pokroku jsou dotazy nad nimi — to sedí na Postgres. Řádková
bezpečnost (RLS) hlídá, že každý vidí jen svá zdravotní data, přímo
v databázi. `supabase-js` funguje v Expo bez nativních modulů [S19]. Jeden
dodavatel pokryje data, přihlášení i serverovou funkci pro webhooky
RevenueCat (priorita 3). Odchod je snadný — standardní Postgres. Free:
500 MB, 50 000 MAU; Pro: 8 GB, 100 000 MAU, 7 dní záloh, spending cap
zapnutý ve výchozím stavu [S11].
**Proč ne ostatní:**
- Firebase — při malém provozu levný (platba za použití), ale dokumentová
  databáze je horší na přehledy a odchod z Firestore je těžký; React Native
  Firebase potřebuje nativní build [S19]; tvrdý strop útraty `UNVERIFIED`
  (z paměti jen upozornění na rozpočet).
- Convex — reaktivní TypeScript backend [S19], ale mladší (*rising*),
  cena free tieru a region EU v tomto běhu neověřeny (`UNVERIFIED`).

**Vyřazeno tvrdými požadavky:** —
**Kontrola výchozí volby:** Supabase i Firebase jsou reflexní volby. Supabase
porazil Firebase (o 4,5) a Convex (o 6,5). *Proč to není jen výchozí
volba:* u zdravotních dat jednoho uživatele je ochrana na úrovni řádků
v databázi (RLS) konkrétní výhoda — jedna politika chrání data i před
chybou v kódu aplikace, kterou AI napíše a nikdo nezkontroluje. A relační
model sedí na přehledy pokroku.
**Kdy znovu zvážit:** přes 8 GB dat nebo potřeba výkonnějšího výpočtu (příplatky
nad Pro `UNVERIFIED`); pokud Supabase změní podmínky Free/Pro.

## Přihlášení

**Rozhodnutí:** Supabase Auth (e-mail + Sign in with Apple + Google).
**Prior před rešerší:** Supabase Auth — potvrzen (lehké posouzení, quick).

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|----|--------|
| Supabase Auth | 5 | 5 | 5 | 4 | 5 | 4 | 4 | **49,5** |
| Firebase Auth | 4 | 5 | 4 | 5 | 2 | 3 | 4 | 42 |
| Clerk | 4 | 4 | 4 | 4 | 3 | 3 | 4 | 39,5 |

**Proč vyhrál:** je v ceně Supabase (50 000 MAU na Free [S11]) a jeho
identita přímo napájí RLS politiky — žádný další dodavatel.
**Proč ne ostatní:** Firebase Auth i Clerk = druhý dodavatel a napojení cizích
tokenů na RLS (`UNVERIFIED`, nerešeršováno — quick).
**Kontrola výchozí volby:** Supabase Auth, Clerk i Firebase jsou reflexní;
Supabase Auth vede o 7,5 a 10 bodů, ale skóre alternativ jsou z velké části
`UNVERIFIED`. *Proč to není jen výchozí volba:* menší počet dodavatelů je
priorita uživatele č. 3 a přihlášení je součástí už zvoleného backendu.
**Kdy znovu zvážit:** pokud by šlo o B2B prodej firmám (SSO).
Pozn.: Apple vyžaduje Sign in with Apple, pokud aplikace nabízí jiné sociální
přihlášení (`UNVERIFIED`, ověřit v T101).

## Buildy a distribuce

**Rozhodnutí:** EAS Build + Submit + Update (Free; Starter 19 $ ≈ 413 Kč/měs.
až nad 1 000 MAU pro OTA aktualizace [S12]), distribuce přes App Store
a Google Play.
**Prior před rešerší:** EAS — potvrzen.

| Kandidát | Fit | Cena | Rychlost | Vyzrálost | Integr. | Exit | AI | Vážené |
|----------|-----|------|----------|-----------|---------|------|----|--------|
| EAS | 5 | 5 | 5 | 4 | 5 | 4 | 4 | **49,5** |
| Jiná cloudová CI pro mobil (např. Codemagic) | 4 | 4 | 3 | 4 | 3 | 4 | 3 | 37,5 |

**Proč vyhrál:** oficiální cesta pro Expo, iOS build bez Macu, 15 + 15 buildů
zdarma, OTA aktualizace pro 1 000 MAU zdarma [S12].
**Proč ne ostatní:** Codemagic apod. — další dodavatel a ruční konfigurace
(skóre `UNVERIFIED`, nerešeršováno).
**Vyřazeno tvrdými požadavky:** lokální build na Windows — iOS bez Macu
postavit nejde.
**Kontrola výchozí volby:** EAS není na seznamu reflexních voleb.
**Kdy znovu zvážit:** když nebude stačit 15 buildů měsíčně nebo fronta Free
plánu bude zdržovat (build navíc 1–4 $ [S12]).

## Hlášení chyb

**Rozhodnutí:** Sentry (Free) — **předběžné**, potvrdí se ADR v T106
(`UNVERIFIED`: cena, limity, region EU a DPA v tomto běhu neověřeny).
**Prior před rešerší:** Sentry — nerešeršováno (quick).
**Proč ne ostatní:** Firebase Crashlytics — další účet a SDK Googlu
(`UNVERIFIED`); jen konzole App Store Connect / Google Play — zdarma, ale
bez chyb, které aplikaci neshodí, a bez chyb z Edge Functions.
**Kontrola výchozí volby:** není na seznamu reflexních voleb.
**Kdy znovu zvážit:** v T106 podle aktuálního ceníku a regionu dat.

## Vývoj (dev workflow)

**Rozhodnutí:** GitHub (soukromý repozitář), GitHub Actions (lint, typecheck,
testy, audit závislostí, gitleaks), Dependabot, Claude Code, workflow B
(větev na úkol + PR).
**Prior před rešerší:** totéž — potvrzeno (nerešeršováno, quick).
**Proč ne ostatní:** GitLab — funkčně srovnatelný, ale Claude Code a Dependabot
jsou s GitHubem nejvíc vyzkoušené (`UNVERIFIED`); bez CI — nepřijatelné,
když kód nikdo nečte řádek po řádku.
**Kdy znovu zvážit:** volné minuty Actions a ochrana větve `main`
u soukromého repozitáře na Free plánu — `UNVERIFIED`, ověřit v T001.

## Bezpečnost

**Rozhodnutí:** spoléhat na kontroly platforem — RLS v Supabase, tajné
klíče jen v Supabase secrets a EAS environment variables, tokeny v telefonu
v zabezpečeném úložišti (`expo-secure-store`, `UNVERIFIED`), gitleaks
v CI [S24], Dependabot, MFA a stropy útraty na všech účtech.
**Proč ne ostatní:** vlastní autentizace nebo šifrování — zakázáno,
neprogramátor je nezkontroluje.
**Kdy znovu zvážit:** po profesionální revizi (T201).
