[🇨🇿 **Česky**](DAIKIN.md) | [🇬🇧 English](../../en/integrations/DAIKIN.md)

---
# Daikin Onecta modul

## 1. Účel a stav
Volitelný modul čte stav klimatizací z Daikin Onecta Cloud API pomocí OAuth 2.0 Authorization Code flow. LINEA CORE na něm není závislá. Současný Daikin flow je stále ve vývoji; dokumentace popisuje aktuální provozní princip, nikoli definitivní wiring.

## 2. Aktuální endpointy
```text
Authorization: https://idp.onecta.daikineurope.com/v1/oidc/authorize
Token:         https://idp.onecta.daikineurope.com/v1/oidc/token
Devices:       https://api.onecta.daikineurope.com/v1/gateway-devices
```
Starší endpointy `user.onecta.daikin.eu/oauth/token` a `/v1/gateway/devices` neodpovídají současné implementaci.

## 3. První OAuth autorizace
1. Přihlaste se do Daikin Developer Portalu účtem odpovídajícím Onecta účtu.
2. Vytvořte integrační/OIDC aplikaci a zaregistrujte Redirect URI.
3. Uložte Client ID a Client Secret.
4. Otevřete:
```text
https://idp.onecta.daikineurope.com/v1/oidc/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=REDIRECT_URI&scope=openid%20onecta:basic.integration
```
5. Po přihlášení zkopírujte `code` z výsledné Redirect URI.
6. Vyměňte jej za tokeny:
```bash
curl -X POST 'https://idp.onecta.daikineurope.com/v1/oidc/token' \
 -d 'grant_type=authorization_code' \
 -d 'client_id=CLIENT_ID' \
 -d 'client_secret=CLIENT_SECRET' \
 -d 'code=AUTHORIZATION_CODE' \
 -d 'redirect_uri=REDIRECT_URI'
```
Redirect URI musí být ve všech krocích přesně shodná.

## 4. Token manager
`refresh_token` bezpečně uložte. Access token se obnovuje přes stejný token endpoint s `grant_type=refresh_token`. Pokud server vrátí nový refresh token, musí nahradit starý – refresh token je rotační. `global.daikinRefreshInFlight` brání paralelnímu refreshi. Client Secret ani tokeny se nesmí commitovat, veřejně debugovat ani publikovat přes LINEA API.

## 5. Konfigurace
Běžná konfigurace směřuje do `global.config.daikinConfig` (`clientId`, `clientSecret`, `pollMinutes`). Tokeny jsou provozní tajemství s jiným životním cyklem. Pracovní flow může ještě obsahovat přechodové části staršího ukládání; ty nejsou považovány za definitivní architekturu.

## 6. Čtení a veřejný stav

## 7. Polling
Cílové nastavení používá `pollMinutes` s minimem **8 minut**. Aktuální pracovní flow může ještě obsahovat starší pevný polling; při dokončení Daikin modulu se má sjednotit na jediný konfigurovatelný mechanismus.

## 8. Troubleshooting
`čekám na Client ID/Secret` znamená, že aktivní `global.config.daikinConfig` obě hodnoty neposkytuje. `refresh uložen | access vypršel` znamená uložený refresh token, ale nepoužitelný access token; pro obnovu jsou stále nutné platné Client ID/Secret. Kontrolujte přesnou Redirect URI, platnost authorization code a nejnovější rotační refresh token.

## 9. Budoucí autorizační průvodce
Samostatný pomocný flow může uživatele provést vytvořením URL, vložením `code`, výměnou za tokeny, bezpečným uložením refresh tokenu a testem `gateway-devices`.

---
[← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
