# Architektura

## Přehled systému

```mermaid
flowchart TB
  user["Uživatel s telefonem a hodinkami"]
  subgraph phone["Telefon"]
    app["Aplikace Expo (React Native, TypeScript)"]
    health["Apple HealthKit / Health Connect"]
    secure["Zabezpečené úložiště (session)"]
  end
  subgraph supabase["Supabase (region EU)"]
    auth["Supabase Auth"]
    db["Postgres + RLS"]
    fn["Edge Functions (webhook předplatného, smazání účtu)"]
  end
  rc["RevenueCat"]
  stores["App Store / Google Play"]
  sentry["Sentry (hlášení chyb)"]
  eas["EAS Build / Submit / Update"]

  user --> app
  health -->|"tréninky a tep po souhlasu"| app
  app --> secure
  app -->|"přihlášení"| auth
  app -->|"čtení a zápis vlastních dat"| db
  app -->|"nákup předplatného"| rc
  rc --> stores
  rc -->|"webhook"| fn
  fn --> db
  app -->|"chyby bez zdravotních dat"| sentry
  eas -->|"buildy a aktualizace"| stores
```

Aplikace v telefonu čte data z Apple Health / Health Connect až po
výslovném souhlasu a ukládá je spolu s ručně zapsanými tréninky do Supabase.
Řádková bezpečnost (RLS) v Postgresu pustí každého uživatele jen k jeho
řádkům. Předplatné kupuje uživatel v obchodě přes RevenueCat; RevenueCat
pošle změnu stavu webhookem do Edge Function, která ji zapíše do databáze.
Buildy a vydání do obchodů obstarává EAS.

## Hlavní tok

```mermaid
sequenceDiagram
  actor U as Uživatel
  participant A as Aplikace
  participant H as HealthKit / Health Connect
  participant S as Supabase
  participant R as RevenueCat
  U->>A: Přihlášení
  A->>S: Supabase Auth
  S-->>A: Session (JWT)
  U->>A: Výslovný souhlas se zdravotními údaji
  A->>H: Žádost o oprávnění ke čtení
  H-->>A: Tréninky a tep
  A->>S: Uložení tréninku (RLS podle uživatele)
  U->>A: Ruční zápis sérií
  A->>S: Uložení sérií
  U->>A: Koupě předplatného
  A->>R: Nákup přes obchod
  R->>S: Webhook se stavem předplatného
  S-->>A: Odemčené funkce
```

## Vývojový postup

```mermaid
flowchart LR
  task["Úkol z QUEUE.md"] --> branch["Větev txxx-nazev"]
  branch --> agent["Claude Code implementuje"]
  agent --> ci["CI: lint, typecheck, testy, audit, gitleaks"]
  ci --> preview["EAS preview build na testovací telefon"]
  preview --> review["Kontrola vlastníkem a merge PR"]
  review --> prod["EAS produkční build a odeslání do obchodů"]
```

## Prostředí

| Prostředí | Účel | Kde běží | Data |
|-----------|------|----------|------|
| development | vývoj a testy na vlastním telefonu | development build z EAS + Supabase projekt `dev` (Free) | jen testovací data |
| preview | testeři (TestFlight / interní testování Google Play) | EAS preview build + Supabase `dev` | testovací data testerů, bez produkčních |
| production | platící uživatelé | obchody + Supabase projekt `prod` (Pro, EU) | skutečná zdravotní data — agent sem nemá přístup |
