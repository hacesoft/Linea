[🇨🇿 **Česky**](15_KONFIGURACE.md) | [🇬🇧 English](../en/15_CONFIGURATION.md)

---

# Konfigurace LINEA

## Centrální konfigurace

LINEA používá centrální objekt `config`. Uživatelské nastavení z Dashboardu se ukládá do tohoto objektu a následně je používá řídicí logika.

## Verze

```javascript
flow_version: "DDMMYYYY_HHMM"
```

Update checker používá `global.linea_version_local`, naplněnou při inicializaci z `config.flow_version`, a porovnává datum i čas s názvy souborů v `release/`. Verze flow, `configSchemaVersion` a `api.schema` označují různé věci.

## ESS parametry

Mezi hlavní parametry patří:

```text
nControl_Mode_ESS_AC_Grid
nGridConsumptionEnable
nSetGridValue
nEnergyThresholdInjector
nNonBatteryPriorityMode
switchSpotGridCharging
nCharging_DurationGRID
nAcceptable_Price_GRID
switchDelayCharging
tStartBattery
tStopBattery
nSOCDeltaBeforeExport_Switch
nSOCDeltaBeforeExport
nDynamicSOC_Reserve
nMorningPeakBatterySales
nMorningSOC_sales
nPredictionThreshold
sPredictionThresholdKW
nEveningPeakBatterySales
nEveningSOC_sales
sGridChargingSwitch
nGridChargingSOC
nMAX_Grid_Point
nBalancingReserve
nSetPeak
nHouseHourlyConsumption
nSunriseProductionOffset
isSpotAutoCtrlEnabled
spotTresholdPrice
```

Význam jednotlivých parametrů je popsán v [referenci nastavení](11_REFERENCE_NASTAVENI.md).

## Modbus připojení

```javascript
connectorConfig: {
    tcpHost: "...",
    tcpPort: "502",
    unitId: "..."
}
```

- `tcpHost` – IP adresa Modbus TCP zařízení;
- `tcpPort` – Modbus TCP port;
- `unitId` – Modbus Unit ID.

## VRM

VRM konfigurace obsahuje API token a Site ID. Tyto údaje slouží pro získání cloudových dat používaných v Dashboardu a vybraných výpočtech.

Citlivé tokeny se nemají zveřejňovat ve veřejném flow.

## Poloha

```javascript
location: {
    latitude: ...,
    longitude: ...
}
```

Poloha se používá pro časové/astronomické údaje a související výpočty.

## MQTT

MQTT broker, port, autentizace a TLS se nastavují ručně v konfiguračním uzlu Node-RED. Objekt `config.mqtt` nepovažujte za ovládání aktivního spojení. Přesný postup je v [Shelly — nastavení MQTT](integrace/SHELLY.md).

## Uložení konfigurace

Dashboard neposílá běžné konfigurační změny přímo do Modbusu. Tok je:

`UI` → `Widget Handler` → `centrální config` → `řídicí logika` → `výpočet setpointu` → `Modbus`

## Bezpečnost konfigurace

Ve veřejném repozitáři nemají být skutečné:

- API tokeny;
- Client Secret;
- refresh tokeny;
- hesla;
- privátní přístupové údaje.

---

[← Dokumentace LINEA](README.md)

## Moduly a hranice konfigurace
Aktivní volitelné moduly používají centrální `global.config` pro běžná nastavení (`daikinConfig`, `upsConfig`). Daikin OAuth tokeny jsou provozní tajemství s vlastním životním cyklem.

Modbus TCP parametry (`tcpHost`, `tcpPort`, `unitId`) lze spravovat v LINEA. MQTT připojení je ale Node-RED **MQTT config node**: LINEA jej nemá dynamicky přepisovat. Broker, port, TLS a autentizace se nastavují přímo v editoru Node-RED.
