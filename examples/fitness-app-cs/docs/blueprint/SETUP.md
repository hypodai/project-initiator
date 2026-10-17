# Nastavení

Postupuj v tomto pořadí. Každý krok říká, co vytvoří, aby člověk i agent
mohl zkontrolovat, že je hotový. Kroky odpovídají úkolům T001–T005
v [`docs/backlog/QUEUE.md`](../backlog/QUEUE.md).

## 1. Účty (T001)

| Služba | Plán na začátek | Proč | Odkaz |
|--------|-----------------|------|-------|
| GitHub | Free, soukromý repozitář | kód, CI, Dependabot | https://github.com |
| Expo (EAS) | Free | cloudové buildy, odeslání do obchodů [S12] | https://expo.dev |
| Supabase | Free (`dev`), později Pro (`prod`) | databáze, přihlášení, Edge Functions [S11] | https://supabase.com |
| RevenueCat | Free | předplatné [S10] | https://www.revenuecat.com |
| Apple Developer Program | 99 $/rok | App Store, TestFlight, HealthKit [S14] | https://developer.apple.com/programs/ |
| Google Play Console | 25 $ jednorázově | Google Play, Health Connect [S15] | https://play.google.com/console |
| Sentry | Free | hlášení chyb (potvrdit v T106) | https://sentry.io |

U každého účtu, který může vydat, platit nebo smazat (GitHub, Expo,
Supabase, RevenueCat, Apple, Google, e-mail vlastníka): **zapni MFA**
a nastav **upozornění na útratu a tvrdý strop** dřív, než cokoli vytvoříš.
U Supabase Pro nech zapnutý spending cap [S11]. Hesla ulož do správce hesel.

Ověř a zapiš do `PLAN.md` (Předpoklady) to, co je zde `UNVERIFIED`: region EU
a DPA u Supabase, RevenueCat a Sentry; limit Small Business Program;
co nabízí GitHub Free pro soukromý repozitář (ochrana `main`, secret
scanning, minuty Actions).

## 2. Repozitář (T001)

GitHub repozitář zatím nevznikl (`gh` se při zakládání nepoužil). Ručně:

1. Na GitHubu vytvoř **soukromý** repozitář bez README (např. `fitness-app`).
2. V této složce: `git remote add origin https://github.com/<účet>/fitness-app.git`
   a `git push -u origin main`.

Potom:

- Chraň `main`: vyžaduj zelené CI před merge — ověř, co Free plán
  u soukromého repozitáře dovolí; pokud to nejde, zůstává CI jako pravidlo
  v `AGENTS.md`.
- Zapni: Dependabot alerts a security updates; secret scanning a push
  protection, pokud je plán nabízí (jinak hlídá gitleaks v CI).
- Tajné klíče nikdy v repozitáři — lokálně v `.env` (git je ignoruje),
  pro buildy v EAS environment variables, pro Edge Functions v Supabase
  secrets.

## 3. Kostra aplikace (T002)

```bash
npx create-expo-app@latest mobile --template default
cd mobile
npx expo install expo-dev-client
npm install --save-dev jest-expo jest @types/jest
```

Potom v `mobile/package.json` doplň skripty `typecheck` (`tsc --noEmit`)
a `test` (`jest`) a ověř, že existuje `lint` (`expo lint`). Ověřovací
příkazy celého projektu:

```bash
cd mobile && npm run lint && npm run typecheck && npm test
```

Přesné volby šablony a jestli `lint` šablona už obsahuje, ověř v aktuální
dokumentaci Expo (`UNVERIFIED`). Referenční šablona kombinující celý stack
nebyla nalezena.

## 4. Služby (T003–T005)

**EAS (T003):**
```bash
npx eas-cli@latest login
cd mobile
npx eas-cli@latest init
npx eas-cli@latest build:configure
npx eas-cli@latest build --profile development --platform android
npx eas-cli@latest build --profile development --platform ios
```
Profily `development`, `preview`, `production` v `eas.json`. Development
build nainstaluj na oba telefony (iPhone vyžaduje registraci zařízení přes
`eas device:create`, `UNVERIFIED`).

**Supabase (T004):**
- Založ dva projekty: `fitness-dev` a `fitness-prod`, **region EU**
  (Frankfurt, pokud je v nabídce — `UNVERIFIED`).
- V repozitáři: `npx supabase init` (vytvoří `supabase/` s migracemi a
  funkcemi); tabulky jen přes migrace.
- RLS zapnout na každé tabulce hned při vytvoření.
- Do `mobile/.env` jen URL a veřejný `anon` klíč projektu `dev`
  (`EXPO_PUBLIC_SUPABASE_URL`, `EXPO_PUBLIC_SUPABASE_ANON_KEY`).
  **`service_role` klíč nikdy do aplikace ani do `.env`** — patří jen do
  Supabase secrets.
- Kontrola spojení: aplikace načte z `dev` testovací řádek.

**Knihovny pro zdravotní data (T005):**
Před přidáním ověř přesný název, údržbu, licenci a počet stažení:
`react-native-health-connect` [S6], `@kingstinct/react-native-healthkit`
(`UNVERIFIED`), záloha `expo-healthkit` od appeeky [S5]. Pak
`npx expo install <balíček>`, Expo plugin do `app.json` a nový
development build.

**RevenueCat (T104):** projekt, produkty (měsíční, roční) v App Store
Connect a Google Play Console, SDK `react-native-purchases` (název ověřit —
`UNVERIFIED`), veřejné klíče SDK do `.env`, webhook do Edge Function
s tajnou autorizační hlavičkou v Supabase secrets.

**Sentry (T106):** ověřit cenu, region a DPA, pak SDK pro Expo.

## 5. CI a prostředí (T002)

- `.github/workflows/ci.yml`: odkomentuj kroky (instalace z lockfile, lint,
  typecheck, test, `npm audit`). Akce jsou připnuté na SHA:
  `actions/checkout` v7.0.1 [S22], `actions/setup-node` v7.1.0 [S23],
  `gitleaks/gitleaks-action` v3.0.0 [S24].
- Riziko je vysoké → v CI běží skenování tajemství (gitleaks). Statická
  analýza CodeQL: ověř, jestli je pro soukromý repozitář zdarma
  (`UNVERIFIED`); pokud ne, zůstává `npm audit` + `expo lint`.
- Prostředí: viz `ARCHITECTURE.md` (development / preview / production).

## 6. Nastavení AI kódování

- Nástroj: Claude Code (předplatné už máš). Otevři terminál v této složce,
  spusť `claude` a napiš: „Začni úkolem T001“.
- **Dohodnutý režim: úroveň 2 — semi-auto.** V Claude Code tomu odpovídá
  režim `acceptEdits` (automaticky přijímá úpravy souborů v projektu) plus
  allowlist ověřovacích příkazů v `.claude/settings.json` [S21]. Nastavuješ
  ho ty, ne agent:
  - během sezení: **Shift+Tab** přepíná režimy (Manual → Accept edits →
    Plan …), aktuální režim je vidět dole pod promptem (`UNVERIFIED` —
    popis z dokumentace režimů, stránka nenačtena);
  - trvale: v uživatelském nastavení (`~/.claude/settings.json`) klíč
    `"permissions": { "defaultMode": "acceptEdits" }` [S21];
  - přehled a úprava pravidel: příkaz `/permissions` [S21].
- Ve fázi 0 (zakládání účtů a klíčů) doporučuji režim Manual (`default`).
- `.claude/settings.json` v repozitáři (odsouhlaseno): povoluje ověřovací
  příkazy a zakazuje čtení `.env` souborů. `bypassPermissions` nepoužívej —
  podle dokumentace jen v izolovaném kontejneru nebo VM [S21].

## Hotovo, když

- [ ] Development build aplikace běží na iPhonu i na Androidu a načte
      testovací řádek ze Supabase `dev`
- [ ] CI na GitHubu je zelené (lint, typecheck, test, audit, gitleaks)
- [ ] MFA, upozornění na útratu a stropy jsou zapnuté u všech účtů výše
