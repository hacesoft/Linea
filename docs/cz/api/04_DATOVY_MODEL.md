[🇨🇿 Česky](04_DATOVY_MODEL.md) | [🇬🇧 English](../../en/api/04_DATA_MODEL.md)

---

# Datový model – schema 3

## `system`

`timestamp` je čas vytvoření API odpovědi, `sourceTimestamp` čas zdrojového LINEA snapshotu, `ageMs` jeho stáří a `stale` příznak zastaralého hlavního snapshotu.

## `energy`

### FVE

- `energy.pv.powerW` – celkový okamžitý FV výkon [W].
- `energy.pv.strings[]` – jednotlivé MPPT stringy: `name`, `powerW`, `pvVoltageV`, `pvCurrentA`, `yieldTodayKWh`.

Interní `instance` se veřejně nepublikuje. Ověřené stringy jsou `MPPT WEST` a `MPPT VJ`. Pokud hlavní PV snapshot přechodně vrátí 0 W a stringy mají výkon, R2.3.3 použije fallback součtu stringů.

### Dům

`energy.house.powerW` a `energy.house.phases.l1PowerW/l2PowerW/l3PowerW`.

### Síť

`energy.grid.powerW` a `energy.grid.phases.l1PowerW/l2PowerW/l3PowerW`.

### Baterie

`socPct`, `powerW`, `currentA`, `voltageV`, `batteryLifeSocLimitPct`. Výkon i proud jsou podepsané hodnoty.

## `ess`

API exportuje výsledek existující LINEA logiky, nikoli nový výpočet.

`decision` zahrnuje např. `gridPointW`, `pvSurplusW`, `predictionActive`, `exportAllowed`, `reason`. `reason.code` může být `UNCLASSIFIED`; API nemá důvod rozhodnutí odhadovat.

`switches` zahrnují `controlModeEssAcGrid`, `spotGridCharging`, `gridCharging`, `gridConsumption`, `energyThresholdInjector`, `nonBatteryPriority`, `delayCharging`, `dynamicSocReserve`, `predictionThreshold`, `socDeltaBeforeExport`, `morningPeakBatterySales`, `eveningPeakBatterySales`.

`settings` zahrnují např. `balancingReserveW`, `setGridValueW`, `maxGridPointW`, `spotThresholdPrice`, `morningSocSalesPct`, `eveningSocSalesPct`, `gridChargingSocPct`, `predictionThresholdKWh`, `socDeltaBeforeExportPct`, `chargingDurationGridH`, `acceptablePriceGrid`.

Časy jsou publikovány čitelně (`HH:MM`) i jako ms od půlnoci.

## `spot`, `forecast`, `solar`, `weather`

- `spot.available`, `spot.currentPrice` – převzaté z LINEA, API cenu samo nestahuje ani nepřepočítává.
- `forecast.solarYieldForecastKWh` a `forecast.consumptionForecastKWh` – zdrojové VRM forecast hodnoty jsou převedeny z Wh na kWh.
- `solar` publikuje např. `sunrise`, `sunset`, `dayLength`.
- `weather` publikuje připravený stav, např. `today`, `rainProbabilityPct`, `trend`.

## `vrm`

VRM totals mapují `grid_history_from` na import ze sítě, `grid_history_to` na export, `total_consumption` na spotřebu a `total_solar_yield` na FV výrobu.

Veřejná struktura zahrnuje `pvYieldKWh`, `consumptionKWh`, `gridImportKWh`, `gridExportKWh`, `batteryChargeKWh`, `batteryDischargeKWh`.

### Důležité: bateriové čítače nejsou denní

`batteryChargeKWh` a `batteryDischargeKWh` vycházejí z LINEA globals:

```text
nBatteryALL_input_Wh
nBatteryALL_output_Wh
```

Tyto hodnoty **se nenulují o půlnoci**. Jsou kumulativní **od posledního restartu Node-RED** a restart je vynuluje. Používají se mimo jiné pro výpočet účinnosti baterie. Jejich současné umístění pod `vrm.data.today` je proto významově nepřesné a klient je nesmí interpretovat jako energii za dnešní den.

## `temperatures`

Publikuje pouze jméno a `temperatureC`, bez Modbus Unit ID. Kategorie: `racks`, `inverters`, `other`. Zdrojová logika mapuje VRM Temperature sensors a Modbus registr 3304, raw hodnotu dělí 100.

## `shelly`

Spínané prvky publikují minimum: `name`, `kind`, `channel`, `state`, `available`. Neexportují IP, MQTT ID, power/current/voltage/energy.

Shelly Smoke publikuje např. `name`, `alarm`, `ok`, `batteryPct`, `batteryVoltageV`, `rssiDbm`, `wakeupReason`, `lastSeen`, `ageSec`. Sleeping senzor může mít vysoké `ageSec` bez závady.

## `ups`

Publikuje stav NUT UPS: `name`, `online`, `onBattery`, stavové příznaky, battery charge/voltage/runtime, input/output voltage, output frequency a load percent/realPowerW. Některé hodnoty mohou být `null`, pokud je UPS neposkytuje. Sériová a síťová data se neexportují.

## `climate`

Daikin jednotka publikuje `name`, `cloudUp`, `on`, `operationMode`, pokojovou/venkovní teplotu, setpoint, energetické statistiky, error/errorCode a firmware informace. Tokeny a přihlašovací údaje se neexportují. `operationMode` neznamená nutně fyzický běh kompresoru.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
