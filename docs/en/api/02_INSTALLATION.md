[🇨🇿 Česky](../../cz/api/02_INSTALACE.md) | [🇬🇧 English](02_INSTALLATION.md)

---

# Installation and connection

LINEA API runs inside Node-RED alongside the main LINEA flow and reads its existing globals/snapshots. It is not a separate control system.

## Requirements

- working LINEA in Node-RED;
- imported LINEA API flow/module matching `1.0-r2.3.3`;
- source globals/snapshots available for the sections used by the installation;
- client network access to the Node-RED HTTP interface according to the local deployment.

## Verification after deployment

Call `GET /api/v1/health` first. Expected contract:

```json
{"name":"LINEA API","version":"1.0-r2.3.3","schema":3,"readOnly":true}
```

Then verify `GET /api/v1/status`, especially `system.ageMs`, `system.stale`, energy values and `available` on optional modules.

An unavailable optional module must not fail the whole API. Clients must accept `available: false, data: null`.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)
