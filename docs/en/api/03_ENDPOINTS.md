# Endpoints

## `GET /api/v1/health`

Checks whether the main state exists, not the health of every device. Returns HTTP **200** when `global.lineaDecisionState` is truthy, otherwise **503**. `status: "ok"` can coexist with `stale: true`.

```json
{
  "api": {
    "name": "LINEA API",
    "version": "1.0.0",
    "schema": 1,
    "readOnly": true
  },
  "status": "ok",
  "sourceAgeMs": 1000,
  "stale": false,
  "timestamp": "2026-09-21T12:00:00.000Z"
}
```

`sourceAgeMs` is the source timestamp age. `stale` is true when age cannot be determined or is **greater than 5,000 ms**. A 503 uses the same shape with `status: "initializing"`, `sourceAgeMs: null`, `stale: true`.

## `GET /api/v1/status`

Returns HTTP **200** with `api`, `system`, `units`, `conventions`, `energy`, `ess`, `spot`, `forecast`, `solar`, `weather`, `vrm`, `temperatures`, `shelly`, `ups`, `climate`. The contract is **version 1.0.0, schema 1**.

`system.stale` is true when age is unknown or **greater than 30,000 ms**. Stale snapshots still return HTTP 200. This does not confirm a successful Modbus write or the freshness of every sensor.

If the main snapshot or its `timestamp` is missing, the response is HTTP **503**:

```json
{
  "api": {
    "name": "LINEA API",
    "version": "1.0.0",
    "schema": 1,
    "readOnly": true
  },
  "status": "initializing",
  "timestamp": "2026-09-21T12:00:00.000Z"
}
```

A 503 has no `system`, `units` or energy blocks. `/status` sets `Cache-Control: no-store`; the reference `/health` function does not.

`vrm`, `temperatures`, `shelly`, `ups` and `climate` use `{available, data}` wrappers. Other sections have their own structure. Availability alone proves neither freshness nor live connectivity. Missing optional modules normally do not block the whole status response.

## Scope

These endpoints are read-only. There is no `/set`, `/control`, `/write` or history endpoint. Paths are relative to the Node-RED HTTP root; `httpNodeRoot` or a reverse-proxy prefix may change the public URL.


[Česky](../../cz/api/03_KONCOVE_BODY.md) · [← LINEA API](README.md) · [← LINEA](../README.md)
