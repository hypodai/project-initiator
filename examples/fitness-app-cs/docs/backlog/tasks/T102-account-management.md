# T102 — Sign in with Apple / Google, smazání účtu v aplikaci

**Fáze:** 1 — První verze | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T101

## Cíl

Účet podle pravidel obchodů: pohodlné přihlášení a smazání účtu přímo
v aplikaci.

## Kroky

1. Ověř aktuální pravidla Apple (Sign in with Apple při sociálním
   přihlášení, smazání účtu v aplikaci) a Google Play.
2. Sign in with Apple a Google přes Supabase Auth.
3. Smazání účtu: Edge Function (jediné místo se `service_role`) smaže účet
   i všechna data; potvrzovací krok.
4. Test: po smazání nezůstane žádný řádek uživatele.

## Kritéria přijetí

- [ ] Přihlášení přes Apple i Google funguje na preview buildu
- [ ] Smazání účtu odstraní účet i data (test)
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

Tvrdá brána: Edge Function se `service_role` — vlastník kontroluje změnu.
