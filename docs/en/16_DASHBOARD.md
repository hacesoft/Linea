[Česky](../cz/16_DASHBOARD.md) | [English](16_DASHBOARD.md)

# Dashboard 2.0

LINEA uses FlowFuse Dashboard 2.0. The PV view displays total and per-phase house/grid power, PV generation, battery SOC, current/voltage, counters, forecasts, SPOT prices and communication information.

Positive grid power means import and negative means export. Distinguish measured grid power from the requested grid setpoint. Counters and cached cloud data have separate refresh/reset behavior.

The ESS view configures strategies described in [ESS controls](10_ESS_CONTROLS.md). Control Mode selects 2700 or 2716/2717; OFF does not disable all writes. The toolbar shows local flow version and update availability. Group/page layout can be adapted to the installation.

Optional module widgets require their flow, shared Dashboard references and dependencies. Broker settings for Shelly remain in the Node-RED editor, even though device settings appear in Dashboard.

[← Documentation](README.md)
