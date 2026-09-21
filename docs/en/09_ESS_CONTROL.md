[Česky](../cz/09_ESS_RIZENI.md) | [English](09_ESS_CONTROL.md)

# ESS control logic

LINEA evaluates active strategies to produce one requested grid setpoint. Positive means import and negative means export. Simultaneous strategies can affect the same result; check the final request and actual system response when combining them.

| Strategy | Effect and conditions |
|---|---|
| Set Point | Uses `nSetGridValue` when `nGridConsumptionEnable` is active. |
| Non Battery Priority | Aims to avoid battery discharge under high load by requesting grid import. |
| Delay Charging | Defers normal charging within the configured window, subject to feed-in and SOC conditions. |
| Morning Peak | Sells battery energy in a selected morning price window, subject to SOC, export and price conditions. |
| Dynamic SOC Reserve | Adjusts the morning reserve using sunrise, production offset and estimated hourly SOC use. |
| Evening Peak | Sells in an evening price window down to its SOC threshold; the morning prediction filter is not applied. |
| GRID Charging | Requests charging to target SOC and switches off when the target is reached; it is not continuous SOC maintenance. |
| Spot-Grid Charging | Selects a continuous low-cost block and applies configured price and operating conditions. |

With valid prediction data, Prediction Threshold compares solar forecast kWh against the larger of consumption forecast and the configured threshold. With invalid data, the reference flow sets `bPredikce = true`: the filter is bypassed rather than blocking dependent strategies.

`findCheapestContinuousBlock` can return the first hours as fallback when input is invalid or no block is found. Missing prices are not a guaranteed automatic stop; disable this strategy when price data are unreliable. The writer is not a substitute for hardware/BMS limits. See [settings](11_SETTINGS_REFERENCE.md).

[← Documentation](README.md)
