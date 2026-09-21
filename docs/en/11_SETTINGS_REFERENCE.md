# Settings reference

Use the reference keys below with `global.config`. Units describe intended interpretation; they are not guaranteed runtime validation. Grid request is positive for import and negative for export. A request can be changed by other active strategy branches and by output limiting.

| Control | Configuration key | Unit/type | Meaning |
|---|---|---|---|
| Set Point | `nGridConsumptionEnable` | boolean | Enable fixed grid request |
| Setpoint | `nSetGridValue` | W | Signed request; + import, − export |
| Control Mode | `nControl_Mode_ESS_AC_Grid` | boolean | false: 2700; true: 2716/2717 |
| Injector | `nEnergyThresholdInjector` | boolean | Installation-specific surplus injection |
| Non Battery Priority | `nNonBatteryPriorityMode` | boolean | Prefer grid over battery under high load |
| Delay Charging | `switchDelayCharging` | boolean | Enable deferred charging |
| Start / stop | `tStartBattery / tStopBattery` | ms / time | Configured delay window |
| SOC condition | `nSOCDeltaBeforeExport_Switch` | boolean | Require SOC before delay export |
| SOC threshold | `nSOCDeltaBeforeExport` | % | Threshold for delay export |
| Dynamic reserve | `nDynamicSOC_Reserve` | boolean | Estimate morning SOC reserve |
| Morning sales | `nMorningPeakBatterySales` | boolean | Enable morning price-window sales |
| Morning SOC | `nMorningSOC_sales` | % | Morning reserve threshold |
| Evening sales | `nEveningPeakBatterySales` | boolean | Enable evening sales |
| Evening SOC | `nEveningSOC_sales` | % | Evening reserve threshold |
| Prediction filter | `nPredictionThreshold` | boolean | Filter dependent operations when forecast is valid |
| Prediction threshold | `sPredictionThresholdKW` | kWh | Energy threshold despite key suffix |
| GRID Charging | `sGridChargingSwitch` | boolean | One-shot charge to target SOC |
| Charge target | `nGridChargingSOC` | % | Target SOC; function stops when reached |
| SPOT charging | `switchSpotGridCharging` | boolean | Enable continuous low-cost block |
| Charging duration | `nCharging_DurationGRID` | h | Length of low-cost block |
| Buy price | `nAcceptable_Price_GRID` | CZK/kWh | Accepted purchase threshold |
| Strategy power | `nMAX_Grid_Point` | W | Demand for selected battery strategies |
| Reserve | `nBalancingReserve` | W | Margin used in surplus calculations |
| Peak duration | `nSetPeak` | h | Peak-window length; reference widget maximum 4 h |
| Sunrise offset | `nSunriseProductionOffset` | h | Delay until useful PV generation |
| House consumption | `nHouseHourlyConsumption` | SOC %/h | Estimated hourly use for reserve calculation |
| SPOT auto | `isSpotAutoCtrlEnabled` | boolean | Price-based export control |
| SPOT threshold | `spotTresholdPrice` | CZK/kWh | Price threshold |

## Operating relationships

Set Point uses the same selected target register as other strategies. The reference UI allows −1000…+1000 W in 10 W steps. Control Mode OFF selects 2700 and suppresses unchanged requests; ON selects 2716/2717 and forwards repeated requests. A mode change forces the current request onto the new target.

Usable PV surplus is calculated from PV minus house consumption and balancing reserve. Energy Threshold Injector is marked as installation-specific. Delay Charging uses its time window, feed-in permission, optional SOC threshold and prediction filter. Morning sales also depend on SOC, selected hours, export permission and price; Dynamic SOC Reserve estimates the SOC needed until useful solar generation. Evening sales use their own SOC/price window and do not apply the morning prediction filter.

GRID Charging switches off after reaching target SOC; it is not permanent SOC maintenance. Spot-Grid Charging seeks a continuous low-cost block, but the reference helper can return first-hour fallback. Do not assume unavailable prices prevent charging. Invalid forecast data similarly bypass Prediction Threshold rather than blocking dependent operations.

`nMAX_Grid_Point` is strategy demand; 2706 is a separate feed-in limit. Actual power is also constrained by inverter, battery/BMS and site conditions. The output writer does not fully validate numeric ranges or the unlimited-feed-in sentinel. See [Modbus](12_MODBUS.md).

## Diagnosing a setting

Check the config value, AC LOAD result, requested `nSet_Grid_Point`, output limiting, selected target, actual Modbus response and measured power. A changed dashboard switch alone does not prove control succeeded.

[Česky](../cz/11_REFERENCE_NASTAVENI.md) · [← Documentation](README.md)
