[🇨🇿 Česky](../../cz/api/04_DATOVY_MODEL.md) | [🇬🇧 English](04_DATA_MODEL.md)

---

# Data model – schema 3

## `system`

`timestamp` is the API-response creation time, `sourceTimestamp` the source LINEA snapshot time, `ageMs` its age and `stale` the stale flag for the main snapshot.

## `energy`

### PV

- `energy.pv.powerW` – total instantaneous PV power [W].
- `energy.pv.strings[]` – MPPT strings with `name`, `powerW`, `pvVoltageV`, `pvCurrentA`, `yieldTodayKWh`.

Internal `instance` is not published. Verified names include `MPPT WEST` and `MPPT VJ`. If the main PV snapshot temporarily reports 0 W while strings have non-zero power, R2.3.3 falls back to the sum of string power.

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

They **do not reset at midnight**. They are cumulative **since the last Node-RED restart**, and a restart resets them to zero. They are also used for battery-efficiency calculation. Their current placement under `vrm.data.today` is therefore semantically imprecise and clients must not interpret them as today's energy.

## `temperatures`, `shelly`, `ups`, `climate`

Temperatures publish names and `temperatureC`, not Modbus Unit IDs. Shelly switching devices expose only minimal state (`name`, `kind`, `channel`, `state`, `available`), while Smoke adds alarm/battery/RSSI/wakeup/last-seen data; sleeping sensors may legitimately have high age. UPS exposes NUT state, battery/input/output/load values where available and omits serial/network data. Daikin climate exposes operational state, temperatures, setpoint, energy and error/firmware information, never tokens or credentials; `operationMode` does not necessarily mean the compressor is physically running.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)
