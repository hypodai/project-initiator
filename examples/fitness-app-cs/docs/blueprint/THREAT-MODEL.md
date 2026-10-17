# Model hrozeb — Fitness appka (pracovní název)

*Zjednodušený model, vytvořen 2026-10-16. Přehodnotit na každé bráně fáze
a při přidání nového vstupního bodu nebo kategorie dat. Úroveň rizika:
**vysoká**.*

## Aktiva

| Aktivum | Proč je cenné |
|---------|---------------|
| Zdravotní údaje uživatelů (tep, tréninky) | zvláštní kategorie GDPR; únik = pokuty, ztráta důvěry, oznamovací povinnost |
| Účty uživatelů (e-mail, přihlášení) | převzetí účtu = přístup k zdravotním datům |
| `service_role` klíč Supabase, webhookový tajný klíč RevenueCat | obejdou RLS / podvrhnou předplatné |
| Účty vývojáře (Apple, Google, Expo, GitHub, Supabase) | vydání podvržené aplikace, smazání dat |
| Tržby z předplatného | podvržený stav předplatného = ztráta tržeb |
| Dostupnost a pověst | jediný vývojář, recenze v obchodech |

## Aktéři

| Aktér | Schopnosti a motivace |
|-------|------------------------|
| Anonymní návštěvník / bot | volá veřejné API Supabase s veřejným `anon` klíčem z aplikace; zkouší číst cizí řádky, hromadné registrace |
| Přihlášený uživatel zneužívající přístup | mění ID v dotazech, zkouší odemknout placené funkce bez platby |
| Uniklý nebo ukradený klíč | `service_role` v repozitáři nebo v aplikaci = čtení všech dat |
| Kompromitovaná závislost nebo CI | škodlivý npm balíček (i AI vymyšlený název), akce GitHubu s posunutým tagem |
| Agent ovlivněný prompt injection | Claude Code čte README balíčků, issues, webové stránky; může být naveden spustit příkaz nebo vypsat `.env` |

## Vstupní body

- Supabase REST/Realtime API (veřejné, chráněné RLS)
- Supabase Auth: registrace, přihlášení, reset hesla
- Edge Function webhook RevenueCat (veřejná URL)
- Edge Function smazání účtu / export dat
- aplikace v telefonu (reverzní inženýrství, `EXPO_PUBLIC_*` proměnné jsou
  veřejné)
- GitHub Actions, Dependabot PR, závislosti npm
- účty vlastníka (Apple, Google, Expo, GitHub, Supabase, RevenueCat, e-mail)
- Claude Code na notebooku vlastníka

## Hlavní hrozby

| # | Hrozba | Pravděpodobnost | Dopad | Zmírnění (a kde žije) | Zbytkové |
|---|--------|-----------------|-------|------------------------|----------|
| H1 | Chybějící nebo chybná RLS politika → čtení cizích zdravotních dat | střední | vysoký | RLS zapnuté na každé tabulce, testy „uživatel B nevidí data A“ v CI (T102), revize (T201) | nízké |
| H2 | `service_role` klíč v aplikaci nebo repozitáři | střední | vysoký | jen v Supabase secrets pro Edge Functions; gitleaks v CI; pravidlo v AGENTS.md; nikdy s prefixem `EXPO_PUBLIC_` | nízké |
| H3 | Podvržený webhook → zdarma předplatné | střední | střední | ověření autorizační hlavičky, idempotence, stav i z RevenueCat API (T104) | nízké |
| H4 | Převzetí účtu vývojáře → podvržená verze aplikace | nízká | vysoký | MFA všude, unikátní hesla, správce hesel (T001) | nízké |
| H5 | Škodlivá / vymyšlená závislost | střední | vysoký | kontrola balíčku před přidáním, lockfile, `npm audit`, Dependabot | střední |
| H6 | Zdravotní data v logech nebo v Sentry | střední | střední | pravidlo v AGENTS.md, filtr citlivých dat v Sentry (T106) | nízké |
| H7 | Prompt injection do Claude Code → spuštění příkazu, vypsání `.env` | nízká | vysoký | režim semi-auto, deny pravidla pro `.env` v `.claude/settings.json`, žádné produkční klíče na notebooku | střední |
| H8 | Hromadné registrace / zneužití e-mailů z Auth | střední | nízký | rate limity Supabase Auth (`UNVERIFIED`), CAPTCHA při zneužití | nízké |

## Přijatá rizika

- H5 a H7 zůstávají střední — kód nikdo nečte řádek po řádku. Přijímá
  vlastník do profesionální revize (T201), pak přehodnotit.
- Data z Garminu jsou neúplná (není to bezpečnostní riziko, viz RISKS.md R5).
