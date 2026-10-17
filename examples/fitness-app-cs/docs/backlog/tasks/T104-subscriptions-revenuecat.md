# T104 — Předplatné přes RevenueCat (sandbox) a webhook

**Fáze:** 1 — První verze | **Priorita:** P0 | **Stav:** ready | **Závisí na:** T101

## Cíl

Měsíční a roční předplatné, které odemyká placené funkce — bezpečně
a otestované v sandboxu.

## Kroky

1. Vlastník: produkty v App Store Connect a Google Play Console (99 Kč
   měsíčně, 790 Kč ročně — předpoklad), projekt a entitlement v RevenueCat.
2. Prověř SDK (`react-native-purchases` — ověřit název a údržbu), přidej,
   nový development build.
3. Paywall a obnovení nákupu.
4. Edge Function pro webhook: ověření autorizační hlavičky, idempotentní
   zápis stavu do `subscriptions` (RLS: uživatel jen čte svůj stav).
5. Placené funkce kontrolovat i na serveru.
6. Testy webhooku: opakované a podvržené požadavky.

## Kritéria přijetí

- [ ] Nákup, obnova a zrušení fungují v sandboxu na obou platformách
- [ ] Podvržený webhook je odmítnut (test)
- [ ] `cd mobile && npm run lint && npm run typecheck && npm test` prochází

## Poznámky

Tvrdá brána: logika předplatného. `STACK-FIT.md`, `THREAT-MODEL.md` H3,
[S10][S13].
