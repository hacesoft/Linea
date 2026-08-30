[🇨🇿 Česky](../../cz/api/03_ENDPOINTY.md) | [🇬🇧 English](03_ENDPOINTS.md)

---

# Endpoints

## `GET /api/v1/health`

Provides a simple connector and contract health check.

```json
{
  "name": "LINEA API",
  "version": "1.0-r2.3.3",
  "schema": 3,
  "readOnly": true
}
```

`version` is the connector implementation version; `schema` is the public data-model version.

## `GET /api/v1/status`

Returns one consolidated LINEA operational snapshot. Schema 3 top-level sections are:

```text
api
system
conventions
energy
ess
spot
forecast
solar
weather
vrm
temperatures
shelly
ups
climate
```

Optional modules use `{"available": true, "data": {}}` or `{"available": false, "data": null}`. A Daikin, UPS, Shelly or other optional-source outage therefore does not mean `/status` has failed.

## What the API does not provide

The main API has no control endpoints, especially no universal `/set`, `/control` or `/write`. If limited control of a few approved devices is ever implemented, it must be separated from the main read-only API.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)
