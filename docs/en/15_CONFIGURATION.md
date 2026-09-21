[Česky](../cz/15_KONFIGURACE.md) | [English](15_CONFIGURATION.md)

# Configuration

Central settings are held in `global.config`. Dashboard handlers change this object; control logic reads it. Use [settings reference](11_SETTINGS_REFERENCE.md) for the strategy keys.

| Area | Configuration |
|---|---|
| Flow version | `flow_version`, format `DDMMYYYY_HHMM`; initialized into `global.linea_version_local`. |
| Modbus | `connectorConfig.tcpHost`, `tcpPort`, `unitId`; editable through the LINEA configuration path. |
| VRM | API token and site ID; configure only if the related functions are used. |
| Location | Latitude/longitude and correct Node-RED timezone for solar/time calculations. |
| UPS | `global.config.upsConfig`; see [UPS](integrations/UPS.md). |
| Daikin | `global.config.daikinConfig`; tokens and module files have their own persistence. Default/configurable minimum polling: 8 minutes. |
| Shelly list | `global.shellyDevices` and its persistence links. |
| MQTT connection | Edit the Node-RED MQTT broker config node manually; `config.mqtt` is not a UI control of the active connection. See [Shelly instructions](integrations/SHELLY.md). |

Save settings to writable persistent storage and verify reload behavior. `Parse Config` in the reference export assigns a loaded object directly; do not assume new defaults are automatically merged on update. `flow_version`, configuration schema and API schema are distinct version identifiers.

Do not publish API tokens, OAuth secrets, refresh tokens or passwords. Back up module settings separately where their manual requires it. A generic LINEA save action must not be assumed to write every standalone module file.

[← Documentation](README.md)
