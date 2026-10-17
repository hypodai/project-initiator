# ADR-0001: Zaznamenávat architektonická rozhodnutí

- **Stav:** přijato
- **Datum:** 2026-10-16

## Kontext

Rozhodnutí udělaná teď se budou později zpochybňovat — vlastníkem, příštím
sezením agenta nebo vývojářem, který prototyp převezme. Bez záznamu se
zdůvodnění ztratí a rozhodnutí se buď znovu otevírají, nebo tiše obracejí.

## Rozhodnutí

Architektonicky významná rozhodnutí zapisujeme jako krátká ADR do
`docs/adr/`, číslovaná postupně: kontext, rozhodnutí, alternativy,
důsledky. Důkazy a srovnání jsou v `docs/blueprint/`; ADR na ně odkazuje.

Nové ADR vzniká při změně stacku nebo knihovny v něm, datového modelu,
hostingu nebo externí služby, přihlašování, RLS politik, zpracování
zdravotních údajů, logiky předplatného — nebo čehokoli, u čeho se budoucí
čtenář zeptá „proč jsme to udělali takhle?“.

## Důsledky

- Pár minut psaní u každého významného rozhodnutí.
- Nahrazená ADR zůstávají se stavem `nahrazeno NNNN`.
