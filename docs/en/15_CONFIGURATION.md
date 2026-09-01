[🇨🇿 Česky](../cz/15_KONFIGURACE.md) | [🇬🇧 **English**](15_CONFIGURATION.md)

---

# English version

The English translation will be generated after the Czech documentation is finalized. This placeholder keeps the CZ ↔ EN link structure stable.

## Modules and configuration boundary
Optional modules use central `global.config` for normal settings (`daikinConfig`, `upsConfig`); Daikin OAuth tokens are operational secrets with a separate lifecycle. Modbus TCP parameters can be managed by LINEA, while MQTT connectivity is a Node-RED **MQTT config node** and should be configured directly in Node-RED rather than dynamically rewritten by LINEA.
