# T202 — Beta: TestFlight a uzavřený test Google Play

**Fáze:** 2 — Před spuštěním | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T201

## Cíl

Ověřit hlavní tok a ochotu platit na skutečných testerech.

## Kroky

1. Build do TestFlight a do uzavřeného testu Google Play (ověřit aktuální
   pravidlo počtu testerů a délky testu pro nové osobní účty [S15]).
2. Získat testery s Apple Watch a s Garminem.
3. Sbírat zpětnou vazbu; měřit kritéria úspěchu z `PLAN.md`.

## Kritéria přijetí

- [ ] Uzavřený test splňuje požadavky Google Play
- [ ] Alespoň 10 testerů prošlo hlavní tok
- [ ] Zpětná vazba zapsaná, úkoly vytvořené
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

Tvrdá brána: odeslání buildu do obchodů.
