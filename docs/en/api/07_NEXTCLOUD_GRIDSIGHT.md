[🇨🇿 Česky](../../cz/api/07_NEXTCLOUD_GRIDSIGHT.md) | [🇬🇧 English](07_NEXTCLOUD_GRIDSIGHT.md)

# GridSight in Nextcloud

[GridSight](https://github.com/hacesoft/GridSight) is a working application for monitoring LINEA in Nextcloud. It obtains operational data through the read-only LINEA API.

## Installation, configuration and features

The application repository maintains the current instructions:

- [GridSight English manual](https://github.com/hacesoft/GridSight/blob/main/README.md) — requirements, installation, configuration and features.
- [GridSight Czech manual](https://github.com/hacesoft/GridSight/blob/main/README_CZ.md).
- [GridSight repository](https://github.com/hacesoft/GridSight) — application source files and documentation.

Follow that manual when installing or updating GridSight. Supported Nextcloud versions, dependencies and installation procedures belong to GridSight documentation; LINEA has its own flow and API.

## Responsibilities

| Component | Role |
| --- | --- |
| LINEA / Node-RED | Device communication, calculations, ESS control, safety rules and current operational state. |
| LINEA API | Publishing current snapshots over read-only HTTP endpoints. |
| GridSight / Nextcloud | Monitoring and displaying data in Nextcloud; application features and settings are documented in the GridSight manual. |

Client-side history, storage and aggregation are not LINEA API endpoints. Follow the application documentation for their configuration. LINEA API does not contain a history database.

## Connecting to LINEA

1. Verify `GET /api/v1/health` and `GET /api/v1/status` in Node-RED; the API is part of the LINEA flow.
2. Check connectivity from the Nextcloud server or container. Access from a desktop browser alone is insufficient.
3. Configure the GridSight connection according to its manual. Use a Node-RED address reachable from Nextcloud and account for any reverse proxy or `httpNodeRoot` prefix.
4. Check the API response, data freshness and availability of optional modules.

For diagnostics, replace `LINEA_HOST:1880` with the actual address:

```bash
curl -i 'http://LINEA_HOST:1880/api/v1/health'
curl -i 'http://LINEA_HOST:1880/api/v1/status'
```

See [API setup and access protection](02_INSTALLATION.md), [endpoints](03_ENDPOINTS.md) and the [data model](04_DATA_MODEL.md).

## Availability and freshness

- HTTP `503` during initialization means LINEA has not prepared its main state yet. Missing measurements do not mean zero consumption or zero power.
- HTTP `200` alone does not guarantee fresh data. Check `system.stale`, timestamps and the [freshness rules](05_CONVENTIONS_AND_FRESHNESS.md).
- An absent optional module may report `available: false`; this does not necessarily mean the application's connection has failed.
- More frequent API requests do not increase the refresh rate of cloud services such as Daikin air conditioning.

## Interface boundaries

LINEA API is read-only. Connecting GridSight through this API does not change ESS settings, write Modbus registers or restore battery counters. Control and safety logic remain in Node-RED. The GridSight description does not introduce new commands or endpoints into LINEA API.

[← LINEA API](README.md) · [← Main documentation](../README.md)
