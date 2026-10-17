# T101 — Nejtenčí průřez: přihlášení e-mailem, zápis tréninku, historie

**Fáze:** 1 — První verze | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T004

## Cíl

Nejtenčí funkční průřez hlavní funkce: uživatel se přihlásí, zapíše
trénink a vidí ho v historii — a nikdo jiný ho nevidí.

## Kroky

1. Přihlášení e-mailem přes Supabase Auth.
2. Migrace: `workouts`, `workout_exercises`, `sets` s `user_id` a RLS
   (číst a zapisovat smí jen vlastník řádku).
3. Obrazovky: nový trénink (cvik, série, opakování, váha) a historie.
4. Integrační test RLS proti `dev`: uživatel B nevidí ani nezmění data
   uživatele A.
5. End-to-end smoke test hlavního toku.

## Kritéria přijetí

- [ ] Trénink se uloží a zobrazí v historii na obou platformách
- [ ] Test RLS prochází v CI
- [ ] Žádné osobní údaje v lozích
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`ARCHITECTURE.md` (hlavní tok), `SECURITY.md` (pravidla projektu).
