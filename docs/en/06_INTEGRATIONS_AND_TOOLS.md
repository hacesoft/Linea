# Optional modules and integrations

LINEA CORE handles PV/ESS control. Shelly, Daikin, UPS and cooling are optional modules maintained in separate repositories. Use each repository for its current flow, dependencies, installation instructions and full manual; the pages here explain the connection to LINEA.

| Module | Repository and full manual | LINEA integration |
|---|---|---|
| Shelly | [node-red-shelly](https://github.com/hacesoft/node-red-shelly) | [Integration](integrations/SHELLY.md) |
| Daikin ONECTA | [node-red-daikin](https://github.com/hacesoft/node-red-daikin) | [Integration](integrations/DAIKIN.md) |
| Eaton UPS / NUT | [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups) | [Integration](integrations/UPS.md) |
| Chlazení / Cooling | [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack) | [Integration](integrations/COOLING.md) |

## Import and updates

Back up the flow and configuration. Check whether the module is already included in your LINEA export. Replace the existing module instead of running a second copy: duplicate MQTT subscriptions, NUT polling, cloud calls or device commands can interfere. Check shared Dashboard pages/groups, configuration nodes, link nodes, globals and file paths before Deploy. Standalone operation requires the dependencies documented by that module; importing JSON alone does not provide missing LINEA helpers.

## Nextcloud monitoring — in development

The planned LINEA Monitor & Analytics application will read the [LINEA API](api/README.md). Its repository URL and preview image are not supplied in this documentation package. Installation and history collection are not available here. See [planned scope](api/07_NEXTCLOUD_AND_FUTURE.md).

## Related projects
- [LM335](https://github.com/hacesoft/LM335)
- [Clever Boiler](https://github.com/hacesoft/Clever_boiler)
- [Modbus Scanner](https://github.com/hacesoft/Scanner_ModBus)
- [7-inch display for Victron](https://github.com/hacesoft/7_inch_display_for_victron)

[← LINEA](README.md)
