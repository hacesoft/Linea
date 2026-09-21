[Česky](../cz/10_OVLADANI_ESS.md) | [English](10_ESS_CONTROLS.md)

# ESS controls

Start with one strategy at a time and compare requested grid point with measured grid flow.

| Dashboard control | Use |
|---|---|
| Control Mode: ESS / AC Grid | OFF selects INT16 register 2700; ON selects INT32 registers 2716/2717. OFF is not a master stop. |
| Set Point | Enable the fixed grid request and enter its signed value. The reference widget range is −1000…+1000 W in 10 W steps. |
| Energy Threshold Injector | Installation-specific PV-surplus setpoint logic; understand its conditions before enabling. |
| Non Battery Priority | Prefer grid supply over battery discharge during high demand. |
| Delay Charging | Set start/end time and enable deferred charging; review SOC and feed-in conditions. |
| SOC Delta Before Export | Apply the configured SOC threshold before Delay Charging export. |
| Morning / Evening Sales | Enable the corresponding price window and set the minimum SOC. |
| Dynamic SOC Reserve | Estimate the reserve needed until useful solar generation. |
| Prediction Threshold | Set an energy threshold in kWh. Invalid forecast data bypass the filter in the reference flow. |
| GRID Charging | Charge to a target SOC using the strategy power setting. |
| Spot-Grid Charging | Select block duration and maximum accepted purchase price; verify complete price data. |
| MAX Grid Point | Power request used by selected battery strategies; distinct from feed-in register 2706. |
| Balancing Reserve | Power margin subtracted when computing usable PV surplus. |

Dashboard handlers update central configuration. AC LOAD reads it and calculates the request; the writer selects and writes the register. Changing a UI control does not prove a successful write. Use the [configuration-key reference](11_SETTINGS_REFERENCE.md) and [troubleshooting](17_TROUBLESHOOTING.md).

[← Documentation](README.md)
