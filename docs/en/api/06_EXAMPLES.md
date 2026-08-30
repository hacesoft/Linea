[🇨🇿 Česky](../../cz/api/06_PRIKLADY.md) | [🇬🇧 English](06_EXAMPLES.md)

---

# Response examples

## Health

```json
{"name":"LINEA API","version":"1.0-r2.3.3","schema":3,"readOnly":true}
```

## Fresh main snapshot

```json
{"system":{"timestamp":"2026-08-30T08:33:59.533Z","sourceTimestamp":"2026-08-30T08:33:49.325Z","ageMs":10208,"stale":false}}
```

## Optional module

Available: `{"available": true, "data": {}}`

Unavailable: `{"available": false, "data": null}`

## MPPT string

```json
{"name":"MPPT WEST","powerW":1295,"pvVoltageV":64.13,"pvCurrentA":20.2,"yieldTodayKWh":1.7}
```

## Strategy time

```json
{"delayCharging":{"start":"04:30","stop":"11:00","startMs":16200000,"stopMs":39600000}}
```

The final R2.3.3 operational test confirmed API version/schema/read-only status, the approximately 30-second stale threshold, PV-string mapping including yield, and working VRM, temperatures, Shelly I/O, smoke, UPS, Daikin, ESS, forecast, solar and weather sections.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)
