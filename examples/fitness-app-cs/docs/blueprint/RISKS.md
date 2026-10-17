# Rizika

*Tohle je vodítko pro stavitele, ne právní rada. Předpisy se mění — před
spolehnutím se ověř v odkazovaných zdrojích.*

## Registr rizik

| # | Riziko | Pravděpodobnost | Dopad | Zmírnění | Vlastník |
|---|--------|-----------------|-------|----------|----------|
| R1 | Únik zdravotních dat (chyba v RLS, uniklý `service_role` klíč) | střední | vysoký | RLS na každé tabulce + automatické testy přístupu (T102), `service_role` jen v Edge Functions, revize T201 | vlastník |
| R2 | Zpracování zdravotních údajů bez platného výslovného souhlasu (GDPR čl. 9) | střední | vysoký | samostatná obrazovka souhlasu před čtením HealthKit / Health Connect, záznam souhlasu, odvolání (T103); DPIA (T201) [S16] | vlastník |
| R3 | Zamítnutí v App Store / Google Play (HealthKit, Health Connect, předplatné, smazání účtu, štítky soukromí) | vysoká | střední | od začátku plnit pravidla (T105), beta přes TestFlight a uzavřený test Google Play (T202) [S15] | vlastník |
| R4 | Žádné odlišení od levné konkurence (Hevy 2,99 $, Strong 4,99 $) | vysoká | vysoký | ověřit odlišení na testerech před investicí do placené části; kritéria úspěchu v PLAN.md [S1] | vlastník |
| R5 | Neúplná data z Garminu (u tréninků jen min./max. tep přes Apple Health) | jistá | střední | komunikovat v aplikaci; přímé API Garmin / agregátor až při tržbách [S17][S18] | vlastník |
| R6 | Provoz nad rozpočtem (Supabase Pro 543 Kč > strop 500 Kč) | jistá po spuštění | nízký | Pro až od ~7 předplatitelů; COSTS.md [S11] | vlastník |
| R7 | Účet bez ověřeného tvrdého stropu útraty (EAS placené plány, Sentry) | nízká | střední | zůstat na Free plánech, upozornění na útratu, kde existují (T001) | vlastník |
| R8 | Ukradený účet Apple / Google / Expo / GitHub → podvržená verze aplikace | nízká | vysoký | MFA na všech účtech, žádné sdílené heslo (T001) | vlastník |
| R9 | Mladé knihovny pro zdravotní data (nekompatibilita, opuštění) | střední | střední | ověřit v T005, záložní knihovna v DECISIONS.md [S5][S6] | vlastník |
| R10 | Změna obchodních podmínek Apple v EU (od 1. 10. 2026) | střední | nízký | volbu platebních podmínek držet 12 měsíců; zůstat u nákupů v aplikaci [S13] | vlastník |
| R11 | Daně a DPH z tržeb z obchodů | střední | střední | konzultace s účetní před prvními tržbami (`UNVERIFIED`) | vlastník |

## Oborové praxe, které platí

**Osobní a zdravotní údaje (GDPR, uživatelé v ČR/SK):**
- tep a tréninková data z hodinek jsou zdravotní údaje — zvláštní kategorie
  podle čl. 9 GDPR; právní titul je výslovný souhlas, oddělený od obchodních
  podmínek, zdokumentovaný a odvolatelný [S16]
- DPIA (posouzení vlivu) před spuštěním — rozsáhlé zpracování zdravotních
  údajů ji typicky vyžaduje (`UNVERIFIED`, ověřit u ÚOOÚ)
- minimalizace: ukládat jen souhrny tréninku, které aplikace opravdu
  zobrazuje; žádné zdravotní údaje v logech, analytice ani v Sentry
- data v EU (Supabase region EU), smlouvy o zpracování (DPA) se všemi
  zpracovateli — viz STACK-FIT.md
- export a smazání dat na žádost, přímo v aplikaci
- šifrování při přenosu (HTTPS) a v klidu (zajišťuje platforma)

**Platby:**
- digitální předplatné v aplikaci jen přes nákupy v obchodech (RevenueCat);
  aplikace nikdy nevidí kartu
- stav předplatného se mění jen z ověřeného webhooku RevenueCat, zpracování
  idempotentní (opakovaný webhook nic nerozbije)
- odemčení funkcí se kontroluje i na serveru, ne jen v aplikaci

**Mobilní aplikace:**
- pravidla App Store Review a Google Play, štítky soukromí (App Store) a
  formulář Data safety (Google Play) musí odpovídat skutečnosti
- žádat jen o oprávnění, která aplikace používá (jen čtení tréninků a tepu,
  ne vše z Apple Health)
- smazání účtu přímo v aplikaci (`UNVERIFIED` — požadavek Apple, ověřit
  v T101)
- hlášení pádů (Sentry) bez osobních údajů

## Kde verze postavená AI nestačí

- **Zdravotní data a GDPR:** DPIA, text souhlasu a zásady ochrany soukromí
  by měl před placeným spuštěním projít člověk, který se v GDPR vyzná.
- **Bezpečnost:** RLS politiky, Edge Functions (webhook, smazání účtu)
  a nastavení Supabase — profesionální bezpečnostní revize před prvními
  platícími uživateli (T201).
- **Platby:** logiku nároků otestovat v sandboxu obchodů na obou
  platformách; při pochybnosti revize.
- **Daně:** zdanění příjmů z obchodů a DPH — účetní.

## Rozhodnutí, která stojí za to přehodnotit

- Expo — pokud bude potřeba aplikace přímo na hodinkách nebo výkon nestačí.
- HealthKit + Health Connect — když testeři s Garminem chtějí plná data
  (průběh tepu, GPS) → přímé API Garmin nebo Terra (Top).
- RevenueCat — tržby nad 2 500 $/měs.; porovnat s Adapty.
- Supabase — přes 8 GB dat nebo změna podmínek; Free → Pro od ~7 předplatitelů.
- Supabase Auth — při prodeji firmám (SSO).
- EAS — když 15 buildů měsíčně nestačí.
- Sentry — potvrdit v T106.
- GitHub — ochrana větve a minuty Actions u soukromého repozitáře (T001).
