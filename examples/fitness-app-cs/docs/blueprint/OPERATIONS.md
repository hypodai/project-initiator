# Provoz — Fitness appka (pracovní název)

*Používá se od prvního nasazení; docs profil `complete`.*

## Prostředí

| Prostředí | Adresa | Nasazuje se z | Data | Klíče |
|-----------|--------|---------------|------|-------|
| development | development build v telefonu | lokálně / větev úkolu | testovací | Supabase `dev`, v lokálním `.env` |
| preview | TestFlight / interní testování Google Play | `main` po merge, profil EAS `preview` | testovací | Supabase `dev`, EAS environment variables |
| production | App Store / Google Play | tag `vX.Y.Z`, profil EAS `production` | skutečná zdravotní data | Supabase `prod`, jen EAS environment variables a Supabase secrets — nikdy na notebooku |

## Nasazení a návrat zpět

- **Nasazení:** `eas build --profile production --platform all`, pak
  `eas submit`; schválení v obchodech trvá dny. Opravy jen v JavaScriptu
  lze poslat přes `eas update` (OTA) — kanál `production`.
- **Ověření po nasazení:** přihlášení, zápis tréninku, import z Apple
  Health / Health Connect, obnova předplatného v sandboxu; žádné nové chyby
  v Sentry.
- **Návrat zpět:** OTA — znovu publikovat předchozí aktualizaci
  (`eas update:republish`, `UNVERIFIED` — ověřit v dokumentaci EAS); build
  v obchodě vrátit nejde, jen vydat novou verzi. **Migrace databáze nejdou
  vrátit automaticky** — každá migrace má ručně napsaný opačný krok a před
  spuštěním na `prod` záloha.

## Kontrolní seznam vydání

- [ ] CI zelené; v `CHANGELOG.md` přesunuto `[Unreleased]` pod novou verzi
- [ ] Tag verze (SemVer, `vX.Y.Z`) a stejné číslo v `app.json`
- [ ] Štítky soukromí / Data safety odpovídají aktuálnímu zpracování dat
- [ ] Build, odeslání, ověření, oznámení testerům

## Zálohy a obnova

| Data | Záloha | Frekvence | Uchování | Poslední test obnovy |
|------|--------|-----------|----------|----------------------|
| Supabase `prod` (Postgres) | Supabase Pro | denně | 7 dní [S11] | zatím ne — úkol T201 |
| Supabase `dev` | žádná (Free) | — | — | nepotřeba |
| Repozitář | GitHub + lokální klon | při pushi | — | — |

## Monitoring a upozornění

| Co | Nástroj | Upozornění jde | Práh |
|----|---------|----------------|------|
| Chyby aplikace | Sentry | e-mail vlastníka | nová chyba / nárůst |
| Dostupnost backendu | stav Supabase + Sentry | e-mail vlastníka | výpadek API |
| Útrata / stropy | spending cap Supabase, Free plány | e-mail vlastníka | nad rozpočet |
| Předplatné | přehled RevenueCat | — | týdenní kontrola |

## Incidenty

Kdo řeší: vlastník (jediný vývojář, bez pohotovosti). První kroky: zastavit
škody (zneplatnit klíče, vypnout funkci), informovat dotčené, obnovit,
zapsat co se stalo. Při úniku zdravotních údajů: oznámení ÚOOÚ do 72 hodin
a dotčeným uživatelům, pokud je vysoké riziko (`UNVERIFIED` — ověřit
aktuální povinnosti). Postup zneplatnění klíčů: `SECURITY.md`.

## Pravidelná údržba

| Úkol | Jak často | Kdo |
|------|-----------|-----|
| Projít PR od Dependabotu | týdně | vlastník + agent |
| Rotace klíčů (`service_role`, webhook) | každých 6 měsíců a po každém podezření | vlastník |
| Zkouška obnovy ze zálohy | každé 3 měsíce | vlastník |
| Znovu ověřit ceny a limity placených služeb | každých 6 měsíců | vlastník |
| Projít model hrozeb a RISKS.md | na každé bráně fáze | agent (docs-keeper) + vlastník |
