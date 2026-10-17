# T005 — Ověřit knihovny HealthKit a Health Connect

**Fáze:** 0 — Nastavení | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T003

## Cíl

Před stavbou funkcí ověřit, že vybrané knihovny opravdu čtou tréninky a tep
na skutečných zařízeních — je to nejméně jistá část stacku.

## Kroky

1. Prověř `react-native-health-connect` a `@kingstinct/react-native-healthkit`
   (přesný název, poslední vydání, údržba, licence, kompatibilita
   s aktuálním Expo SDK); záloha `expo-healthkit` (appeeky).
2. Přidej přes `npx expo install`, nastav Expo plugin, nový development
   build.
3. Na zkušební obrazovce přečti posledních 7 dní tréninků a tepu (jen
   čtení, jen tyto typy dat).
4. Ověř s Apple Watch a s Garminem (Garmin Connect → Apple Health /
   Health Connect), co přesně dorazí.
5. Výsledek a volbu knihoven zapiš do ADR-0003.

## Kritéria přijetí

- [ ] Tréninky a tep se načtou na iPhonu i Androidu
- [ ] Zapsáno, která data z Garminu dorazí (porovnat se `STACK-FIT.md`)
- [ ] ADR-0003 se zvolenými knihovnami
- [ ] Zkušební obrazovka není v produkčním buildu
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

`DECISIONS.md` — Data z hodinek, `RISKS.md` R5 a R9. Zatím bez ukládání do
databáze (to až po souhlasu v T103).
