# Zprovoznění LINEA API

## Dostupnost a přístup

API je součástí dodaného LINEA flow, neinstaluje se jako samostatný balíček. V editoru najděte HTTP In uzly `GET /api/v1/health` a `GET /api/v1/status` a ověřte spojení přes Function uzly na HTTP Response. Přítomnost ověřujte podle obsahu importu, nikoli jen data v názvu souboru.

Klient potřebuje síťový přístup k HTTP rozhraní Node-RED. Příklady používají zástupný `LINEA_HOST:1880`; doplňte svou adresu a případný prefix. Pro Nextcloud musí být tato adresa dostupná ze serveru/kontejneru Nextcloudu, nejen z prohlížeče na PC.

```bash
curl -i 'http://LINEA_HOST:1880/api/v1/health'
curl -i 'http://LINEA_HOST:1880/api/v1/status'
```

Nejprve zkontrolujte HTTP kód, potom `api.schema`, stáří dat a dostupnost jednotlivých bloků. HTTP 503 při inicializaci není stejný JSON jako úspěšný status. Klient má při chybě zobrazit nedostupnost a opakovat dotaz s prodlevou; nesmí nahradit chybějící měření nulou.

## Ochrana přístupu

Samotné dvě větve v exportu nemají vlastní API klíč ani autentizační uzel. Ochranu řešte nastavením HTTP endpointů Node-RED nebo reverse proxy, případně přístupem pouze z důvěryhodné sítě/VPN. Přihlášení do editoru samo o sobě není důkazem ochrany těchto endpointů. Flow export neobsahuje kompletní serverové nastavení, proto skutečnou ochranu ověřte v nasazení.

Při vzdáleném přístupu použijte HTTPS. Nezveřejňujte nechráněný Node-RED port. Nepředpokládejte automaticky nastavené CORS. API nevyžaduje, aby klient dostal token VRM, Daikin Client Secret nebo MQTT heslo.

## Sběr dat

API vrací snapshot při HTTP požadavku, nemá WebSocket/SSE stream ani vlastní archiv. Frekvenci dotazování zvolte podle požadovaného zobrazení a zatížení; častější dotaz neurychlí obnovu cloudových dat. Pro historii ukládejte data na straně klienta a zaznamenávejte výpadky.


[English](../../en/api/02_INSTALLATION.md) · [← LINEA API](PREHLED.md) · [← LINEA](../README.md)
