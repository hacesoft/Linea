[Česky](../cz/04_FAQ.md) | [English](04_FAQ.md)

# FAQ

| Question | What to check |
|---|---|
| Wrong time or peak window? | Server timezone, `TZ`, source date and SPOT hours. |
| Configuration disappears? | Persistent writable storage and actual file paths; verify after restart. |
| Must I install Shelly, Daikin, UPS or cooling? | No; they are optional. See [modules](06_INTEGRATIONS_AND_TOOLS.md). |
| Can I change the MQTT host in LINEA UI? | No. Edit the Node-RED MQTT broker config node; see [Shelly](integrations/SHELLY.md). |
| Does Control Mode OFF disable control? | No, it selects register 2700. |
| Why repeat 2716/2717 writes? | The external setpoint requires refresh; verify device timeout behavior. |
| Why suppress unchanged 2700 writes? | This branch deliberately uses send-on-change. |
| Why is charging lower than MAX Grid Point? | The strategy request is subject to inverter, battery/BMS and system limits. |
| Is MAX Grid Point the same as register 2706? | No; strategy demand and maximum feed-in are separate settings. |
| Is VRM mandatory? | Basic local Modbus functions do not all need it; cloud displays and prediction functions do. |
| Does HTTP 200 mean all data are current? | No. Check [API freshness](api/05_CONVENTIONS_AND_FRESHNESS.md). |

Use one authoritative setpoint controller. Export strategies require a suitable installation and permitted feed-in.

[← Documentation](README.md)
