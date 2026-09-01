[🇨🇿 **Česky**](04_DATOVY_MODEL.md) \| [🇬🇧
English](../../en/api/04_DATA_MODEL.md)

------------------------------------------------------------------------

# Datový model LINEA API -- schema 3

Tato kapitola je **referenční popis odpovědi `GET /api/v1/status`
položku po položce**. Uvedené příklady neobsahují skutečné IP adresy,
tokeny ani jiné citlivé konfigurační údaje. Hodnoty jsou pouze
ilustrační; rozhodující je význam pole, datový typ, jednotka a
znaménková konvence.

## 1. Kořen odpovědi

``` json
{
  "api": {},
  "system": {},
  "conventions": {},
  "energy": {},
  "ess": {},
  "spot": {},
  "forecast": {},
  "solar": {},
  "weather": {},
  "vrm": {},
  "temperatures": {},
  "shelly": {},
  "ups": {},
  "climate": {}
}
```

Každý blok představuje samostatnou oblast dat. Bloky modulů mají
zpravidla `available`; klient se proto nemá spoléhat pouze na existenci
objektu.

## 2. `api` -- identifikace API

``` json
"api": { "name": "LINEA API", "version": "1.0-r2.3.3", "schema": 3, "readOnly": true }
```

  -----------------------------------------------------------------------
  Pole                    Typ                     Význam
  ----------------------- ----------------------- -----------------------
  `api.name`              string                  Název rozhraní.

  `api.version`           string                  Verze implementace
                                                  LINEA API. Nejde o
                                                  verzi LINEA FLOW ani
                                                  dokumentace.

  `api.schema`            number                  Verze struktury JSON.
                                                  Klient ji používá pro
                                                  kontrolu kompatibility
                                                  datového modelu.

  `api.readOnly`          boolean                 `true` potvrzuje, že
                                                  toto API pouze
                                                  publikuje stav a
                                                  neposkytuje řídicí
                                                  operace.
  -----------------------------------------------------------------------

## 3. `system` -- čas a čerstvost snímku

``` json
"system": { "timestamp": "...", "sourceTimestamp": "...", "ageMs": 781, "stale": false }
```

  ------------------------------------------------------------------------
  Pole                       Typ / jednotka         Význam
  -------------------------- ---------------------- ----------------------
  `system.timestamp`         ISO 8601 UTC           Čas vytvoření API
                                                    odpovědi.

  `system.sourceTimestamp`   ISO 8601 UTC           Čas zdrojového LINEA
                                                    snímku, ze kterého
                                                    byla odpověď
                                                    sestavena.

  `system.ageMs`             ms                     Stáří zdrojového
                                                    snímku v okamžiku
                                                    vytvoření odpovědi.

  `system.stale`             boolean                `true` znamená, že
                                                    zdrojová data jsou
                                                    podle pravidel API
                                                    příliš stará.
  ------------------------------------------------------------------------

## 4. `conventions` -- znaménkové konvence

``` json
"conventions": {
  "gridPower": { "positive": "import", "negative": "export", "unit": "W" },
  "batteryPower": { "positive": "charging", "negative": "discharging", "unit": "W" }
}
```

`gridPower`: kladná hodnota znamená odběr ze sítě, záporná dodávku do
sítě. `batteryPower`: kladná hodnota znamená nabíjení baterie, záporná
vybíjení. Klient má tyto konvence respektovat a nemá znaménka obracet
podle vlastní domněnky.

## 5. `energy` -- okamžitý energetický stav

### 5.1 Dostupnost

`energy.available` (boolean) říká, zda je energetický blok aktuálně
dostupný.

### 5.2 `energy.pv`

  -----------------------------------------------------------------------
  Pole                        Typ / jednotka        Význam
  --------------------------- --------------------- ---------------------
  `energy.pv.powerW`          W                     Celkový okamžitý
                                                    výkon FVE.

  `energy.pv.strings[]`       array                 Seznam jednotlivých
                                                    MPPT/FV větví.

  `strings[].name`            string                Uživatelský název
                                                    větve/regulátoru.

  `strings[].powerW`          W                     Okamžitý výkon dané
                                                    FV větve.

  `strings[].pvVoltageV`      V                     FV napětí dané větve.

  `strings[].pvCurrentA`      A                     FV proud dané větve.

  `strings[].yieldTodayKWh`   kWh                   Energie vyrobená
                                                    danou větví od
                                                    začátku dne.
  -----------------------------------------------------------------------

### 5.3 `energy.house`

  Pole                             Typ / jednotka   Význam
  -------------------------------- ---------------- ----------------------------------
  `energy.house.powerW`            W                Celkový okamžitý příkon objektu.
  `energy.house.phases.l1PowerW`   W                Příkon fáze L1.
  `energy.house.phases.l2PowerW`   W                Příkon fáze L2.
  `energy.house.phases.l3PowerW`   W                Příkon fáze L3.

### 5.4 `energy.grid`

  -------------------------------------------------------------------------
  Pole                            Typ / jednotka       Význam
  ------------------------------- -------------------- --------------------
  `energy.grid.powerW`            W                    Celkový tok výkonu
                                                       mezi objektem a
                                                       distribuční sítí.
                                                       Kladně import,
                                                       záporně export.

  `energy.grid.phases.l1PowerW`   W                    Tok výkonu na L1;
                                                       stejná znaménková
                                                       konvence.

  `energy.grid.phases.l2PowerW`   W                    Tok výkonu na L2.

  `energy.grid.phases.l3PowerW`   W                    Tok výkonu na L3.
  -------------------------------------------------------------------------

### 5.5 `energy.battery`

  -------------------------------------------------------------------------------
  Pole                                      Typ / jednotka     Význam
  ----------------------------------------- ------------------ ------------------
  `energy.battery.socPct`                   \%                 Aktuální stav
                                                               nabití baterie
                                                               (SOC).

  `energy.battery.powerW`                   W                  Okamžitý výkon
                                                               baterie. Kladně
                                                               nabíjení, záporně
                                                               vybíjení.

  `energy.battery.currentA`                 A                  Proud baterie;
                                                               znaménko odpovídá
                                                               směru toku podle
                                                               zdrojových dat.

  `energy.battery.voltageV`                 V                  Aktuální napětí
                                                               baterie.

  `energy.battery.batteryLifeSocLimitPct`   \%                 Aktuální spodní
                                                               SOC limit
                                                               používaný funkcí
                                                               BatteryLife/ESS.
                                                               Nejde o aktuální
                                                               SOC.
  -------------------------------------------------------------------------------

## 6. `ess` -- stav a rozhodnutí řídicí logiky

`ess.available` říká, zda je blok ESS k dispozici.

### 6.1 `ess.decision`

  ----------------------------------------------------------------------------
  Pole                              Typ / jednotka       Význam
  --------------------------------- -------------------- ---------------------
  `ess.decision.gridPointW`         W                    Výsledný požadovaný
                                                         Grid Point vypočtený
                                                         LINEA.

  `ess.decision.pvSurplusW`         W                    Interně vypočtená
                                                         hodnota
                                                         přebytku/nedostatku
                                                         FV používaná řídicí
                                                         logikou. Znaménko je
                                                         nutné interpretovat
                                                         podle aktuální logiky
                                                         LINEA, nikoli jako
                                                         `gridPower`.

  `ess.decision.predictionActive`   boolean              Zda je v aktuálním
                                                         rozhodnutí aktivní
                                                         predikční logika.

  `ess.decision.exportAllowed`      boolean              Zda aktuální
                                                         rozhodnutí dovoluje
                                                         export energie do
                                                         sítě.

  `ess.decision.reason.code`        string               Strojově čitelný kód
                                                         důvodu rozhodnutí.
                                                         `UNCLASSIFIED`
                                                         znamená, že současná
                                                         logika nepřiřadila
                                                         specifičtější kód.

  `ess.decision.reason.text`        string/null          Volitelný lidsky
                                                         čitelný popis důvodu.
                                                         `null` znamená, že
                                                         text není k
                                                         dispozici.
  ----------------------------------------------------------------------------

### 6.2 `ess.switches`

Každé pole je boolean a odpovídá aktuálnímu stavu dané funkce LINEA.

  ---------------------------------------------------------------------
  Pole                               Význam
  ---------------------------------- ----------------------------------
  `controlModeEssAcGrid`             `true`: řízení používá ESS AC/Grid
                                     režim s registry 2716/2717;
                                     `false`: legacy režim registru
                                     2700.

  `spotGridCharging`                 Povolení automatického nabíjení ze
                                     sítě podle SPOT strategie.

  `gridCharging`                     Stav/povolení obecného nabíjení
                                     baterie ze sítě.

  `gridConsumption`                  Povolení odběru ze sítě v
                                     příslušné řídicí logice.

  `energyThresholdInjector`          Stav funkce Energy Threshold
                                     Injector.

  `nonBatteryPriority`               Stav režimu Non Battery Priority.

  `delayCharging`                    Stav funkce odloženého nabíjení.

  `dynamicSocReserve`                Stav dynamické SOC rezervy.

  `predictionThreshold`              Stav použití prahu energetické
                                     predikce.

  `socDeltaBeforeExport`             Stav podmínky SOC rozdílu před
                                     povolením exportu.

  `morningPeakBatterySales`          Stav ranní strategie
                                     prodeje/vybíjení baterie.

  `eveningPeakBatterySales`          Stav večerní strategie
                                     prodeje/vybíjení baterie.
  ---------------------------------------------------------------------

### 6.3 `ess.settings`

  ----------------------------------------------------------------------------
  Pole                                         Jednotka Význam
  --------------------------- ------------------------- ----------------------
  `balancingReserveW`                                 W Rezerva používaná při
                                                        výpočtu řízení Grid
                                                        Pointu.

  `setGridValueW`                                     W Uživatelsky nastavená
                                                        cílová hodnota sítě.

  `maxGridPointW`                                     W Maximální/limitní Grid
                                                        Point používaný
                                                        logikou; znaménko
                                                        zachovává interní
                                                        konvenci LINEA.

  `spotThresholdPrice`              dle cenového zdroje Cenový práh SPOT
                                                        strategie. API zde
                                                        pouze publikuje
                                                        hodnotu nastavení.

  `morningSocSalesPct`                               \% SOC hranice pro ranní
                                                        strategii
                                                        prodeje/vybíjení.

  `eveningSocSalesPct`                               \% SOC hranice pro
                                                        večerní strategii.

  `gridChargingSocPct`                               \% Cílové/limitní SOC pro
                                                        nabíjení ze sítě.

  `predictionThresholdKWh`                          kWh Energetický práh
                                                        používaný predikční
                                                        funkcí.

  `socDeltaBeforeExportPct`                          \% SOC rozdíl/hranice
                                                        používaná před
                                                        exportem.

  `chargingDurationGridH`                             h Nastavená délka
                                                        nabíjení ze sítě.

  `acceptablePriceGrid`             dle cenového zdroje Maximální/přijatelná
                                                        cena pro příslušnou
                                                        GRID nabíjecí logiku.
  ----------------------------------------------------------------------------

### 6.4 `ess.time`

  -----------------------------------------------------------------------
  Pole                      Typ                    Význam
  ------------------------- ---------------------- ----------------------
  `delayCharging.start`     `HH:MM`                Čas začátku okna Delay
                                                   Charging pro
                                                   zobrazení.

  `delayCharging.stop`      `HH:MM`                Čas konce okna Delay
                                                   Charging.

  `delayCharging.startMs`   ms od 00:00            Stejný začátek v
                                                   interním číselném
                                                   formátu.

  `delayCharging.stopMs`    ms od 00:00            Stejný konec v
                                                   interním číselném
                                                   formátu.

  `morningPeakHours[]`      hodina 0--23           Hodiny považované za
                                                   ranní špičku.

  `eveningPeakHours[]`      hodina 0--23           Hodiny považované za
                                                   večerní špičku.
  -----------------------------------------------------------------------

## 7. `spot`

  -----------------------------------------------------------------------
  Pole                    Typ / jednotka          Význam
  ----------------------- ----------------------- -----------------------
  `spot.available`        boolean                 Dostupnost aktuální
                                                  SPOT ceny.

  `spot.currentPrice`     číslo                   Aktuální cena použitá
                                                  LINEA. Jednotku/měnu je
                                                  nutné převzít z
                                                  konfigurace a zdroje
                                                  cen; schema 3 ji v
                                                  tomto poli samostatně
                                                  neposílá.
  -----------------------------------------------------------------------

## 8. `forecast`

  ------------------------------------------------------------------------
  Pole                                        Jednotka Význam
  -------------------------- ------------------------- -------------------
  `forecast.available`                         boolean Dostupnost
                                                       predikce.

  `solarYieldForecastKWh`                          kWh Predikovaná výroba
                                                       FV pro období
                                                       používané LINEA.

  `consumptionForecastKWh`                         kWh Predikovaná
                                                       spotřeba pro období
                                                       používané LINEA.
  ------------------------------------------------------------------------

## 9. `solar`

  Pole                Význam
  ------------------- ----------------------------------
  `solar.available`   Dostupnost astronomických údajů.
  `solar.sunrise`     Čas východu slunce.
  `solar.sunset`      Čas západu slunce.
  `solar.dayLength`   Textově formátovaná délka dne.

## 10. `weather`

  ---------------------------------------------------------------------
  Pole                               Význam
  ---------------------------------- ----------------------------------
  `weather.available`                Dostupnost dat počasí.

  `weather.today`                    Stručný textový popis
                                     aktuálního/dnešního počasí.

  `weather.rainProbabilityPct`       Pravděpodobnost srážek v %.

  `weather.trend`                    Textový/symbolický trend počasí
                                     poskytovaný zdrojovou logikou.
  ---------------------------------------------------------------------

## 11. `vrm` -- denní energetické součty

`vrm.available` udává dostupnost bloku. `vrm.data.updatedAt` je čas
aktualizace a `vrm.data.source` textově identifikuje zdroj dat.

  -----------------------------------------------------------------------
  Pole `vrm.data.today.*`                   Jednotka Význam
  ----------------------- -------------------------- --------------------
  `pvYieldKWh`                                   kWh Dnešní výroba FV.

  `consumptionKWh`                               kWh Dnešní spotřeba
                                                     objektu.

  `gridImportKWh`                                kWh Dnešní energie
                                                     odebraná ze sítě.

  `gridExportKWh`                                kWh Dnešní energie
                                                     dodaná do sítě.

  `batteryChargeKWh`                             kWh Kumulativní energie
                                                     nabitá do baterie
                                                     podle LINEA čítače.

  `batteryDischargeKWh`                          kWh Kumulativní energie
                                                     vybitá z baterie
                                                     podle LINEA čítače.
  -----------------------------------------------------------------------

**Pozor:** současné `batteryChargeKWh` a `batteryDischargeKWh` vycházejí
z čítačů LINEA, které jsou kumulativní od restartu Node-RED a po
restartu se vynulují. Nejsou to spolehlivé kalendářní denní čítače,
dokud nebude doplněna perzistence/obnova.

## 12. `temperatures`

`temperatures.available` udává dostupnost a
`temperatures.data.updatedAt` čas poslední aktualizace.

-   `racks[]`: `name`, `temperatureC` -- název místa/senzoru a teplota
    racku v °C.
-   `inverters[]`: `name`, `temperatureC` -- název měniče a jeho teplota
    v °C.
-   `other[]`: stejný obecný prostor pro další teplotní senzory; může
    být prázdný.

## 13. `shelly`

### 13.1 Zařízení `shelly.data.devices[]`

  ---------------------------------------------------------------------
  Pole                               Význam
  ---------------------------------- ----------------------------------
  `name`                             Uživatelský název zařízení/kanálu.

  `kind`                             Typ kanálu, např. `output` nebo
                                     `input`.

  `channel`                          Číslo kanálu v zařízení.

  `state`                            Logický stav kanálu. Význam ON/OFF
                                     závisí na `kind` a konkrétním
                                     zapojení.

  `available`                        Zda jsou data zařízení/kanálu
                                     dostupná.
  ---------------------------------------------------------------------

`shelly.data.updatedAt` je čas poslední aktualizace celého Shelly bloku.

### 13.2 Kouřová čidla `shelly.data.smokeDetectors[]`

  -----------------------------------------------------------------------
  Pole                    Typ / jednotka          Význam
  ----------------------- ----------------------- -----------------------
  `name`                  string                  Uživatelský název
                                                  čidla.

  `alarm`                 boolean                 Aktivní kouřový alarm.

  `ok`                    boolean                 Souhrnný stav čidla
                                                  podle LINEA.

  `batteryPct`            \%                      Stav baterie.

  `batteryVoltageV`       V                       Napětí baterie.

  `rssiDbm`               dBm                     Síla Wi-Fi signálu.
                                                  Více záporná hodnota
                                                  znamená slabší signál.

  `wakeupReason`          string                  Důvod posledního
                                                  probuzení zařízení.

  `lastSeen`              ISO 8601 UTC            Čas posledního
                                                  přijatého hlášení.

  `ageSec`                s                       Stáří posledního
                                                  hlášení.
  -----------------------------------------------------------------------

## 14. `ups`

`ups.available` udává dostupnost modulu a `ups.data.updatedAt` čas
poslední úspěšné aktualizace.

  ------------------------------------------------------------------------
  Pole                                        Jednotka Význam
  ------------------------- -------------------------- -------------------
  `ups.data.name`                                   -- Název/model UPS
                                                       publikovaný
                                                       modulem.

  `online`                                     boolean UPS je v
                                                       online/síťovém
                                                       stavu.

  `onBattery`                                  boolean UPS právě napájí z
                                                       baterie.

  `status.raw`                                      -- Původní NUT status,
                                                       např. `OL`, `OB`,
                                                       `LB`.

  `status.lowBattery`                          boolean NUT hlásí nízkou
                                                       baterii.

  `status.charging`                            boolean Baterie se nabíjí.

  `status.discharging`                         boolean Baterie se vybíjí.

  `status.overload`                            boolean UPS hlásí
                                                       přetížení.

  `status.replaceBattery`                      boolean UPS hlásí požadavek
                                                       na výměnu baterie.

  `status.bypass`                              boolean UPS je v bypass
                                                       režimu.

  `battery.chargePct`                               \% Stav nabití
                                                       baterie.

  `battery.voltageV`                            V/null Napětí baterie,
                                                       pokud jej NUT
                                                       poskytuje; jinak
                                                       `null`.

  `battery.runtimeSec`                               s Odhad zbývající
                                                       doby provozu na
                                                       baterii.

  `input.voltageV`                                   V Vstupní síťové
                                                       napětí.

  `output.voltageV`                                  V Výstupní napětí
                                                       UPS.

  `output.frequencyHz`                              Hz Výstupní frekvence.

  `load.percent`                                    \% Zatížení UPS vůči
                                                       jmenovitému výkonu.

  `load.realPowerW`                                  W Aktuální reálný
                                                       výkon zátěže.
  ------------------------------------------------------------------------

## 15. `climate`

`climate.available` udává dostupnost Daikin modulu.
`climate.data.updatedAt` je čas aktualizace a `climate.data.devices[]`
obsahuje jednotlivé klimatizační jednotky.

  ------------------------------------------------------------------------
  Pole zařízení              Typ / jednotka         Význam
  -------------------------- ---------------------- ----------------------
  `name`                     string                 Uživatelský název
                                                    jednotky.

  `cloudUp`                  boolean                Jednotka/gateway je
                                                    podle Onecta cloudu
                                                    dostupná.

  `on`                       boolean                Zapnutí klimatizace.

  `operationMode`            string                 Provozní režim, např.
                                                    `cooling`.

  `roomTemperatureC`         °C                     Naměřená pokojová
                                                    teplota.

  `outdoorTemperatureC`      °C                     Venkovní teplota
                                                    hlášená
                                                    jednotkou/cloudem.

  `setpointC`                °C                     Aktuální požadovaná
                                                    teplota.

  `energy.unit`              string                 Jednotka energetických
                                                    čítačů, aktuálně
                                                    `kWh`.

  `energy.todayKWh`          kWh                    Energie klimatizace za
                                                    dnešek podle Onecta
                                                    dat.

  `energy.weekKWh`           kWh                    Energie za
                                                    aktuální/poskytované
                                                    týdenní období.

  `energy.monthKWh`          kWh                    Celková energie za
                                                    poskytované měsíční
                                                    období.

  `energy.coolingMonthKWh`   kWh                    Měsíční energie
                                                    chlazení.

  `energy.heatingMonthKWh`   kWh                    Měsíční energie
                                                    topení.

  `error`                    boolean                Jednotka hlásí chybu.

  `errorCode`                string/null            Kód chyby poskytovaný
                                                    Daikin daty.

  `firmwareVersion`          string                 Aktuálně zjištěná
                                                    verze firmware.

  `firmwareChanged`          boolean                LINEA zjistila změnu
                                                    firmware proti dříve
                                                    známé hodnotě.
  ------------------------------------------------------------------------

## 16. `available`, `null`, prázdná pole a klientská pravidla

-   `available: false` znamená, že klient nemá hodnoty daného modulu
    považovat za aktuálně dostupné.
-   `null` znamená „hodnota není k dispozici / zdroj ji neposkytl",
    nikoli automaticky nulu.
-   Prázdné pole `[]` znamená, že pro danou kategorii nejsou v aktuálním
    snímku položky.
-   Klient nemá z chybějícího volitelného pole odvozovat havárii celého
    API.
-   Časové údaje `updatedAt`, `timestamp`, `sourceTimestamp` slouží k
    posouzení stáří dat; jednotlivé moduly mohou mít různé časy
    aktualizace.

## 17. Co API záměrně neobsahuje

Veřejný status nesmí obsahovat API tokeny, OAuth access/refresh tokeny,
Client Secret, hesla, interní konfigurační soubory ani jiné údaje
potřebné k řízení či autentizaci. LINEA API schema 3 je monitorovací
rozhraní.

------------------------------------------------------------------------

[← LINEA API](README.md) · [Konvence a stáří dat
→](05_KONVENCE_A_FRESHNESS.md)
