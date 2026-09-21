[🇨🇿 Česky](../../cz/integrace/UPS.md) | [🇬🇧 **English**](UPS.md)

---
# UPS / NUT module

The active optional module provides read-only UPS monitoring through Network UPS Tools (NUT) and is not required by LINEA CORE. It connects to `upsd` over TCP and requests `LIST VAR <UPS_NAME>`. Legacy disabled UPS branches are not treated as supported implementations.

Configuration lives in `global.config.upsConfig`: host, port (normally 3493), case-sensitive `upsName`, TCP timeout and optional ntfy URL. The active module has no user enable switch and no configurable polling interval. The Dashboard can test current form values before saving.

LINEA queries NUT about every **5 seconds**. This is not the physical UPS refresh rate because the driver has separate `pollinterval` and `pollfreq`. The watchdog reports `NUT OFFLINE` after more than **15 seconds** without a successful response; unchanged values are not a communication fault.

The parser builds `global.ups`; a reduced `global.lineaApiUpsState` is exposed through the public read-only API. Events include at least `POWER_LOST`, `POWER_RESTORED` and `BATTERY_LOW`. Future Nextcloud history should use change-based validity intervals for slowly changing UPS state.

---
[← Integrations](../06_INTEGRATIONS_AND_TOOLS.md)


Complete installation, NUT setup and current flow: [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups).
