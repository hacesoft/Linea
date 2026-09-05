[🇨🇿 **Česky**](04_DATOVY_MODEL.md) | [🇬🇧 English](../../en/api/04_DATA_MODEL.md)

# Datový model LINEA API – schema 1

Referenční popis je záměrně oddělen od běžné JSON odpovědi. JSON obsahuje data a kompaktní mapu jednotek; dlouhé popisy, datové typy, rozlišení a omezení jsou pouze zde a v `openapi.yaml`.

## Jak číst tabulku

- **Typ** je typ hodnoty vracené API.
- **Jednotka** je jednoznačná jednotka. `1` znamená bezrozměrnou hodnotu.
- **Rozlišení** je nejmenší krok doložitelný z aktuálního zdroje nebo převodu. „dle zdroje“ znamená, že LINEA hodnotu dále nekvantuje.
- **Rozsah / limit** není automaticky hardwarový limit. Pokud API hodnotu neomezuje, je to výslovně uvedeno.
- **Null** říká, zda může hodnota chybět.

> Důležité: rozlišení není totéž jako přesnost měření. Například hodnota s krokem 0,01 °C nemusí mít přesnost ±0,01 °C.

## Pole odpovědi `GET /api/v1/status`

| Cesta | Typ | Jednotka | Rozlišení | Rozsah / limit | Null | Význam | Zdroj |
|---|---|---|---|---|---|---|---|
| `api.name` | string | — | — | — | ne | Název rozhraní. | LINEA API |
| `api.version` | string | — | — | — | ne | Verze implementace API. | LINEA API |
| `api.schema` | integer | 1 | 1 | — | ne | Verze datového schématu. | LINEA API |
| `api.readOnly` | boolean | — | false/true | — | ne | API je pouze pro čtení. | LINEA API |
| `system.timestamp` | string(date-time) | — | — | — | ne | Čas vytvoření odpovědi. | LINEA API |
| `system.sourceTimestamp` | string(date-time) | — | — | — | ne | Čas zdrojového snímku LINEA. | LINEA |
| `system.ageMs` | integer | ms | 1 ms | 0… | ano | Stáří zdrojového snímku. | Výpočet API |
| `system.stale` | boolean | — | false/true | — | ne | Příznak příliš starých zdrojových dat. | Výpočet API |
| `energy.pv.powerW` | number | W | 1 W* | dle zdroje | ano | Celkový okamžitý výkon FVE. | LINEA / MPPT |
| `energy.pv.strings[].powerW` | number | W | 1 W | dle zdroje | ano | Okamžitý výkon MPPT větve. | Modbus; výkon je zaokrouhlen na celé W |
| `energy.pv.strings[].pvVoltageV` | number | V | 0.01 V | dle zařízení | ano | FV napětí MPPT větve. | Modbus, raw / 100 |
| `energy.pv.strings[].pvCurrentA` | number | A | 0.1 A | dle zařízení | ano | Vypočtený proud FV větve. | Výpočet power/voltage, výstup na 0.1 A |
| `energy.pv.strings[].yieldTodayKWh` | number | kWh | 0.1 kWh** | ≥ 0 | ano | Dnešní výnos MPPT větve. | Modbus, raw / 10 |
| `energy.house.powerW` | number | W | dle zdroje | dle instalace | ano | Celkový okamžitý příkon objektu. | LINEA |
| `energy.house.phases.l1PowerW` | number | W | dle zdroje | dle instalace | ano | Okamžitý příkon fáze L1. | LINEA |
| `energy.house.phases.l2PowerW` | number | W | dle zdroje | dle instalace | ano | Okamžitý příkon fáze L2. | LINEA |
| `energy.house.phases.l3PowerW` | number | W | dle zdroje | dle instalace | ano | Okamžitý příkon fáze L3. | LINEA |
| `energy.grid.powerW` | number | W | dle zdroje | dle instalace | ano | Tok výkonu mezi objektem a sítí; + import, − export. | LINEA |
| `energy.grid.phases.l1PowerW` | number | W | dle zdroje | dle instalace | ano | Tok výkonu fáze L1; + import, − export. | LINEA |
| `energy.grid.phases.l2PowerW` | number | W | dle zdroje | dle instalace | ano | Tok výkonu fáze L2; + import, − export. | LINEA |
| `energy.grid.phases.l3PowerW` | number | W | dle zdroje | dle instalace | ano | Tok výkonu fáze L3; + import, − export. | LINEA |
| `energy.battery.socPct` | number | % | dle zdroje | oček. 0–100 % | ano | Aktuální stav nabití baterie. | Victron/LINEA |
| `energy.battery.powerW` | number | W | 1 W*** | dle systému | ano | Okamžitý výkon baterie; + nabíjení, − vybíjení. | Modbus/LINEA |
| `energy.battery.currentA` | number | A | 0.1 A | zdrojově signed INT16 / 10 | ano | Okamžitý proud baterie. | Modbus, signed raw / 10 |
| `energy.battery.voltageV` | number | V | 0.1 V | zdrojově raw / 10 | ano | Napětí baterie. | Modbus, raw / 10 |
| `energy.battery.batteryLifeSocLimitPct` | number | % | 0.1 % | oček. 0–100 % | ano | Spodní SOC limit BatteryLife/ESS. | Modbus, raw / 10 |
| `ess.decision.gridPointW` | number | W | 1 W*** | dle konfigurace | ne | Výsledný požadovaný Grid Point. | LINEA |
| `ess.decision.pvSurplusW` | number | W | 1 W*** | dle instalace | ne | Interní přebytek/nedostatek FV pro rozhodování. | LINEA |
| `ess.decision.predictionActive` | boolean | — | false/true | — | ne | Predikční logika je v rozhodnutí aktivní. | LINEA |
| `ess.decision.exportAllowed` | boolean | — | false/true | — | ne | Aktuální rozhodnutí dovoluje export. | LINEA |
| `ess.decision.reason.code` | string | — | — | enum se může rozšířit | ne | Strojový kód důvodu rozhodnutí. | LINEA |
| `ess.decision.reason.text` | string\|null | — | — | — | ano | Volitelný lidský popis důvodu. | LINEA |
| `ess.settings.balancingReserveW` | number | W | 1 W*** | dle konfigurace | ne | Vyrovnávací výkonová rezerva. | Konfigurace LINEA |
| `ess.settings.setGridValueW` | number | W | 1 W*** | dle konfigurace | ne | Uživatelská cílová hodnota sítě. | Konfigurace LINEA |
| `ess.settings.maxGridPointW` | number | W | 1 W*** | dle konfigurace/hardware | ne | Limit Grid Pointu používaný logikou. | Konfigurace LINEA |
| `ess.settings.spotThresholdPrice` | number | CZK/kWh | dle zdroje/nastavení | bez API limitu | ne | Cenový práh SPOT strategie. | Konfigurace LINEA |
| `ess.settings.morningSocSalesPct` | number | % | dle nastavení | 0–100 % oček. | ne | SOC hranice ranní strategie. | Konfigurace LINEA |
| `ess.settings.eveningSocSalesPct` | number | % | dle nastavení | 0–100 % oček. | ne | SOC hranice večerní strategie. | Konfigurace LINEA |
| `ess.settings.gridChargingSocPct` | number | % | dle nastavení | 0–100 % oček. | ne | Cílové SOC pro nabíjení ze sítě. | Konfigurace LINEA |
| `ess.settings.predictionThresholdKWh` | number | kWh | dle nastavení | ≥ 0 oček. | ne | Energetický práh predikční funkce. | Konfigurace LINEA |
| `ess.settings.socDeltaBeforeExportPct` | number | % | dle nastavení | 0–100 % oček. | ne | SOC rozdíl/hranice před exportem. | Konfigurace LINEA |
| `ess.settings.chargingDurationGridH` | number | h | dle nastavení | ≥ 0 oček. | ne | Délka nabíjení ze sítě. | Konfigurace LINEA |
| `ess.settings.acceptablePriceGrid` | number | CZK/kWh | dle zdroje/nastavení | bez API limitu | ne | Maximální přijatelná cena pro GRID charging. | Konfigurace LINEA |
| `ess.time.delayCharging.start` | string | HH:MM | 1 min | 00:00–23:59 | ne | Začátek okna Delay Charging. | LINEA |
| `ess.time.delayCharging.stop` | string | HH:MM | 1 min | 00:00–23:59 | ne | Konec okna Delay Charging. | LINEA |
| `ess.time.delayCharging.startMs` | number | ms | 1 ms | 0–<86400000 oček. | ne | Začátek okna jako milisekundy od půlnoci. | LINEA |
| `ess.time.delayCharging.stopMs` | number | ms | 1 ms | 0–<86400000 oček. | ne | Konec okna jako milisekundy od půlnoci. | LINEA |
| `ess.time.morningPeakHours[]` | integer | h | 1 h | 0–23 | ne | Hodiny ranní špičky. | LINEA |
| `ess.time.eveningPeakHours[]` | integer | h | 1 h | 0–23 | ne | Hodiny večerní špičky. | LINEA |
| `spot.currentPrice` | number | CZK/kWh | dle zdroje | může být záporná | ano | Aktuální SPOT cena používaná LINEA. | Delta Green / LINEA |
| `forecast.solarYieldForecastKWh` | number | kWh | dle zdroje | ≥ 0 oček. | ano | Predikovaná výroba FV. | Forecast zdroj |
| `forecast.consumptionForecastKWh` | number | kWh | dle zdroje | ≥ 0 oček. | ano | Predikovaná spotřeba. | Forecast zdroj |
| `weather.rainProbabilityPct` | number | % | dle zdroje | 0–100 % oček. | ano | Pravděpodobnost srážek. | Weather zdroj |
| `vrm.data.today.pvYieldKWh` | number | kWh | dle VRM | ≥ 0 | ano | Dnešní výroba FV. | Victron VRM |
| `vrm.data.today.consumptionKWh` | number | kWh | dle VRM | ≥ 0 | ano | Dnešní spotřeba. | Victron VRM |
| `vrm.data.today.gridImportKWh` | number | kWh | dle VRM | ≥ 0 | ano | Dnešní import ze sítě. | Victron VRM |
| `vrm.data.today.gridExportKWh` | number | kWh | dle VRM | ≥ 0 | ano | Dnešní export do sítě. | Victron VRM |
| `vrm.data.today.batteryChargeKWh` | number | kWh | 0.001 kWh**** | ≥ 0 | ano | Kumulativní nabitá energie od restartu Node-RED. | LINEA čítač Wh / 1000 |
| `vrm.data.today.batteryDischargeKWh` | number | kWh | 0.001 kWh**** | ≥ 0 | ano | Kumulativní vybitá energie od restartu Node-RED. | LINEA čítač Wh / 1000 |
| `temperatures.data.racks[].temperatureC` | number | degC | 0.01 °C | dle čidla | ano | Teplota racku/baterie. | Modbus, raw / 100 |
| `temperatures.data.inverters[].temperatureC` | number | degC | 0.01 °C | dle čidla | ano | Teplota měniče. | Modbus, raw / 100 |
| `temperatures.data.other[].temperatureC` | number | degC | 0.01 °C | dle čidla | ano | Jiná teplota. | Modbus, raw / 100 |
| `shelly.data.devices[].channel` | integer | 1 | 1 | ≥ 0 | ne | Číslo kanálu Shelly. | Shelly |
| `shelly.data.smokeDetectors[].batteryPct` | number | % | dle Shelly | 0–100 % oček. | ano | Stav baterie kouřového čidla. | Shelly |
| `shelly.data.smokeDetectors[].batteryVoltageV` | number | V | dle Shelly | dle zařízení | ano | Napětí baterie kouřového čidla. | Shelly |
| `shelly.data.smokeDetectors[].rssiDbm` | number | dBm | 1 dBm typicky | dle Wi-Fi | ano | Síla Wi-Fi signálu. | Shelly |
| `shelly.data.smokeDetectors[].ageSec` | integer | s | 1 s | ≥ 0 | ano | Stáří posledního hlášení. | Výpočet LINEA |
| `ups.data.battery.chargePct` | number | % | dle NUT/UPS | 0–100 % oček. | ano | Nabití UPS baterie. | NUT |
| `ups.data.battery.voltageV` | number\|null | V | dle NUT/UPS | dle UPS | ano | Napětí UPS baterie. | NUT |
| `ups.data.battery.runtimeSec` | number\|null | s | dle NUT/UPS | ≥ 0 | ano | Odhad zbývajícího runtime. | NUT |
| `ups.data.input.voltageV` | number\|null | V | dle NUT/UPS | dle UPS | ano | Vstupní napětí UPS. | NUT |
| `ups.data.output.voltageV` | number\|null | V | dle NUT/UPS | dle UPS | ano | Výstupní napětí UPS. | NUT |
| `ups.data.output.frequencyHz` | number\|null | Hz | dle NUT/UPS | dle UPS | ano | Výstupní frekvence UPS. | NUT |
| `ups.data.load.percent` | number\|null | % | dle NUT/UPS | 0–100 % běžně | ano | Zatížení UPS. | NUT |
| `ups.data.load.realPowerW` | number\|null | W | dle NUT/UPS | ≥ 0 běžně | ano | Reálný výkon UPS zátěže. | NUT |
| `climate.data.devices[].roomTemperatureC` | number\|null | degC | dle Onecta | dle jednotky | ano | Pokojová teplota. | Daikin Onecta |
| `climate.data.devices[].outdoorTemperatureC` | number\|null | degC | dle Onecta | dle jednotky | ano | Venkovní teplota. | Daikin Onecta |
| `climate.data.devices[].setpointC` | number\|null | degC | dle Onecta | dle režimu/jednotky | ano | Požadovaná teplota. | Daikin Onecta |
| `climate.data.devices[].energy.todayKWh` | number\|null | kWh | dle Onecta | ≥ 0 | ano | Energie klimatizace za dnešek. | Daikin Onecta |
| `climate.data.devices[].energy.weekKWh` | number\|null | kWh | dle Onecta | ≥ 0 | ano | Energie za týdenní období. | Daikin Onecta |
| `climate.data.devices[].energy.monthKWh` | number\|null | kWh | dle Onecta | ≥ 0 | ano | Energie za měsíční období. | Daikin Onecta |
| `climate.data.devices[].energy.coolingMonthKWh` | number\|null | kWh | dle Onecta | ≥ 0 | ano | Měsíční energie chlazení. | Daikin Onecta |
| `climate.data.devices[].energy.heatingMonthKWh` | number\|null | kWh | dle Onecta | ≥ 0 | ano | Měsíční energie topení. | Daikin Onecta |

## Poznámky k rozlišení

`*` Celkový FV výkon může pocházet přímo z telemetry nebo jako součet MPPT větví. V fallbacku jsou MPPT výkony celé watty.

`**` MPPT yield je ve zdroji škálován `raw / 10`; následné formátování na dvě desetinná místa nezvyšuje skutečné rozlišení nad 0,1 kWh.

`***` Aktuální LINEA snapshot převádí tyto hodnoty přes `Number(...)`; pokud zdroj dodává celé watty, API je zachová. API samo další jemnější rozlišení negarantuje.

`****` Bateriové čítače jsou v LINEA vedeny ve Wh a API je převádí dělením 1000 na kWh. Jsou kumulativní od restartu Node-RED, nikoli garantované kalendářní denní čítače.

## Jednotky přímo v JSON

Schema 1 přidává top-level objekt `units`, kde je pro každou číselnou veličinu uvedena jednotka podle cesty, například:

```json
{
  "units": {
    "energy.battery.currentA": "A",
    "energy.battery.voltageV": "V",
    "spot.currentPrice": "CZK/kWh",
    "temperatures.data.racks[].temperatureC": "degC"
  }
}
```

Popis pole, datový typ, rozlišení a omezení zůstávají v dokumentaci/OpenAPI, aby se běžná odpověď zbytečně nenafukovala.

[← LINEA API](PREHLED.md) · [Konvence →](05_KONVENCE_A_STARI_DAT.md)

## SPOT ceny pro grafy

Sekce `spot` obsahuje aktuální cenu a hodinové ceny pro dnešní a následující den.

| Cesta | Typ | Jednotka | Význam |
|---|---|---|---|
| `spot.currentPrice` | number \| null | CZK/kWh | Aktuální SPOT cena |
| `spot.intervalMinutes` | integer | min | Časový krok cen; aktuálně 60 minut |
| `spot.today.date` | string | — | Datum dne ve formátu `YYYY-MM-DD` |
| `spot.today.available` | boolean | — | Dostupnost dnešního cenového pole |
| `spot.today.prices[]` | array<object> | — | Hodinové ceny dneška |
| `spot.today.prices[].hour` | integer | h | Hodina 0–23 |
| `spot.today.prices[].price` | number \| null | CZK/kWh | Cena pro danou hodinu |
| `spot.tomorrow.date` | string | — | Datum následujícího dne |
| `spot.tomorrow.available` | boolean | — | `true`, pokud už jsou ceny následujícího dne dostupné |
| `spot.tomorrow.prices[]` | array<object> | — | Hodinové ceny následujícího dne |
| `spot.tomorrow.prices[].hour` | integer | h | Hodina 0–23 |
| `spot.tomorrow.prices[].price` | number \| null | CZK/kWh | Cena pro danou hodinu |

Pokud ceny následujícího dne ještě nejsou zdrojem zveřejněny, `spot.tomorrow.available` je `false` a `spot.tomorrow.prices` je prázdné pole.

## VRM predikce pro grafy

LINEA již načítá forecast z Victron VRM v intervalu `15mins`. API proto vedle souhrnných hodnot zveřejňuje i časové řady vhodné přímo pro grafy.

| Cesta | Typ | Jednotka | Význam |
|---|---|---|---|
| `forecast.source` | string | — | Zdroj predikce; `Victron VRM` |
| `forecast.intervalMinutes` | integer | min | Časový krok predikce; 15 minut |
| `forecast.solarYieldForecastKWh` | number \| null | kWh | Souhrnná predikce FV výroby |
| `forecast.consumptionForecastKWh` | number \| null | kWh | Souhrnná predikce spotřeby |
| `forecast.series.solarYield[]` | array<object> | — | Časová řada predikované FV energie |
| `forecast.series.solarYield[].timestampMs` | integer \| null | ms | Unix timestamp v milisekundách |
| `forecast.series.solarYield[].timestamp` | string \| null | — | ISO 8601 čas |
| `forecast.series.solarYield[].energyKWh` | number \| null | kWh | Predikovaná energie v daném intervalu |
| `forecast.series.consumption[]` | array<object> | — | Časová řada predikované spotřeby |
| `forecast.series.consumption[].timestampMs` | integer \| null | ms | Unix timestamp v milisekundách |
| `forecast.series.consumption[].timestamp` | string \| null | — | ISO 8601 čas |
| `forecast.series.consumption[].energyKWh` | number \| null | kWh | Predikovaná energie v daném intervalu |

Hodnoty časových řad jsou převáděny z Wh poskytovaných VRM na kWh. API neprovádí interpolaci ani dopočítávání chybějících bodů.

