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


## Verze a kompatibilita

```text
API version: 1.0.0
schema:      1
path:        /api/v1/...
```

Klient se má při kompatibilitě orientovat především podle `schema`. Bugfix implementace může změnit `version` bez změny datového schématu. Nekompatibilní změna veřejné struktury vyžaduje zvýšení schema; nový API path má smysl až podle rozsahu skutečné změny.

1.0.0 je uzavřená první produkčně použitelná verze pro monitorovací účely. Bez konkrétní potřeby klienta se API nemá kosmeticky měnit.

---

[← LINEA API](PREHLED.md) · [← Hlavní dokumentace](PREHLED.md)
