[Česky](../cz/05_ZAVISLOSTI.md) | [English](05_DEPENDENCIES.md)

# Dependencies

The reference export lists these packages:

| Package | Version recorded in export |
|---|---|
| `@flowfuse/node-red-dashboard` | 1.30.2 |
| `node-red-contrib-modbus` | 5.45.2 |

These are reference versions, not claims about the latest or minimum supported version. Install through **Menu → Manage palette → Install**. Dashboard 2.0 is the intended UI; legacy `node-red-dashboard` is a different package.

MQTT, HTTP, TCP, Function and File nodes are built into Node-RED. Modbus and Dashboard nodes require their packages. If import reports unknown node types, install the corresponding package and restart if required. Check each [module repository](06_INTEGRATIONS_AND_TOOLS.md) for its dependencies and standalone prerequisites.

[← Documentation](README.md)
