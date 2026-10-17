# T201 — Bezpečnostní a GDPR revize, DPIA, test obnovy

**Fáze:** 2 — Před spuštěním | **Priorita:** P0 | **Stav:** ready | **Závisí na:** Fáze 1

## Cíl

Před prvními platícími uživateli nechat člověka s odborností zkontrolovat
to, co AI napsala a nikdo nečetl: zdravotní data, RLS, Edge Functions,
souhlas a zásady.

## Kroky

1. Vlastník poptá revizi (bezpečnost Supabase a mobilní aplikace + GDPR /
   DPIA); cenu doplní do `COSTS.md`.
2. Agent připraví podklady: `ARCHITECTURE.md`, `THREAT-MODEL.md`, seznam
   RLS politik a Edge Functions, zpracovatelé.
3. Nálezy opravit — každý jako samostatný úkol.
4. Přepnout `prod` na Supabase Pro (tvrdá brána — rozpočet), vyzkoušet
   obnovu ze zálohy a zapsat datum do `OPERATIONS.md`.

## Kritéria přijetí

- [ ] Zpráva z revize a DPIA uložené mimo repozitář
- [ ] Nálezy s vysokou závažností opravené
- [ ] Obnova ze zálohy vyzkoušená
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`RISKS.md` (Kde verze postavená AI nestačí); vysoká úroveň rizika.
