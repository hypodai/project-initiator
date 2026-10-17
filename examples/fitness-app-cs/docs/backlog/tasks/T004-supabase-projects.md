# T004 — Supabase dev a prod v EU, migrace, RLS

**Fáze:** 0 — Nastavení | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T002

## Cíl

Databáze a přihlášení připravené tak, aby zdravotní data byla od první
tabulky chráněná RLS a uložená v EU.

## Kroky

1. Vlastník založí projekty `fitness-dev` a `fitness-prod` v regionu EU
   (ověřit dostupné regiony); `prod` zatím na Free.
2. V repozitáři `npx supabase init`; první migrace s tabulkou `profiles`
   a zapnutým RLS.
3. `mobile/.env` jen s URL a `anon` klíčem projektu `dev` (hodnoty vloží
   vlastník, agent je nečte).
4. Klient `@supabase/supabase-js` s ukládáním session do zabezpečeného
   úložiště (prověřit `expo-secure-store`).
5. Kontrola spojení z aplikace.

## Kritéria přijetí

- [ ] Oba projekty existují v EU; DPA staženo / podepsáno
- [ ] Migrace v `supabase/migrations/`, RLS zapnuté
- [ ] Aplikace načte testovací řádek z `dev`
- [ ] `service_role` klíč není nikde v repozitáři ani v aplikaci
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`DECISIONS.md` — Backend a databáze, `SECURITY.md`, `THREAT-MODEL.md` H1–H2.
