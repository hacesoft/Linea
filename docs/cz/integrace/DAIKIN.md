[🇨🇿 Česky](DAIKIN.md) | 🇬🇧 English *(bude doplněno)*

---

# Samostatný projekt: Daikin Onecta pro Node-RED

> Tento dokument je připraven jako základ README pro budoucí samostatný repozitář. Modul není nutnou součástí LINEA.

## 1. Účel

Modul komunikuje s Daikin Onecta Cloud API a poskytuje:

- OAuth2 autentizaci;
- správu access/refresh tokenů;
- periodické čtení zařízení;
- parsování stavů klimatizací;
- ovládání;
- Dashboard 2.0;
- lokální konfiguraci.

Lze jej použít:

```text
samostatný Node-RED
```

nebo:

```text
LINEA + Daikin modul
```

## 2. Vnitřní struktura modulu

Aktuální modul je logicky rozdělen do skupin:

```text
MODULE::DAIKIN::KLIMATIZACE
├── načtení tokenů
├── Token manager
├── token refresh
├── polling
├── DAIKING::CONFIG
├── DAIKING::LOAD_CERT
└── DAIKING::UI
```

## 3. Lokální soubory

Modul používá:

```text
daikin_config.json
daikin_tokens.json
```

`daikin_config.json` obsahuje konfiguraci OAuth klienta, například Client ID, Client Secret a polling interval.

`daikin_tokens.json` obsahuje tokeny. Tento soubor je citlivý a **nesmí být commitnut do veřejného GitHub repozitáře**.

## 4. Registrace v Daikin Developer Portal

Daikin Developer Portal:

https://developer.cloud.daikineurope.com/

Přihlaste se účtem, který odpovídá vašemu Onecta účtu.

V portálu vytvořte novou aplikaci (`My Apps` → `New App`). Zvolte Onecta/OIDC autentizaci, pokud ji portal nabízí, a nastavte Redirect URI.

Po vytvoření aplikace získáte:

```text
Client ID
Client Secret
```

Client Secret bezpečně uložte. U některých implementací/portálových workflow se secret zobrazí pouze při vytvoření.

## 5. Redirect URI

Redirect URI zaregistrované v Daikin portálu se musí **přesně shodovat** s URI použité při autorizaci i při výměně authorization code za token.

Pro současný ruční postup může být použita vlastní HTTPS callback adresa. Flow UI historicky uvádělo jako jednoduchý příklad:

```text
https://example.com/callback
```

Důležité není, zda na adrese běží aplikace, pokud pouze ručně odečítáte `code` z adresního řádku; důležité je, aby URI byla při všech OAuth krocích shodná.

Pro produkční samostatný modul je vhodnější doplnit vlastní callback endpoint a celý první OAuth handshake automatizovat.

## 6. Získání prvního authorization code

Autorizační endpoint používaný Onecta integracemi:

```text
https://idp.onecta.daikineurope.com/v1/oidc/authorize
```

Sestavte URL s:

```text
response_type=code
client_id=CLIENT_ID
redirect_uri=REDIRECT_URI
scope=openid onecta:basic.integration
```

Koncept:

```text
https://idp.onecta.daikineurope.com/v1/oidc/authorize
 ?response_type=code
 &client_id=CLIENT_ID
 &redirect_uri=URL_ENCODED_REDIRECT_URI
 &scope=openid%20onecta:basic.integration
```

Otevřete URL v prohlížeči, přihlaste se k Onecta a potvrďte přístup.

Daikin následně přesměruje prohlížeč na Redirect URI s parametrem:

```text
?code=...
```

I když cílová stránka neexistuje, authorization code může být vidět v adresním řádku.

Authorization code je krátkodobý a jednorázový.

## 7. První výměna code → tokeny

Token endpoint:

```text
https://idp.onecta.daikineurope.com/v1/oidc/token
```

Příklad:

```bash
curl -X POST "https://idp.onecta.daikineurope.com/v1/oidc/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code" \
  -d "client_id=CLIENT_ID" \
  -d "client_secret=CLIENT_SECRET" \
  -d "code=AUTHORIZATION_CODE" \
  -d "redirect_uri=REDIRECT_URI"
```

Z odpovědi potřebujeme především:

```text
access_token
refresh_token
expires_in
```

Do Node-RED konfigurace se při prvním nastavení vloží `refresh_token`.

## 8. Co se děje po vložení refresh tokenu

```text
Refresh Token
      │
      ▼
Token manager
      │
      ▼
POST /v1/oidc/token
grant_type=refresh_token
      │
      ▼
nový access_token
      │
      ├──► API requests
      │
      └──► expirace
```

Pokud Daikin vrátí nový `refresh_token`, modul jej musí okamžitě uložit. Refresh token může rotovat.

## 9. Startup synchronizace


```text
daikin_tokens.json ──► refresh
daikin_config.json ──► config
```

Proto mohl refresh proběhnout dříve, než byl načten Client ID/Secret.

Opravená architektura:

```text
Nacti tokens do kontextu ─┐
                          ├──► Daikin startup sync ──► Token manager
Nacti config do kontextu ─┘
```

`Daikin startup sync` čeká na:

```text
Client ID
Client Secret
Refresh Token
```

a teprve potom spustí refresh.

## 10. Lock proti dvojímu refreshi

Používá se:

```text
global.daikinRefreshInFlight
```

Důvodem je rotace refresh tokenu. Dva paralelní refresh requesty se stejným tokenem mohou způsobit chyby.

Lock se musí uvolnit:

- po úspěchu;
- po chybě;
- případně timeoutem, pokud request nedoběhne.

## 11. Periodický refresh

Současný modul používá periodický refresh přibližně po 2,5 hodinách.

Access token se nepovažuje za platný těsně do poslední sekundy; před expirací se nechává bezpečnostní rezerva.

## 12. Polling zařízení

API endpoint používaný flow:

```text
GET https://api.onecta.daikineurope.com/v1/gateway-devices
```

Header:

```text
Authorization: Bearer ACCESS_TOKEN
```

Polling interval je konfigurovatelný. Současné UI nastavuje minimum 7 minut a upozorňuje na API rate limit. Pro samostatný projekt doporučujeme limit a doporučený interval vždy ověřit proti aktuálním podmínkám Daikin Developer Portalu.

## 13. HTTP 401

Při 401 modul:

- označí access token jako expirovaný/neplatný;
- nesmí pokračovat s nekonečným rychlým pollingem;
- má vyvolat řízený refresh;
- při opakovaném selhání má zobrazit diagnostickou chybu.

## 14. `Client authentication failed`

Kontrolovat:

- Client ID;
- Client Secret;
- zda patří ke stejné aplikaci;
- zda se při startu načetl `daikin_config.json`;
- zda neběží dva refresh requesty současně;
- zda nebyla aplikace v Developer Portalu změněna nebo zrušena.

## 15. `invalid_grant`

Typicky kontrolovat:

- refresh token;
- rotaci tokenu;
- zda nebyl použit starý refresh token;
- zda authorization code nebyl již použit;
- shodu Redirect URI;
- případnou potřebu nové autorizace.

## 16. Bezpečnost

Nikdy nezveřejňovat:

```text
Client Secret
access_token
refresh_token
authorization code
```

Do ukázkového flow patří pouze prázdné hodnoty/placeholders.

## 17. Budoucí vylepšení samostatného projektu

Doporučeno:

- automatický OAuth callback node;
- tlačítko „Autorizovat Daikin“;
- odstranění nutnosti ručního `curl`;
- jednoznačný stav OAuth v UI;
- export bez secrets;
- samostatný changelog;
- test rate-limit chování;
- retry/backoff pro cloud chyby.

---

[← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
