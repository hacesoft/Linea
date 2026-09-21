[🇨🇿 **Česky**](11_REFERENCE_NASTAVENI.md) | [🇬🇧 English](../en/11_SETTINGS_REFERENCE.md)

---

# Referenční manuál nastavení LINEA

Tento dokument je referenční přehled hlavních ovládacích prvků současného LINEA flow.

Zdroj pravdy pro aktuální chování je současný Node-RED flow. Starší GitHub README byl použit jako doplňkový zdroj historického účelu a uživatelského vysvětlení jednotlivých funkcí.

---

## 1. Základní znaménková konvence

V aktuálním LINEA flow se při práci s Grid Pointem používá konvence:

```text
záporná hodnota = export / dodávka do sítě
kladná hodnota  = import / odběr ze sítě
0 W             = snaha o nulový tok
```

Při práci s historickou dokumentací nebo dokumentací konkrétního Victron registru vždy ověřte znaménkovou konvenci konkrétního Modbus rozhraní. LINEA obsahuje konverzní funkce a některé hodnoty před zápisem převádí.

---

# 2. ESS – hlavní ovládání

## Set Point

Konfigurační klíče:

```javascript
nGridConsumptionEnable
nSetGridValue
```

### Co dělá

Při zapnutém Set Pointu LINEA nastavuje požadovaný Grid Point na hodnotu `nSetGridValue`.

Typický příklad:

```text
-100 W
```

Systém se potom nesnaží oscilovat přesně kolem 0 W, ale kolem malého exportu 100 W. To může pomoci omezit nežádoucí krátkodobý odběr ze sítě.

### Rozsah v současném ESS widgetu

```text
-1000 až +1000 W
krok 10 W
```

Pole je aktivní pouze při zapnutém Set Pointu.

### Vazby

Set Point používá stejný cílový registr jako ostatní ESS funkce:

```text
Control Mode OFF → 2700
Control Mode ON  → 2716/2717
```

---

## Control Mode: ESS / AC Grid

Konfigurační klíč:

```javascript
nControl_Mode_ESS_AC_Grid
```

Toto je jeden z nejdůležitějších přepínačů LINEA.

### OFF – ESS Mode

```text
nControl_Mode_ESS_AC_Grid = false
```

Cílový registr:

```text
2700 – ESS Control Loop Setpoint
```

Datový typ ve flow:

```text
INT16
```

Jedná se o starší způsob řízení.

Větev 2700 je ve flow určena pro zápis při změně nastavení.

LINEA v tomto režimu používá **send-on-change** logiku:

```text
setpoint se změnil → zapsat
setpoint je stejný → nezapisovat
```

Výjimkou je změna samotného Control Mode – při změně cílového registru se aktuální hodnota vždy znovu odešle.

### ON – AC Grid Mode

```text
nControl_Mode_ESS_AC_Grid = true
```

Cílový registr:

```text
2716/2717 – AC Grid Set Point
```

Datový typ:

```text
INT32
```

32bitová hodnota je ve flow rozdělena na dvě 16bitová slova a zapisována od adresy 2716.

Tato větev je určena pro průběžně obnovované externí řízení.

### Proč se 2716/2717 zapisuje periodicky

U tohoto režimu je periodický zápis **záměrný a nutný**.

```text
500 W → 500 W → 500 W → 500 W ...
```

Externí controller tím obnovuje požadavek. Timeout a návratový setpoint při ztrátě obnovování je nutné ověřit pro konkrétní GX/firmware; nelze je považovat za nezávislou bezpečnostní záruku.

Proto zde nesmí být:

```text
RBE
send-only-on-change
deduplikace stejných hodnot
```

### Proč se 2700 naopak periodicky nezapisuje

Větev 2700 potlačuje opakované zápisy stejné hodnoty.

Současný `sendValueWithinLimits` proto odlišuje oba režimy:

```text
2700       → stejnou hodnotu neposílat
2716/2717  → stejnou hodnotu opakovaně posílat
```

### Požadavky AC Grid režimu


```text
Ověřte podporu registrů 2716/2717 v konkrétní verzi Venus OS a zařízení.
```

Před použitím je vhodné podporu ověřit pro konkrétní zařízení a firmware.

---

## Energy Threshold Injector

Konfigurační klíč:

```javascript
nEnergyThresholdInjector
```

### Účel

Funguje podobně jako Set Point, ale pouze tehdy, když existuje dostatečný přebytek z FV.

Současný widget popisuje podmínku přibližně:

```text
Zbytek FV > 0
SPOT cena > 0
AcPowerSetPoint < 0
```

Pak může být Grid Point nastaven na `AcPowerSetPoint`.

### Výpočet přebytku

```text
Zbytek FV =
PV výroba
-
(spotřeba domu + Balancing Reserve)
```

### Poznámka

Současný widget tuto funkci označuje jako specifickou/proprietární pro autorovu instalaci. Při použití v jiné FVE je vhodné nejprve přesně ověřit její účel.

---

## Non Battery Priority Mode

Konfigurační klíč:

```javascript
nNonBatteryPriorityMode
```

### Účel

Režim vznikl zejména pro velkou spotřebu, například:

```text
nabíjení elektromobilu
```

Cílem je omezit zbytečné vybíjení baterie a chybějící výkon raději odebrat z GRIDu.

Současná logika pracuje s rozdílem mezi:

```text
výrobou FV
spotřebou domu
výkonem baterie
```

Pokud je výsledný rozdíl záporný, převede jej na kladný Grid Point a požádá o odpovídající odběr ze sítě.

---

# 3. Nabíjení baterie

## GRID CHARGING

Konfigurační klíče:

```javascript
sGridChargingSwitch
nGridChargingSOC
```

### Účel

Okamžité nabití baterie ze sítě.

```text
GRID → baterie
```

Výkon je odvozen od:

```javascript
nMAX_Grid_Point
```

a znaménko si řídicí logika upraví podle směru toku.

### Cílové SOC

Baterie se nabíjí pouze do:

```text
nGridChargingSOC
```

Po dosažení cílového SOC se funkce sama vypne.

To je důležitý rozdíl:

```text
GRID CHARGING ≠ permanentní regulace SOC
```

Funkce baterii neudržuje kolem cílového SOC opakovaným zapínáním/vypínáním.

### Typické použití

- očekávaný výpadek sítě;
- potřeba rychle vytvořit energetickou rezervu;
- ruční příprava baterie.

---

## Spot-Grid Charging

Konfigurační klíče:

```javascript
switchSpotGridCharging
nCharging_DurationGRID
nAcceptable_Price_GRID
```

Další vazba:

```javascript
nMAX_Grid_Point
```

### Účel

Automaticky najde nejlevnější souvislý blok hodin a v tomto období nabíjí baterii ze sítě.

### `nCharging_DurationGRID`

Počet hodin nabíjení.

Algoritmus hledá:

```text
nejlevnější souvislý blok požadované délky
```

nikoli několik náhodných nejlevnějších hodin rozmístěných během dne.

### `nAcceptable_Price_GRID`

Maximální přijatelná SPOT cena pro nákup.

Nabíjení proběhne jen pokud:

```text
aktuální cena <= nAcceptable_Price_GRID
```

### Výkon

Používá `MAX Grid Point`.

Skutečný nabíjecí výkon může být nižší kvůli:

- nastavení měničů;
- BMS;
- nabíjecím limitům;
- aktuálnímu SOC;
- teplotě baterie;
- jiným limitům Victron systému.

### Fail-safe

`findCheapestContinuousBlock` při neplatném poli nebo nenalezeném bloku vrací náhradní první hodiny. Nelze tedy tvrdit, že neplatná SPOT data automatické nabíjení vždy zablokují. Konečné spuštění závisí i na dalších podmínkách a aktuální ceně; při nedostupných nebo neúplných cenách funkci vypněte.

---

# 4. Delay Charging

Konfigurační klíče:

```javascript
switchDelayCharging
tStartBattery
tStopBattery
```

### Účel

Odložit běžné nabíjení baterie a vytvořit v ní místo pro pozdější očekávanou výrobu.

V definovaném časovém okně se baterie nenabíjí běžným přebytkem. Podle aktuální logiky zůstává zachována vyvažovací rezerva a přebytek může být exportován.

### Vazby

Delay Charging spolupracuje s:

```text
Prediction Threshold
SOC Delta Before Export
povolením přetoků
Balancing Reserve
```

---

## SOC Delta Before Export

Konfigurační klíče:

```javascript
nSOCDeltaBeforeExport_Switch
nSOCDeltaBeforeExport
```

Pokud je tato podmínka zapnuta, export při Delay Charging se povolí až po dosažení definovaného SOC.

Příklad:

```text
SOC Delta = 35 %

SOC 30 % → přebytek nejprve do baterie
SOC 36 % → může se aktivovat export podle další logiky
```

Smyslem je zachovat minimální energetickou rezervu před prodejem přebytku.

---

# 5. Ranní a večerní prodej baterie

## Peak Sale

Konfigurační klíč:

```javascript
nSetPeak
```

Určuje maximální délku cenové špičky používané pro Morning/Evening Peak.

Současný widget uvádí:

```text
maximum 4 hodiny
```

---

## Morning Peak Battery Sales

Konfigurační klíče:

```javascript
nMorningPeakBatterySales
nMorningSOC_sales
```

### Funkce

Prodává energii z baterie během vybraného ranního SPOT peaku.

Pro aktivaci musí být současně splněny další podmínky:

```text
čas ∈ ranní peak
SOC >= ranní rezerva
přetoky povoleny
aktuální cena >= prodejní limit
Prediction Threshold splněn, pokud je aktivní
```

### Cílové SOC

`nMorningSOC_sales` je minimální pevná rezerva.

Při aktivní Dynamic SOC Reserve může být tato hodnota dynamicky zvýšena.

---

## Dynamic SOC Reserve

Konfigurační klíč:

```javascript
nDynamicSOC_Reserve
```

Další parametry:

```javascript
nSunriseProductionOffset
nHouseHourlyConsumption
```

### Účel

Ranní prodej nesmí baterii vybít tak hluboko, že nebude mít dost energie do začátku skutečné FV výroby.

Algoritmus proto odhaduje potřebnou rezervu podle:

```text
čas do východu slunce
+
Sunrise Production Offset
+
odhad hodinové spotřeby domu
```

### `nSunriseProductionOffset`

Počet hodin po východu slunce, po které ještě nelze očekávat dostatečnou výrobu.

### `nHouseHourlyConsumption`

Odhad spotřeby domu za hodinu vyjádřený v logice současného flow jako spotřeba rezervy SOC.

---

## Prediction Threshold

Konfigurační klíče:

```javascript
nPredictionThreshold
sPredictionThresholdKW
```

### Účel

Zabraňuje vybraným energetickým operacím v situaci, kdy není očekávána dostatečná výroba.

Používá se zejména pro:

```text
Morning Peak Battery Sales
Delay Charging
```

Současný flow porovnává predikovanou výrobu s požadovanou hranicí/spotřebou.


Při platném zdroji se porovnává FV predikce v kWh s vyšší z predikované spotřeby a uživatelského prahu. Při neplatném zdroji nastaví referenční flow `bPredikce = true`: filtr se **neuplatní**, závislá operace není z tohoto důvodu blokována. Ostatní podmínky strategie zůstávají v platnosti.

---

## Evening Peak Battery Sales

Konfigurační klíče:

```javascript
nEveningPeakBatterySales
nEveningSOC_sales
```

Prodává baterii během večerního cenového peaku.

Podmínky:

```text
čas ∈ večerní peak
SOC >= nEveningSOC_sales
přetoky povoleny
SPOT cena >= prodejní limit
```

Na rozdíl od Morning Peak se Prediction Threshold pro večerní prodej v aktuální logice nekontroluje.

---

# 6. Energy nastavení

## MAX Grid Point

Konfigurační klíč:

```javascript
nMAX_Grid_Point
```

### Účel

Výkonový limit používaný pro některé operace baterie:

- Morning Peak;
- Evening Peak;
- GRID CHARGING;
- Spot-Grid Charging.

Není to totéž jako:

```text
Maximum Grid Feed-In / registr 2706
```

`MAX Grid Point` určuje požadovaný výkon řídicí strategie, zatímco 2706 představuje samostatný horní limit feed-in.

---

## Balancing Reserve

Konfigurační klíč:

```javascript
nBalancingReserve
```

Vyvažovací rezerva ve W.

Používá se například:

```text
Zbytek FV =
PV výkon
-
(AC Load + Balancing Reserve)
```

Smyslem je omezit kmitání regulační smyčky kolem nuly nebo jiného cílového bodu.

---

# 7. Feed-In řízení

## Maximum Grid Feed-In – registr 2706

Registr:

```text
2706
```

Určuje maximální povolený export systému.

LINEA před odesláním Grid Pointu kontroluje požadavek proti načtenému limitu 2706 a ořeže příliš vysoký export.

### Důležitý rozdíl

```text
MAX GRID POINT
```

a:

```text
MAXIMUM GRID FEED-IN / 2706
```

jsou dvě různé věci.

### Doporučená rezerva

Kvůli regulační dynamice může krátkodobě dojít k překmitu. Proto nenastavujte limit přesně na maximální hodnotu povolenou distributorem.

---

## Grid Feed-In Enable – registr 2707

Používá se pro povolení/zakázání DC feed-in.

```text
0 → DC feed-in zakázán
1 → DC feed-in povolen
```

Týká se DC zdrojů, typicky FV/MPPT.

---

## AC Feed-In Enable – registr 2708

Určen pro AC zdroje.

Současná základní LINEA logika jej standardně nevyužívá.

---

# 8. SPOT – řízení přetoků

## Automaticky podle SPOTu

Konfigurační klíč:

```javascript
isSpotAutoCtrlEnabled
```

Zapíná automatické rozhodování o přetocích podle aktuální ceny.

### Limitní cena

```javascript
spotTresholdPrice
```

Určuje cenový práh používaný při rozhodování.

Ve starší dokumentaci LINEA byl princip popsán jednoduše:

```text
aktuální SPOT > limit → prodej povolen
aktuální SPOT < limit → prodej omezen
```

Konkrétní aktuální chování je vždy nutné posuzovat podle současného SPOT flow, protože SPOT cena vstupuje také do Morning/Evening Peak a dalších strategií.

---

# 9. Energy – výpočtové parametry

## Sunrise Production Offset

```javascript
nSunriseProductionOffset
```

Posun v hodinách od astronomického východu slunce do doby, kdy lze očekávat skutečně použitelnou výrobu FVE.

Používá Dynamic SOC Reserve.

---

## House Hourly Consumption

```javascript
nHouseHourlyConsumption
```

Odhad běžné hodinové spotřeby domu používaný při výpočtu rezervy baterie do začátku FV výroby.

---

## Peak Sale

```javascript
nSetPeak
```

Maximální délka ranního/večerního cenového okna.

---

# 10. Jak UI ovlivňuje FVE

Dashboard neposílá své přepínače přímo do Modbus write nodu.

Tok je:

UI přes `ESS :: Widget_Handler` a `setConfigProperty()` mění centrální config. AC LOAD vypočte `nSet_Grid_Point`, který zpracuje `sendValueWithinLimits`. Podle Control Mode potom následuje zápis **buď do 2700, nebo do 2716/2717**.

To je důležité pro diagnostiku.

Pokud přepínač v UI změní stav, ale FVE nereaguje, je nutné postupně ověřit:

1. změnil se config?
2. načetla jej ESS logika?
3. změnil se `nSet_Grid_Point`?
4. nebyl požadavek omezen 2706?
5. který Control Mode je aktivní?
6. proběhl Modbus zápis?
7. odpovídá skutečně přečtený registr?

---

# 11. Rychlá tabulka

| UI / parametr | Config klíč | Typ | Hlavní vliv |
|---|---|---|---|
| Set Point | `nGridConsumptionEnable` | bool | aktivuje pevný Grid Point |
| AcPowerSetPoint | `nSetGridValue` | W | hodnota Set Pointu |
| Control Mode | `nControl_Mode_ESS_AC_Grid` | bool | volí 2700 nebo 2716/2717 |
| Energy Threshold Injector | `nEnergyThresholdInjector` | bool | setpoint pouze při FV přebytku |
| Spot-Grid Charging | `switchSpotGridCharging` | bool | automatické levné GRID nabíjení |
| Non Battery Priority | `nNonBatteryPriorityMode` | bool | omezuje vybíjení baterie při velké zátěži |
| Delay Charging | `switchDelayCharging` | bool | odkládá nabíjení |
| Delay start | `tStartBattery` | čas | začátek okna |
| Delay stop | `tStopBattery` | čas | konec okna |
| SOC Delta | `nSOCDeltaBeforeExport_Switch` | bool | podmínka SOC před exportem |
| SOC Delta value | `nSOCDeltaBeforeExport` | % | minimální SOC |
| Dynamic SOC Reserve | `nDynamicSOC_Reserve` | bool | dynamická ranní rezerva |
| Morning Sales | `nMorningPeakBatterySales` | bool | ranní prodej baterie |
| Morning SOC | `nMorningSOC_sales` | % | minimální ranní SOC |
| Prediction Threshold | `nPredictionThreshold` | bool | zapíná filtr predikce |
| Prediction limit | `sPredictionThresholdKW` | kWh | práh predikce |
| Evening Sales | `nEveningPeakBatterySales` | bool | večerní prodej |
| Evening SOC | `nEveningSOC_sales` | % | minimální večerní SOC |
| Grid Charging | `sGridChargingSwitch` | bool | ruční GRID nabíjení |
| Grid Charging SOC | `nGridChargingSOC` | % | cílové SOC |
| Peak Sale | `nSetPeak` | h | délka peak okna |
| MAX Grid Point | `nMAX_Grid_Point` | W | max. výkon vybraných strategií |
| Balancing Reserve | `nBalancingReserve` | W | regulační rezerva |
| Spot charging hours | `nCharging_DurationGRID` | h | délka levného bloku |
| Max buy price | `nAcceptable_Price_GRID` | cena | limit GRID nákupu |
| Sunrise offset | `nSunriseProductionOffset` | h | posun startu FV výroby |
| House consumption | `nHouseHourlyConsumption` | podle logiky flow | noční SOC rezerva |
| SPOT auto | `isSpotAutoCtrlEnabled` | bool | automatické SPOT přetoky |
| SPOT threshold | `spotTresholdPrice` | cena | cenový práh |

---

[← Dokumentace LINEA](README.md)
