[🇨🇿 Česky](01_PREHLED_A_ARCHITEKTURA.md) | [🇬🇧 English](../../en/api/01_OVERVIEW_AND_ARCHITECTURE.md)

---

# Přehled a architektura

## Účel

API je malá read-only integrační vrstva nad existujícími LINEA globals a hotovými snapshoty. Prvním klientem bude Nextcloud LINEA Monitor & Analytics, ale stejné rozhraní může využít externí displej, Home Assistant, Grafana nebo jiné monitorovací řešení.

```text
Victron / Modbus ─┐
Shelly / MQTT ────┤
UPS / NUT ────────┤
Daikin Cloud ─────┤──> LINEA Node-RED ──> LINEA API (READ-ONLY) ──> klient
VRM API ──────────┤                         GET /api/v1/status
Weather/Forecast ─┘
```

LINEA zůstává zdrojem aktuální pravdy a řídicí logiky. API nesmí znovu počítat ESS rozhodnutí, měnit registry, ovládat měniče, baterii ani jiná zařízení.

## Bezpečnostní princip

Do veřejné odpovědi se nesmí exportovat citlivé nebo zbytečně interní údaje, zejména Daikin/VRM tokeny, klientské secrety, interní IP adresy, Shelly MQTT ID, nepotřebné Modbus Unit ID a nepotřebná sériová čísla. `api.readOnly` je explicitně `true`.

## Verze a kompatibilita

```text
API version: 1.0-r2.3.3
schema:      3
path:        /api/v1/...
```

Klient se má při kompatibilitě orientovat především podle `schema`. Bugfix implementace může změnit `version` bez změny datového schématu. Nekompatibilní změna veřejné struktury vyžaduje zvýšení schema; nový API path má smysl až podle rozsahu skutečné změny.

R2.3.3 je uzavřená první produkčně použitelná verze pro monitorovací účely. Bez konkrétní potřeby klienta se API nemá kosmeticky měnit.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
