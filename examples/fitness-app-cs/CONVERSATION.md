# Transcript — eval-1-with-skill (project-initiator)

Simulovaná konverzace. Zprávy asistenta jsou přesně tak, jak by šly
uživateli; odpovědi uživatele jsou simulované a označené **[SIMULOVÁNO]**.

---

## Uživatel

> chci udelat vibe coded fitness appku na mobily. sleduje treninky, napoji se na chytre hodinky a chci to prodavat jako predplatne. neumim programovat, mam claude code a windows notebook. rozpocet par stovek korun mesicne.

---

## Asistent — krok 1 + 2 (shrnutí, hloubka, kolo 1)

Rozumím tomu takhle: chceš mobilní aplikaci pro iPhone i Android, do které
si lidé zapisují tréninky, sama si stáhne data z chytrých hodinek
a prodávat ji budeš jako předplatné — postavenou ve vibe codingu přes Claude
Code na Windows notebooku, s provozem za pár stovek korun měsíčně. Pokud je
to jinak, oprav mě.

Pojedu v **rychlé hloubce (quick)**: jedno kolo otázek, pak rešerše 2–3
kandidátů na 3–4 vrstvách, na kterých nejvíc záleží (aplikace, napojení
hodinek, předplatné, backend) — zhruba 10–15 vyhledávání, asi 20–30 minut.
Kdybys chtěl důkladnější srovnání všech vrstev (standard), řekni.

Než začnu, pár otázek. U každé je v závorce výchozí odpověď — stačí napsat
„ok“, nebo třeba „2: jen Android, zbytek ok“.

1. **Co přesně aplikace sleduje a pro koho?**
   *[Výchozí: deník silových i kardio tréninků pro rekreační sportovce,
   česky (ČR/SK); tep a aktivity se načtou z hodinek automaticky, posilovna
   se zapisuje ručně]*
2. **Telefony a hodinky** — iPhone, Android, nebo obojí? Které hodinky?
   *[Výchozí: iPhone i Android; hodinky přes Apple Health (Apple Watch,
   Garmin…) a Health Connect na Androidu (Garmin, Samsung, Pixel Watch…),
   ne přímé napojení každé značky zvlášť]*
3. **Kolik uživatelů** při spuštění a za rok?
   *[Výchozí: do ~300 aktivních v prvních měsících, z toho ~30 platících;
   za rok ~2 000 aktivních a ~200 platících; veřejně v obchodech]*
4. **Rozpočet** — měsíčně píšeš pár stovek. Je to tvrdý strop? A kolik
   jednorázově?
   *[Výchozí: strop 500 Kč měsíčně na provoz; jednorázově do ~3 000 Kč
   (poplatky App Store a Google Play); nové zařízení jen když bude nutné]*
5. **Termín a ambice** — kdy první použitelná verze a co má ověřit?
   *[Výchozí: verze pro testery do 2–3 měsíců, cílem ověřit, jestli za to
   lidi zaplatí; ostrý prodej až potom]*
6. **Omezení a citlivá data** — tep a tréninky jsou podle GDPR zdravotní
   údaje. Kde jsou uživatelé, kde mají být data, jsou mezi nimi děti,
   potřebuješ offline režim?
   *[Výchozí: uživatelé v ČR/SK (EU), data uložená v EU, žádní uživatelé
   pod 16 let; zápis bez signálu by byl fajn, ale není podmínka]*
7. **Priority** — seřaď aspoň tři: nízká cena · rychlost spuštění ·
   jednoduchost / málo dodavatelů · škálovatelnost · žádný vendor lock-in ·
   výkon · špičková kvalita · ochrana dat.
   *[Výchozí: 1. nízká cena, 2. rychlost spuštění, 3. jednoduchost / málo
   dodavatelů]*
8. **Preference** — nástroje nebo služby, které už používáš a máš rád,
   účty, které už máš (GitHub, vývojářský účet Apple / Google…), něco, čemu
   se chceš vyhnout? „Žádné“ je úplně v pořádku.
   *[Výchozí: žádné preference; máš Claude Code, jiné účty zatím ne]*

## Uživatel **[SIMULOVÁNO]**

> ok

---

## Asistent — průběh (krok 4)

Díky, beru všechny výchozí odpovědi. Pouštím se do rešerše: nejdřív trh
a konkurence, pak aplikace, hodinky, předplatné a backend.

*(Kontrolní bod trhu: verdikt vyšel **hybrid** — stavět vlastní aplikaci,
komoditní části koupit jako služby. Nákup hotového řešení nevyhrál, takže
podle pravidla se rešerše nezastavuje a uživatel se neptá.)*

---

## Asistent — krok 8 (shrnutí a nastavení projektu)

Hotovo, tady je výsledek. Podrobnosti budou v repozitáři v `docs/blueprint/`.

**Doporučený balík (Balanced)**
- **Aplikace:** Expo (React Native, TypeScript) — jeden kód pro iPhone
  i Android; AI v TypeScriptu píše nejspolehlivěji; iOS se staví v cloudu,
  Mac nepotřebuješ
- **Hodinky:** Apple Health (HealthKit) + Health Connect přímo v telefonu —
  zdarma, pokryje Apple Watch, Garmin, Samsung a další přes jejich
  synchronizaci
- **Předplatné:** nákupy v App Store / Google Play přes RevenueCat — zdarma
  do 2 500 $ měsíčních tržeb
- **Backend, databáze, přihlášení:** Supabase (Postgres, region EU) — Free
  při vývoji, Pro (25 $) od prvních platících
- **Buildy a vydávání:** EAS (Free) + App Store + Google Play
- **Hlášení chyb:** Sentry Free (potvrdí se v úkolu T106)
- **Vývoj:** GitHub (soukromý repozitář), GitHub Actions, Claude Code

Varianty: **Budget** = Supabase Free i po spuštění (bez záloh — u zdravotních
dat platících lidí nedoporučuji). **Top** = navíc agregátor Terra pro 500+
zařízení a plná data z Garminu (~8 700 Kč měsíčně, mimo rozpočet).

**Tři největší rizika**
1. **Zdravotní data (GDPR, čl. 9):** potřebuješ výslovný souhlas, data
   v EU a posouzení vlivu (DPIA). Kód napíše AI a nikdo ho nebude číst
   řádek po řádku — před placeným spuštěním doporučuji profesionální
   bezpečnostní a GDPR revizi.
2. **Schválení v obchodech:** HealthKit, Health Connect, předplatné, smazání
   účtu a štítky soukromí — zamítnutí může spuštění zdržet o týdny.
3. **Přeplněný trh:** Hevy stojí 2,99 $ a Strong 4,99 $ měsíčně a umí toho
   hodně. Zatím nemáš doložené odlišení. Navíc Garmin posílá do Apple Health
   u tréninků jen minimum a maximum tepu.

**Náklady** (kurz ČNB 21,732 Kč/$ k 15. 10. 2026)
- vývoj: 0–179 Kč měsíčně (179 Kč = rozpočítaný roční poplatek Apple)
- spuštění (300 aktivních, 30 platících): **~722 Kč měsíčně (~33 $)** —
  o ~220 Kč nad stropem 500 Kč kvůli Supabase Pro; pokryje to zhruba
  7 předplatitelů po 99 Kč. Do té doby zůstaneš na 0–179 Kč.
- růst (2 000 aktivních, 200 platících): ~1 135 Kč měsíčně (~52 $)
- k tomu 15 % provize obchodů z tržeb; jednorázově Google Play 543 Kč,
  Apple 2 151 Kč ročně

**Hardware:** Windows notebook stačí. Na testování potřebuješ skutečný
iPhone a Android telefon (stačí půjčené nebo použité) a hodinky.

**Jazyk dokumentů:** čeština. Názvy souborů, kód, ID úkolů a commit messages
zůstávají anglicky (konvence) — kdybys chtěl commity česky, řekni.

**Nastavení projektu** — u každého bodu je výchozí volba, stačí „ok“:

1. **Kam projekt založit** — *[Výchozí: `./fitness-app`]*
2. **Git** — inicializovat repozitář? *[Výchozí: ano]*
3. **GitHub** — vytvořit **soukromý** repozitář? *[Výchozí: ano, soukromý —
   jen pokud je `gh` přihlášený; jinak dám ruční postup do `SETUP.md`]*
4. **Kolik dokumentace a údržby** —
   - *minimal*: jen to nejnutnější, nejlevnější na tokeny; dokumentace
     časem zastará, pokud ji občas neosvěžíš
   - *balanced*: rozumný střed, k tomu model hrozeb a šablony PR
   - *complete*: všechno, co placený produkt potřebuje — provozní příručka,
     soubor pro každý úkol, kontrolní seznam vydání a pomocný agent, který
     před každým commitem projde dokumentaci. Dokumentace zůstane
     důvěryhodná, ale každý úkol stojí znatelně víc tokenů a jde pomaleji.
   *[Výchozí: **complete** — placený produkt se zdravotními daty]*
5. **Jak má agent pracovat (režim autonomie)** — tohle je tvoje rozhodnutí
   a nastavuješ ho ty v Claude Code, repozitář ho jen zapíše:
   - *1 — pod dohledem:* ptá se před každou úpravou a příkazem; nejpomalejší
   - *2 — semi-auto:* úpravy souborů v projektu bere sám, příkazy jen ze
     schváleného seznamu (testy, kontrola kódu, git), na ostatní se ptá
   - *3 — auto v sandboxu:* skoro nic se neptá, ale běží izolovaně
     (kontejner / VM) bez produkčních klíčů
   - *4 — plně auto na tvém počítači:* nic se neptá, má přístup ke všem
     tvým souborům a přihlášením — **nedoporučuji**
   Rizika vyšších úrovní: agent čte cizí texty (webové stránky, README
   balíčků, chybové výstupy) a ty ho mohou navést ke spuštění příkazu
   (prompt injection); chyby se dějí rychlostí stroje (smazané složky,
   přepsaný kód); na tvém počítači může přečíst `.env`, klíče a přihlášení;
   smyčka volající placenou službu účtuje, i když spíš; automatické
   instalace stahují kód od cizích lidí. Brzdí to git historie, chráněná
   větev `main`, omezené tokeny, stropy útraty a žádné produkční klíče na
   notebooku. Pravidla v `AGENTS.md` model žádají, ale nevynucují — vynucuje
   to až nastavení nástroje.
   *[Výchozí: **2 — semi-auto**; při zakládání účtů a klíčů (fáze 0) raději
   1. Přidám i `.claude/settings.json`, který povolí ověřovací příkazy
   a zakáže čtení `.env` — ano?]*
6. **Git workflow** — *A:* commit rovnou do `main` po každém úkolu; *B:*
   větev na každý úkol + pull request, CI musí projít, mergeuješ ty.
   *[Výchozí: **B** — sedí k profilu complete]*
7. **Licence** — repozitář bude soukromý, takže licence není potřeba;
   do README dám „© vlastník, všechna práva vyhrazena“. *[Výchozí: ok]*

## Uživatel **[SIMULOVÁNO]**

> ok — umístění: `./fitness-app`; git ano; GitHub repozitář **ne**; zbytek ok (complete, režim 2 + `.claude/settings.json`, workflow B, bez licence).

---

## Asistent — krok 11 (předání)

Hotovo. Projekt je v
`./fitness-app`
s lokálním gitem a prvním commitem. GitHub repozitář jsem nevytvářel —
ruční postup je v `docs/blueprint/SETUP.md`, krok 2 (soukromý repozitář,
`git remote add origin …`, `git push -u origin main`).

**Co kde najdeš**
- `AGENTS.md` — pravidla pro agenta (stack, bezpečnost, tvrdé brány,
  režim autonomie, údržba dokumentace); `CLAUDE.md` — krátký výtah,
  který Claude Code načte vždycky
- `docs/blueprint/` — celá rešerše: `PLAN.md` (shrnutí a předpoklady),
  `DECISIONS.md` (srovnání a proč), `COSTS.md`, `RISKS.md`, `SETUP.md`,
  `SOURCES.md` a další
- `docs/backlog/QUEUE.md` — 15 úkolů ve třech fázích, každý má vlastní
  soubor s kritérii přijetí
- `SECURITY.md` — bezpečnostní pravidla a co dělat, když něco unikne

**Co udělat jako první**
1. Otevři v té složce terminál, spusť `claude` a napiš: **„Začni úkolem T001“**.
   T001 je založení účtů s MFA a stropy útraty — to děláš ty, agent tě vede.
2. Pak T002 (kostra aplikace a CI) a dál podle backlogu.

**Režim autonomie nastavuješ ty, ne agent.** Dohodli jsme úroveň 2
(semi-auto). V Claude Code:
- během sezení přepínáš režimy **Shift+Tab** (Manual → Accept edits → …);
  aktuální režim vidíš pod promptem. Pro fázi 0 nech *Manual*, pro práci
  na funkcích přepni na *Accept edits* (`acceptEdits`);
- natrvalo: do `~/.claude/settings.json` přidej
  `"permissions": { "defaultMode": "acceptEdits" }`;
- pravidla zobrazíš a upravíš příkazem `/permissions`. V projektu už je
  `.claude/settings.json` s povolenými ověřovacími příkazy a zákazem čtení
  `.env`.
- `bypassPermissions` (úroveň 4) nepoužívej — podle dokumentace jen
  v izolovaném kontejneru nebo VM.

Docs profil i režim můžeš kdykoli změnit — stačí to říct agentovi a on
upraví `AGENTS.md`.

**Co stojí za to přehodnotit, až budou skuteční uživatelé nebo čísla**
- data z hodinek: když testeři s Garminem budou chtít průběh tepu a GPS →
  přímé API Garminu nebo agregátor (Terra)
- Supabase Free → Pro: od ~7 platících předplatitelů
- cena předplatného (99 Kč) a odlišení od Hevy / Strong — po betě
- RevenueCat: až tržby přesáhnou 2 500 $ měsíčně, porovnat s Adapty
- EAS: když nebude stačit 15 buildů měsíčně nebo 1 000 uživatelů pro
  aktualizace

Ještě jednou to hlavní: před prvními platícími uživateli nech zdravotní
data, zabezpečení databáze a texty souhlasu zkontrolovat odborníkem
(úkol T201). Tohle verze postavená AI sama nepokryje.
