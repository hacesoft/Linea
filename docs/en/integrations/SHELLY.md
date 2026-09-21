# Shelly — LINEA integration

Use [node-red-shelly](https://github.com/hacesoft/node-red-shelly) for the complete manual and flow. The Dashboard 2.0 module monitors and controls Shelly Gen2+ over MQTT, including switching channels, smoke sensors and ntfy notifications.

## Configure the MQTT broker in the flow editor

The broker address **cannot be changed from this module's LINEA dashboard**. Unlike Modbus settings, MQTT connectivity remains in a Node-RED configuration node. Attempts to change it dynamically from the UI caused instability in this project.

1. Find `SHELLY :: MQTT` or search for `MQTT Switch events` in Node-RED.
2. Open the MQTT input and click the pencil beside **Server**.
3. Edit the **Server/host** and **Port** in the broker configuration named `NAS_docker_mqtt` in the reference export. Port 1883 is commonly used without TLS; follow your broker configuration.
4. Set authentication under **Security** and TLS where required. Renaming the node does not change its host.
5. Ensure `MQTT Switch RPC` uses the same broker. Editing a shared configuration affects all nodes using it.
6. Confirm **Update/Done → Deploy** and check the connection and incoming messages.
7. Configure the same broker on the Shelly devices. A device IP in the Shelly list is not the broker IP.

The device list uses `global.shellyDevices`. Preserve configuration persistence and initialization links when importing. `global.lineaApiShellyState` and `global.lineaApiShellySmokeState` feed the API `shelly` section. Cached `available` is not a live connectivity check; use smoke `lastSeen` for age. See the module manual for topics, supported devices and standalone requirements.

[Česky](../../cz/integrace/SHELLY.md) · [← Integrations](../06_INTEGRATIONS_AND_TOOLS.md)
