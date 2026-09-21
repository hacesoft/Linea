# Daikin ONECTA — zapojení do LINEA

Úplný postup registrace, vytvoření aplikace, získání prvních OAuth tokenů, nastavení modulu a řešení chyb je v [node-red-daikin](https://github.com/hacesoft/node-red-daikin).

Modul čte klimatizace přes ONECTA Cloud API a zobrazuje jejich provozní stav, teploty a dostupnou spotřebu v Dashboardu 2.0. LINEA CORE na modulu není závislá.

## Konfigurace a interval

Běžné nastavení používá `global.config.daikinConfig`: `clientId`, `clientSecret`, `pollMinutes`. Polling nastavte na **8 minut**, interval je konfigurovatelný s minimem 8 minut. Access/refresh tokeny mají vlastní životní cyklus a nesmějí být součástí veřejného exportu ani API.

Při importu propojte inicializaci konfigurace, token manager a ukládání souborů podle návodu modulu. Ověřte cesty `daikin_config.json` a souboru tokenů, jejich zapisovatelnost a obnovu po restartu. Nestačí předpokládat, že obecné tlačítko uložení LINEA obslouží všechny soubory samostatného modulu.

## Napojení API

Parser vytváří `global.lineaApiClimateState`; hlavní API jej publikuje jako `climate.data`. Pokud stav chybí, blok je nedostupný. Nový hlavní energetický snapshot neznamená nové cloudové měření — kontrolujte `updatedAt`. Provozní režim klimatizace sám o sobě nedokazuje běh kompresoru.

Při potížích nejprve ověřte Client ID/Secret, shodu Redirect URI a aktuální rotační refresh token podle samostatného návodu.

[English](../../en/integrations/DAIKIN.md) · [← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
