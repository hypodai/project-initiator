# T105 — Zásady soukromí, export dat, štítky soukromí, odlišení

**Fáze:** 1 — První verze | **Priorita:** P1 | **Stav:** ready | **Závisí na:** T103, T104

## Cíl

Splnit požadavky GDPR a obchodů na transparentnost a ověřit, čím se
aplikace liší od konkurence.

## Kroky

1. Koncept zásad ochrany soukromí (česky) podle skutečného zpracování
   a seznamu zpracovatelů ze `STACK-FIT.md`; zveřejnit na stálé URL.
2. Export vlastních dat (JSON / CSV) v aplikaci.
3. Podklady pro štítky soukromí (App Store) a Data safety (Google Play),
   deklarace zdravotních oprávnění Health Connect.
4. Formulovat odlišení od Hevy a Strong a ověřit ho na 5 testerech;
   výsledek do `LANDSCAPE.md`.

## Kritéria přijetí

- [ ] Zásady zveřejněné, odkaz v aplikaci
- [ ] Export dat funguje
- [ ] Podklady pro obchody připravené
- [ ] Odlišení zapsané v `LANDSCAPE.md`
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

Text zásad projde odborníkem v T201. `RISKS.md` R3 a R4.
