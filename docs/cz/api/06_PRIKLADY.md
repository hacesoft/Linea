[🇨🇿 Česky](06_PRIKLADY.md) | [🇬🇧 English](../../en/api/06_EXAMPLES.md)

---

# Příklady odpovědí

## Health

```json
{
  "name": "LINEA API",
  "version": "1.0-r2.3.3",
  "schema": 3,
  "readOnly": true
}
```

## Fresh hlavní snapshot

```json
{
  "system": {
    "timestamp": "2026-08-30T08:33:59.533Z",
    "sourceTimestamp": "2026-08-30T08:33:49.325Z",
    "ageMs": 10208,
    "stale": false
  }
}
```

## Volitelný modul je dostupný

```json
{"available": true, "data": {}}
```

## Volitelný modul je nedostupný

```json
{"available": false, "data": null}
```

## MPPT string

```json
{
  "name": "MPPT WEST",
  "powerW": 1295,
  "pvVoltageV": 64.13,
  "pvCurrentA": 20.2,
  "yieldTodayKWh": 1.7
}
```

## Čas strategie

```json
{
  "delayCharging": {
    "start": "04:30",
    "stop": "11:00",
    "startMs": 16200000,
    "stopMs": 39600000
  }
}
```

## Ověřený provozní test R2.3.3

Test potvrdil `api.version = 1.0-r2.3.3`, `schema = 3`, `readOnly = true`, funkční cca 30s stale hranici a správné mapování PV stringů včetně `yield`. Ověřeny byly také VRM, temperatures, Shelly outputs/inputs, smoke, UPS, Daikin, ESS, forecast, solar a weather.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
