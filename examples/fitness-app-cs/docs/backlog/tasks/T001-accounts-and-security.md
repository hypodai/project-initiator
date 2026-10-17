# T001 — Účty, MFA, stropy útraty, repozitář

**Fáze:** 0 — Nastavení | **Priorita:** P0 | **Stav:** ready | **Závisí na:** —

## Cíl

Všechny účty existují a jsou zabezpečené dřív, než vznikne první klíč nebo
kód. Ukradený účet vývojáře nebo uniklý klíč je pro tento projekt největší
reálné riziko.

## Kroky

1. Založ účty podle `docs/blueprint/SETUP.md`, krok 1: GitHub, Expo,
   Supabase, RevenueCat, Apple Developer Program (99 $/rok), Google Play
   Console (25 $), Sentry.
2. U každého zapni MFA; hesla do správce hesel.
3. Nastav upozornění na útratu a tvrdé stropy (Supabase spending cap,
   jinde Free plány).
4. Vytvoř soukromý GitHub repozitář a propoj ho (`SETUP.md`, krok 2);
   zapni Dependabot alerts, secret scanning / push protection, pokud jsou
   k dispozici, a ochranu `main`, pokud ji plán dovolí.
5. Ověř a zapiš do `PLAN.md` (Předpoklady) položky `UNVERIFIED` ze
   `SETUP.md`, krok 1 (region EU a DPA u Supabase, RevenueCat a Sentry;
   GitHub Free pro soukromé repozitáře; limit Small Business Program).
6. Přihlas se do Apple Small Business Program.
7. Tento úkol provádí vlastník; agent vede a kontroluje (doporučen režim
   Manual).

## Kritéria přijetí

- [ ] MFA zapnutá na všech 7 účtech a na e-mailu vlastníka
- [ ] Stropy / upozornění útraty nastavené, kde existují; kde ne, zapsáno v `RISKS.md` (R7)
- [ ] Soukromý repozitář existuje, `main` je pushnutý, Dependabot zapnutý
- [ ] Položky `UNVERIFIED` z kroku 5 jsou ověřené a zapsané
- [ ] Žádný klíč není v repozitáři

## Poznámky

`docs/blueprint/SETUP.md` (1–2), `SECURITY.md`, `THREAT-MODEL.md` H4.
