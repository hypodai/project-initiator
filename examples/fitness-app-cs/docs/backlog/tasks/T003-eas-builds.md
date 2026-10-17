# T003 — EAS development a preview buildy na telefonech

**Fáze:** 0 — Nastavení | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T002

## Cíl

Aplikace se dá postavit pro iOS i Android bez Macu a nainstalovat na
testovací telefony — předpoklad pro všechno se zdravotními daty a nákupy.

## Kroky

1. `npx eas-cli@latest login`; v `mobile/`: `npx eas-cli@latest init`
   a `npx eas-cli@latest build:configure`.
2. Profily `development`, `preview`, `production` v `eas.json`.
3. Registrace iPhonu (`eas device:create`, ověřit v dokumentaci)
   a development build pro obě platformy; instalace na telefony.
4. Do `README.md` zapsat, jak build spustit.

## Kritéria přijetí

- [ ] Development build běží na iPhonu i Androidu
- [ ] Preview build lze nainstalovat testerovi
- [ ] Spotřeba buildů je v limitu Free (15 + 15 měsíčně)
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`SETUP.md` (4), `COSTS.md` (hranice free tieru EAS).
