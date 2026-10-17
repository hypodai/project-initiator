# Trh — Fitness appka (pracovní název)

*Účel: komerční produkt · zkontrolováno 2026-10-16 · hloubka quick*

## Verdikt

**Hybrid** — koupit hotovou aplikaci nedává smysl, protože cílem je vlastní
produkt na prodej. Komoditní části (databáze a přihlášení, předplatné, data
z hodinek, buildy) se ale koupí jako služby a postaví se jen to, co je
pointa: zápis a vyhodnocení tréninku. To nejlépe sedí na prioritu č. 1
(nízká cena) i č. 3 (málo vlastního kódu a provozu).

Upřímně: trh je přeplněný a konkurence je levná a vyzrálá. Bez jasného
odlišení bude těžké získat platící uživatele. Doporučuji odlišení
formulovat a ověřit na testerech **dřív**, než se postaví placená část
(úkol T105 a fáze 2).

## Zvažované možnosti

| Možnost | Co to je | Pokrývá | Chybí | Cena dnes | Lock-in / export | Zdroj |
|---------|----------|---------|-------|-----------|------------------|-------|
| Koupit: Hevy / Strong | hotové aplikace na záznam tréninku | záznam tréninků, Apple Watch, historie | není to tvůj produkt — nedá se prodávat | Hevy 2,99 $/měs., Strong 4,99 $/měs. | export CSV `UNVERIFIED` | [S1] |
| Upravit: white-label / no-code builder | šablona fitness aplikace pod vlastní značkou | rychlé spuštění | napojení na Health Connect a předplatné bývá omezené; měsíční poplatky | `UNVERIFIED` (nehledáno — quick) | vysoký | — |
| Upravit: Open Wearables | open-source server pro data z hodinek (MIT) | Garmin, Polar, Suunto, Whoop, Apple Health | provoz vlastního serveru — pro neprogramátora náročné | 0 $ licence + hosting | nízký | [S9] |
| Postavit (hybrid) | vlastní aplikace + služby | vše z první verze | nic zásadního, ale čas a odlišení | viz COSTS.md | střední (Supabase = Postgres, export možný) | — |

Tříletý náklad vlastnictví: pro *koupit* není relevantní (nejde prodávat).
Hybrid: ~0–179 Kč/měs. během vývoje, ~722–1 135 Kč/měs. v provozu + 15 %
provize obchodů + tvůj čas. Čas odhadem 5–10 h týdně po 2–3 měsíce
(`UNVERIFIED estimate`).

## Konkurence a náhražky

| Název | Pozicování | Cena | Silné stránky | Opakované stížnosti | Zdroj |
|-------|------------|------|---------------|---------------------|-------|
| Hevy | záznam silových tréninků, komunita | 2,99 $/měs., 23,99 $/rok, 74,99 $ doživotně; free: 4 rutiny, 7 vlastních cviků, ~3 měsíce historie | nejlepší poměr cena/výkon, Android, programy | `UNVERIFIED` (recenze nečteny — quick) | [S1] |
| Strong | jednoduchý záznam silových tréninků | 4,99 $/měs., 29,99 $/rok, 99,99 $ doživotně; free: neomezené tréninky, 3 vlastní rutiny | iOS a Apple Watch | `UNVERIFIED` | [S1] |
| Fitbod, Jefit, Stronger | plány generované algoritmem / AI | `UNVERIFIED` | personalizace | `UNVERIFIED` | [S2] |
| Garmin Connect, Apple Fitness | aplikace výrobců hodinek | zdarma k hodinkám | přímá data z hodinek | slabý záznam silových tréninků `UNVERIFIED` | — |
| Sešit, tabulka | ruční zápis | zdarma | žádná závislost | žádné vyhodnocení | — |

**Čím se tento projekt liší:** zatím ničím doloženým. Kandidáti
k ověření na testerech: aplikace a podpora v češtině, spojení ručního
silového tréninku s tepem z jakýchkoli hodinek v jednom přehledu, nižší cena
pro CZ/SK. Nic z toho zatím není ověřené poptávkou.

**Kontrola názvu / ochranné známky:** neprovedena — projekt má jen pracovní
název. Před vydáním zkontrolovat ÚPV, EUIPO a obsazenost názvu v obou
obchodech (úkol T203).

## Kritéria úspěchu

- Do 3 měsíců od spuštění alespoň 30 platících předplatitelů.
- 40 % uživatelů zapíše trénink i ve 4. týdnu po instalaci.
- Import z hodinek funguje u testerů s Apple Watch i s Garminem bez ručního
  zásahu.
