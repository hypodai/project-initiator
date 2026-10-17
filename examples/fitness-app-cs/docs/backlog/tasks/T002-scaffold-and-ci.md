# T002 — Kostra aplikace Expo, testy, CI zelené

**Fáze:** 0 — Nastavení | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T001

## Cíl

Kostra aplikace z oficiálního generátoru s testy a CI, které hlídá každou
další změnu.

## Kroky

1. Na větvi `t002-scaffold`: `npx create-expo-app@latest mobile --template default`
   (volby ověř v aktuální dokumentaci Expo).
2. `cd mobile && npx expo install expo-dev-client`;
   `npm install --save-dev jest-expo jest @types/jest` (balíčky prověř
   podle `AGENTS.md`).
3. Do `mobile/package.json` skripty `typecheck` (`tsc --noEmit`) a `test`
   (`jest`); ověř `lint` (`expo lint`).
4. První test (např. vykreslení úvodní obrazovky), který prochází.
5. V `.github/workflows/ci.yml` odkomentuj kroky; ověř licenční požadavek
   gitleaks pro osobní účet; CodeQL jen pokud je zdarma.
6. Zkontroluj licence přímých závislostí (žádné GPL/AGPL).

## Kritéria přijetí

- [ ] `mobile/` obsahuje kostru Expo a `package-lock.json`
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází
- [ ] CI na GitHubu je zelené na PR
- [ ] `.gitignore` pokrývá vygenerované soubory

## Poznámky

`docs/blueprint/SETUP.md` (3, 5), `DECISIONS.md` — Mobilní aplikace.
