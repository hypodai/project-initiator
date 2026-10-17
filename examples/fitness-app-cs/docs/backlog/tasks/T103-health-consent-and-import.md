# T103 — Výslovný souhlas a import z Apple Health / Health Connect

**Fáze:** 1 — První verze | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T005, T101

## Cíl

Zákonné a pro uživatele srozumitelné zpracování zdravotních údajů: nejdřív
výslovný souhlas, pak import tréninků a tepu.

## Kroky

1. Obrazovka souhlasu oddělená od obchodních podmínek: jaká data, k čemu,
   kde jsou uložena, jak souhlas odvolat. Text schvaluje vlastník.
2. Tabulka `health_consents` (čas, verze textu, odvolání) s RLS.
3. Teprve po souhlasu žádost o oprávnění (jen čtení tréninků a tepu).
4. Import souhrnů (začátek, délka, typ, průměrný a maximální tep) do
   `workouts`, bez duplicit.
5. Odvolání souhlasu zastaví import a nabídne smazání importovaných dat.
6. Testy: bez souhlasu se nic nečte ani neukládá.

## Kritéria přijetí

- [ ] Bez souhlasu žádné čtení ani ukládání (test)
- [ ] Import funguje s Apple Watch i s Garminem
- [ ] Odvolání souhlasu funguje
- [ ] Žádné zdravotní údaje v lozích
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

Tvrdá brána: souhlas a rozsah zdravotních dat. `RISKS.md` R2, [S16].
