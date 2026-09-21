[🇨🇿 Česky](../../cz/api/04_DATOVY_MODEL.md) | [🇬🇧 English](04_DATA_MODEL.md)

---

# Data model – schema 1

## `system`

`timestamp` is the API-response creation time, `sourceTimestamp` the source LINEA snapshot time, `ageMs` its age and `stale` the stale flag for the main snapshot.

## `energy`

### PV

- `energy.pv.powerW` – total instantaneous PV power [W].
- `energy.pv.strings[]` – MPPT strings with `name`, `powerW`, `pvVoltageV`, `pvCurrentA`, `yieldTodayKWh`.

Internal `instance` is not published. If the main PV snapshot temporarily reports 0 W while strings have non-zero power, 1.0.0 falls back to the sum of string power.

### House and grid

House: `energy.house.powerW` and per-phase `l1PowerW/l2PowerW/l3PowerW`. Grid uses the equivalent structure under `energy.grid`.

### Battery

Published values include `socPct`, `powerW`, `currentA`, `voltageV`, `batteryLifeSocLimitPct`. Power and current are signed.

## `ess`

The API exports the result of existing LINEA logic; it does not recalculate it. `decision` includes values such as `gridPointW`, `pvSurplusW`, `predictionActive`, `exportAllowed`, `reason`. `reason.code` may be `UNCLASSIFIED`; the API must not guess a reason.

Published `switches` include `controlModeEssAcGrid`, `spotGridCharging`, `gridCharging`, `gridConsumption`, `energyThresholdInjector`, `nonBatteryPriority`, `delayCharging`, `dynamicSocReserve`, `predictionThreshold`, `socDeltaBeforeExport`, `morningPeakBatterySales`, `eveningPeakBatterySales`.

Published `settings` include values such as `balancingReserveW`, `setGridValueW`, `maxGridPointW`, `spotThresholdPrice`, morning/evening SOC targets, grid-charging SOC, prediction threshold, SOC delta, charging duration and acceptable grid price. Times are exposed both as `HH:MM` and milliseconds since midnight.

## `spot`, `forecast`, `solar`, `weather`

`spot.available` and `spot.currentPrice` are taken from LINEA; the API does not download or recalculate the price. Forecast solar yield and consumption are converted from source Wh to kWh. Solar exposes values such as sunrise/sunset/day length. Weather only publishes the prepared LINEA weather state.

## `vrm`

VRM totals map `grid_history_from` to grid import, `grid_history_to` to export, `total_consumption` to consumption and `total_solar_yield` to PV yield. Public values include `pvYieldKWh`, `consumptionKWh`, `gridImportKWh`, `gridExportKWh`, `batteryChargeKWh`, `batteryDischargeKWh`.

### Important: battery counters are not daily values

`batteryChargeKWh` and `batteryDischargeKWh` are based on:

```text
nBatteryALL_input_Wh
nBatteryALL_output_Wh
```

They **do not reset at midnight**. They accumulate **since reset**. Default memory context is lost on restart; persistent context can change this behavior, and its configuration is not included in the flow export. They are also used for battery-efficiency calculation. Their current placement under `vrm.data.today` is therefore semantically imprecise and clients must not interpret them as today's energy.

## `temperatures`, `shelly`, `ups`, `climate`

Temperatures publish names and `temperatureC`, not Modbus Unit IDs. Shelly switching devices expose only minimal state (`name`, `kind`, `channel`, `state`, `available`), while Smoke adds alarm/battery/RSSI/wakeup/last-seen data; sleeping sensors may legitimately have high age. UPS exposes NUT state, battery/input/output/load values where available and omits serial/network data. Daikin climate exposes operational state, temperatures, setpoint, energy and error/firmware information, never tokens or credentials; `operationMode` does not necessarily mean the compressor is physically running.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)

## Interpretation limits

`ess.decision.gridPointW` is the AC LOAD request before output limiting, not an acknowledged write or measured grid flow. `predictionActive` is the value of `bPredikce`, which can be true when the filter is disabled or bypassed for invalid data. `ess.switches.predictionThreshold` is the actual enable switch.

The battery counter adds `P/3600` per call, assuming one second. Fractional Wh are possible; division by 1000 does not guarantee 0.001 kWh resolution. Some conversions turn null into zero. See [freshness and limitations](05_CONVENTIONS_AND_FRESHNESS.md).

## Numeric and decision field reference

Ranges below describe source expectations, not enforced API clamps. Resolution is not measurement accuracy. Missing values and coercion limits are described above.

| Path | Type | Unit | Resolution | Expected range | Meaning |
|---|---|---|---|---|---|
| `api.name` | string | — | — | — | Interface name. |
| `api.version` | string | — | — | — | API implementation version. |
| `api.schema` | integer | 1 | 1 | — | Public data schema version. |
| `api.readOnly` | boolean | — | false/true | — | Read-only flag. |
| `system.timestamp` | string(date-time) | — | — | — | Response creation time. |
| `system.sourceTimestamp` | string(date-time) | — | — | — | Main LINEA snapshot time. |
| `system.ageMs` | integer | ms | 1 ms | 0… | Main snapshot age. |
| `system.stale` | boolean | — | false/true | — | Main snapshot stale flag (>30000 ms or unknown age). |
| `energy.pv.powerW` | number | W | 1 W | according to source | Total instantaneous PV power. |
| `energy.pv.strings[].powerW` | number | W | 1 W | according to source | Instantaneous MPPT branch power. |
| `energy.pv.strings[].pvVoltageV` | number | V | 0.01 V | according to device | MPPT PV voltage. |
| `energy.pv.strings[].pvCurrentA` | number | A | 0.1 A | according to device | Calculated PV branch current. |
| `energy.pv.strings[].yieldTodayKWh` | number | kWh | 0.1 kWh | ≥ 0 | Today's MPPT yield. |
| `energy.house.powerW` | number | W | according to source | according to installation | Total house power. |
| `energy.house.phases.l1PowerW` | number | W | according to source | according to installation | House phase L1 power. |
| `energy.house.phases.l2PowerW` | number | W | according to source | according to installation | House phase L2 power. |
| `energy.house.phases.l3PowerW` | number | W | according to source | according to installation | House phase L3 power. |
| `energy.grid.powerW` | number | W | according to source | according to installation | Grid power: positive import, negative export. |
| `energy.grid.phases.l1PowerW` | number | W | according to source | according to installation | Grid phase L1 power: positive import, negative export. |
| `energy.grid.phases.l2PowerW` | number | W | according to source | according to installation | Grid phase L2 power: positive import, negative export. |
| `energy.grid.phases.l3PowerW` | number | W | according to source | according to installation | Grid phase L3 power: positive import, negative export. |
| `energy.battery.socPct` | number | % | according to source | expected 0–100 % | Battery state of charge. |
| `energy.battery.powerW` | number | W | 1 W | according to system | Battery power: positive charging, negative discharging. |
| `energy.battery.currentA` | number | A | 0.1 A | source signed INT16 / 10 | Battery current. |
| `energy.battery.voltageV` | number | V | 0.1 V | source raw / 10 | Battery voltage. |
| `energy.battery.batteryLifeSocLimitPct` | number | % | 0.1 % | expected 0–100 % | BatteryLife/ESS lower SOC limit. |
| `ess.decision.gridPointW` | number | W | 1 W | according to configuration | AC LOAD grid request before output limiting; not acknowledged or measured power. |
| `ess.decision.pvSurplusW` | number | W | 1 W | according to installation | Internal PV surplus/deficit used for decisions. |
| `ess.decision.predictionActive` | boolean | — | false/true | — | Value of bPredikce; can be true when prediction filter is disabled or bypassed. |
| `ess.decision.exportAllowed` | boolean | — | false/true | — | Export-permission flag used by the decision. |
| `ess.decision.reason.code` | string | — | — | enum may expand | Machine-readable reason code. |
| `ess.decision.reason.text` | string\|null | — | — | — | Optional reason text. |
| `ess.settings.balancingReserveW` | number | W | 1 W | according to configuration | Balancing power reserve. |
| `ess.settings.setGridValueW` | number | W | 1 W | according to configuration | Configured grid target. |
| `ess.settings.maxGridPointW` | number | W | 1 W | according to configuration/hardware | Strategy grid-power setting. |
| `ess.settings.spotThresholdPrice` | number | CZK/kWh | according to source/settings | no API clamp | SPOT strategy price threshold. |
| `ess.settings.morningSocSalesPct` | number | % | according to settings | 0–100 % expected | Morning strategy SOC threshold. |
| `ess.settings.eveningSocSalesPct` | number | % | according to settings | 0–100 % expected | Evening strategy SOC threshold. |
| `ess.settings.gridChargingSocPct` | number | % | according to settings | 0–100 % expected | Grid-charging target SOC. |
| `ess.settings.predictionThresholdKWh` | number | kWh | according to settings | ≥ 0 expected | Prediction energy threshold. |
| `ess.settings.socDeltaBeforeExportPct` | number | % | according to settings | 0–100 % expected | SOC threshold before export. |
| `ess.settings.chargingDurationGridH` | number | h | according to settings | ≥ 0 expected | Grid charging duration. |
| `ess.settings.acceptablePriceGrid` | number | CZK/kWh | according to source/settings | no API clamp | Maximum accepted grid purchase price. |
| `ess.time.delayCharging.start` | string | HH:MM | 1 min | 00:00–23:59 | Delay Charging window start. |
| `ess.time.delayCharging.stop` | string | HH:MM | 1 min | 00:00–23:59 | Delay Charging window end. |
| `ess.time.delayCharging.startMs` | number | ms | 1 ms | 0–<86400000 expected | Configured start in milliseconds; source value is converted, not range-validated. |
| `ess.time.delayCharging.stopMs` | number | ms | 1 ms | 0–<86400000 expected | Configured end in milliseconds; source value is converted, not range-validated. |
| `ess.time.morningPeakHours[]` | integer | h | 1 h | 0–23 | Morning peak hours. |
| `ess.time.eveningPeakHours[]` | integer | h | 1 h | 0–23 | Evening peak hours. |
| `spot.currentPrice` | number | CZK/kWh | according to source | may be negative | Current SPOT price used by LINEA. |
| `forecast.solarYieldForecastKWh` | number | kWh | according to source | ≥ 0 expected | Forecast PV energy. |
| `forecast.consumptionForecastKWh` | number | kWh | according to source | ≥ 0 expected | Forecast consumption energy. |
| `weather.rainProbabilityPct` | number | % | according to source | 0–100 % expected | Rain probability. |
| `vrm.data.today.pvYieldKWh` | number | kWh | according to VRM | ≥ 0 | Today's PV yield. |
| `vrm.data.today.consumptionKWh` | number | kWh | according to VRM | ≥ 0 | Today's consumption. |
| `vrm.data.today.gridImportKWh` | number | kWh | according to VRM | ≥ 0 | Today's grid import. |
| `vrm.data.today.gridExportKWh` | number | kWh | according to VRM | ≥ 0 | Today's grid export. |
| `vrm.data.today.batteryChargeKWh` | number | kWh | Depends on integration and call timing | ≥ 0 | Cumulative battery charge energy since reset, not daily energy. |
| `vrm.data.today.batteryDischargeKWh` | number | kWh | Depends on integration and call timing | ≥ 0 | Cumulative battery discharge energy since reset, not daily energy. |
| `temperatures.data.racks[].temperatureC` | number | degC | 0.01 °C | according to sensor | Rack/battery temperature. |
| `temperatures.data.inverters[].temperatureC` | number | degC | 0.01 °C | according to sensor | Inverter temperature. |
| `temperatures.data.other[].temperatureC` | number | degC | 0.01 °C | according to sensor | Other temperature. |
| `shelly.data.devices[].channel` | integer | 1 | 1 | ≥ 0 | Shelly channel number. |
| `shelly.data.smokeDetectors[].batteryPct` | number | % | according to Shelly | 0–100 % expected | Smoke-sensor battery state. |
| `shelly.data.smokeDetectors[].batteryVoltageV` | number | V | according to Shelly | according to device | Smoke-sensor battery voltage. |
| `shelly.data.smokeDetectors[].rssiDbm` | number | dBm | 1 dBm typically | according to Wi-Fi | Wi-Fi signal strength. |
| `shelly.data.smokeDetectors[].ageSec` | integer | s | 1 s | ≥ 0 | Stored last-report age; may not advance on every HTTP request. |
| `ups.data.battery.chargePct` | number | % | according to NUT/UPS | 0–100 % expected | UPS battery charge. |
| `ups.data.battery.voltageV` | number\|null | V | according to NUT/UPS | according to UPS | UPS battery voltage. |
| `ups.data.battery.runtimeSec` | number\|null | s | according to NUT/UPS | ≥ 0 | Estimated UPS runtime. |
| `ups.data.input.voltageV` | number\|null | V | according to NUT/UPS | according to UPS | UPS input voltage. |
| `ups.data.output.voltageV` | number\|null | V | according to NUT/UPS | according to UPS | UPS output voltage. |
| `ups.data.output.frequencyHz` | number\|null | Hz | according to NUT/UPS | according to UPS | UPS output frequency. |
| `ups.data.load.percent` | number\|null | % | according to NUT/UPS | 0–100 % typically | UPS load percentage. |
| `ups.data.load.realPowerW` | number\|null | W | according to NUT/UPS | ≥ 0 typically | UPS real load power. |
| `climate.data.devices[].roomTemperatureC` | number\|null | degC | according to Onecta | according to device | Room temperature. |
| `climate.data.devices[].outdoorTemperatureC` | number\|null | degC | according to Onecta | according to device | Outdoor temperature. |
| `climate.data.devices[].setpointC` | number\|null | degC | according to Onecta | according to mode/device | Target temperature. |
| `climate.data.devices[].energy.todayKWh` | number\|null | kWh | according to Onecta | ≥ 0 | Today's air-conditioner energy. |
| `climate.data.devices[].energy.weekKWh` | number\|null | kWh | according to Onecta | ≥ 0 | Energy for the weekly period. |
| `climate.data.devices[].energy.monthKWh` | number\|null | kWh | according to Onecta | ≥ 0 | Energy for the monthly period. |
| `climate.data.devices[].energy.coolingMonthKWh` | number\|null | kWh | according to Onecta | ≥ 0 | Monthly cooling energy. |
| `climate.data.devices[].energy.heatingMonthKWh` | number\|null | kWh | according to Onecta | ≥ 0 | Monthly heating energy. |
| `spot.intervalMinutes` | integer | min | — | — | SPOT price interval. |
| `spot.today.date` | string | — | — | — | Current local date YYYY-MM-DD. |
| `spot.today.prices[].hour` | integer | h | — | Array index; not validated | Array index assigned as hour; length and DST mapping are not validated by the API. |
| `spot.today.prices[].price` | number\|null | CZK/kWh | — | — | Hourly SPOT price. |
| `spot.tomorrow.date` | string | — | — | — | Next local date YYYY-MM-DD. |
| `spot.tomorrow.prices[].hour` | integer | h | — | Array index; not validated | Array index assigned as hour; length and DST mapping are not validated by the API. |
| `spot.tomorrow.prices[].price` | number\|null | CZK/kWh | — | — | Next-day hourly SPOT price. |
| `forecast.intervalMinutes` | integer | min | — | — | VRM forecast interval; 15 minutes. |
| `forecast.series.solarYield[].timestampMs` | integer\|null | ms | — | — | Forecast point Unix timestamp. |
| `forecast.series.solarYield[].energyKWh` | number\|null | kWh | — | — | Forecast PV energy for interval. |
| `forecast.series.consumption[].timestampMs` | integer\|null | ms | — | — | Forecast point Unix timestamp. |
| `forecast.series.consumption[].energyKWh` | number\|null | kWh | — | — | Forecast consumption energy for interval. |

## Module state fields

`ups.data.online` corresponds to NUT `OL` (mains power), not general communication availability; `onBattery` corresponds to `OB`. `status.raw` retains the NUT flags. Shelly `state` is cached channel state and its `available` is not a heartbeat. Daikin `cloudUp`, `on`, `operationMode` and `error` describe the last processed cloud state. `firmwareChanged` is a comparison during processing, not a persistent change history.
