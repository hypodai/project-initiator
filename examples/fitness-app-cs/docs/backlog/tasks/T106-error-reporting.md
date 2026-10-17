# T106 — Hlášení chyb (Sentry) bez osobních údajů — ADR

**Fáze:** 1 — První verze | **Priorita:** P1 | **Stav:** ready | **Závisí na:** T003

## Cíl

Vidět chyby z aplikace, aniž by do nástroje odcházely osobní nebo zdravotní
údaje.

## Kroky

1. Ověř aktuální cenu, limity Free, region dat a DPA Sentry; porovnej
   s alternativou (hlášení pádů v konzolích obchodů). Výsledek do ADR-0004.
2. Pokud schváleno: SDK pro Expo a filtr citlivých dat před odesláním.
3. Test: chyba s e-mailem a tepem v kontextu dorazí bez nich.

## Kritéria přijetí

- [ ] ADR-0004 s rozhodnutím o nástroji
- [ ] Chyby dorazí bez osobních a zdravotních údajů (test)
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`DECISIONS.md` — Hlášení chyb, `THREAT-MODEL.md` H6.
