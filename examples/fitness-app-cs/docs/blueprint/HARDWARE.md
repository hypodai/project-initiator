# Hardware

Windows notebook na vývoj stačí — iOS buildy běží v cloudu EAS [S12].
Navíc je potřeba skutečný iPhone a skutečný Android telefon s hodinkami,
protože data z Apple Health / Health Connect nejdou v Expo Go ani bez
development buildu otestovat [S5].

| Položka | Na co | Koupit / půjčit / vynechat | Cena | Alternativy | Zdroj |
|---------|-------|---------------------------|------|-------------|-------|
| Windows notebook | vývoj, Claude Code | máš | 0 Kč | — | — |
| iPhone s iOS 16.4+ | testování HealthKit, nákupů, TestFlight | půjčit od známého, nebo koupit použitý | `UNVERIFIED` — cenu ověřit v T001 | tester s iPhonem přes TestFlight | [S5] |
| Android telefon s Androidem 8+ a Health Connect | testování Health Connect a nákupů | použitý telefon, pokud nemáš | `UNVERIFIED` | tester s Androidem | [S5][S6] |
| Chytré hodinky (Apple Watch a/nebo Garmin) | ověření importu z hodinek | vlastní / půjčit | `UNVERIFIED` | testeři se svými hodinkami | [S17][S18] |
| Mac | — | vynechat | 0 Kč | EAS Build v cloudu | [S12] |

## Vývojový počítač

Windows stačí. Node.js LTS a Git; Android emulátor není nutný (testuje se
na skutečném telefonu). RAM a disk: běžný notebook s 8 GB+ zvládne
(`UNVERIFIED estimate`).

## Testovací zařízení

Minimum: jeden iPhone (iOS 16.4+) a jeden Android (8+ / API 26+) — dáno
knihovnami pro zdravotní data [S5]. Ideálně jeden starší / levnější
Android, aby se ukázalo, jestli aplikace nezpomaluje.

## Zařízení, senzory

Hodinky se nepřipojují přímo — data jdou přes aplikaci výrobce do Apple
Health / Health Connect. Garmin posílá do Apple Health u tréninků jen min.
a max. tep [S17]; do Health Connect kroky, tep, spánek a váhu [S18].

## Požadavky na zařízení uživatelů

iOS 16.4+ nebo Android 8+ (API 26+) [S5]; na Androidu nainstalovaný /
vestavěný Health Connect (`UNVERIFIED` pro starší verze Androidu).
