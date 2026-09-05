[🇨🇿 Česky](../../cz/api/PREHLED.md) | [🇬🇧 English](README.md)

---

# LINEA API

LINEA API is an integration layer inside Node-RED that exposes the already existing LINEA operational state to other applications. The current production-verified contract is **LINEA API `1.0.0`, schema `1`**.

> **Core rule:** the main LINEA API is strictly **READ-ONLY**. It publishes the completed LINEA state; it does not make ESS decisions, recalculate control logic, or write Modbus registers.

## Contents

- [Overview and architecture](01_OVERVIEW_AND_ARCHITECTURE.md)
- [Installation and connection](02_INSTALLATION.md)
- [Endpoints](03_ENDPOINTS.md)
- [schema 1 data model](04_DATA_MODEL.md)
- [Conventions, freshness and availability](05_CONVENTIONS_AND_FRESHNESS.md)
- [Response examples](06_EXAMPLES.md)
- [Nextcloud, history and future work](07_NEXTCLOUD_AND_FUTURE.md)

## Public endpoints

```text
GET /api/v1/health
GET /api/v1/status
```

Control endpoints such as `/set`, `/control`, or `/write` are not part of the API and should not be added without a concrete requirement.

## Responsibility split

**LINEA / Node-RED** is authoritative for current state, Modbus, ESS control, safety and operational decisions. **LINEA API** only publishes the result. The future **Nextcloud LINEA Monitor & Analytics** is intended for history, database storage, aggregation, statistics, analytics and visualization.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)


## OpenAPI

[OpenAPI 3.1 specification](openapi.yaml)

## Availability

LINEA API is integrated directly into LINEA FLOW starting with LINEA FLOW version `01092026`. It is not a separately installed module or application. Older LINEA FLOW versions do not provide the API and it will not be backported to them for technical reasons.

