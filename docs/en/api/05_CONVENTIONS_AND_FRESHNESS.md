# Conventions and freshness

## Signs and units

Grid power: positive = import, negative = export. Battery power: positive = charging, negative = discharging. Power is W and energy is kWh. `units` maps numeric paths to units; it is not a validator or a complete type description.

## Main-state freshness

`system.timestamp` is response creation time; `system.sourceTimestamp` is when AC LOAD creates `lineaDecisionState`. `system.ageMs` uses server time and clamps a negative difference to zero. `/status` becomes stale at **> 30,000 ms**, `/health` at **> 5,000 ms**. An invalid timestamp produces null age and `stale: true`.

A fresh AC LOAD snapshot can contain cached fallback values. Its timestamp does not prove the last physical measurement time of each register. HTTP 200 does not confirm freshness or device health.

## Modules and `available`

Optional-module availability tests whether a cached object exists. It does not prove connectivity or freshness. Check module `updatedAt` or device `lastSeen`; missing time means unknown age. `shelly.data.updatedAt` may describe only the most recently updated Shelly section. Stored smoke `ageSec` may not advance on every HTTP request; calculate ongoing age from `lastSeen`.

Successful status always sets `energy.available: true`. Forecast tests `success === true`; SPOT tests whether a price or price array exists. None of these flags independently validates freshness or completeness. There is no uniform per-module freshness contract.

## Zero and missing values

Clients must distinguish zero from null. However, `finiteOrNull` uses `Number(v)`, which converts null, empty strings and false to **0**. Some AC LOAD inputs also substitute cached values or zero. A returned zero is therefore not always a verified physical measurement.

## Time series

SPOT maps array indices to `hour`, declares `intervalMinutes: 60` and assigns dates using server local time. It does not guarantee 24 samples or explicitly handle 23/25-hour days. Check dates, lengths and the upstream interval before cost calculations. Forecast declares 15-minute intervals, converts Wh to kWh and does not interpolate missing points.

## Compatibility

Check `api.schema === 1`, tolerate additional fields and missing optional values. A structural or semantic change may require a new schema. Despite their location under `vrm.data.today`, battery energy counters accumulate since reset and are not daily counters.


[Česky](../../cz/api/05_KONVENCE_A_STARI_DAT.md) · [← LINEA API](README.md) · [← LINEA](../README.md)
