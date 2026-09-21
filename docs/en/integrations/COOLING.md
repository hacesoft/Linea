# Rack and MPPT cooling — LINEA integration

Use [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack) for the flow and complete configuration manual.

The reference flow uses `global.pvPower`, `global.nBattery_Power`, battery-register sign conversion and HTTP relay commands. Standalone use requires compatible inputs and helper functions with the same units and meaning.

Configure relay addresses, channels, power thresholds, schedule and hysteresis for your installation. This export is not a ready-made adapter for arbitrary temperature sensors. An OFF request or displayed requested state does not confirm that a fan stopped. In the reference flow, `FAN ALL STOP` passes through ordinary hysteresis and must not be treated as an immediate safety stop.

LINEA API has no dedicated `cooling` block or fan-control endpoint. Values under `temperatures` do not confirm fan state.

[Česky](../../cz/integrace/COOLING.md) · [← Integrations](../06_INTEGRATIONS_AND_TOOLS.md)
