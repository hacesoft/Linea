[🇨🇿 **Česky**](10_OVLADANI_ESS.md) | [🇬🇧 English](../en/10_ESS_CONTROLS.md)

---

# Ovládací prvky ESS Dashboardu

Tento dokument popisuje uživatelské přepínače a hodnoty v kartě ESS a jejich vliv na řídicí algoritmus.

## Volba řídicího registru

### Control Mode: ESS / AC Grid

Konfigurace:

```javascript
nControl_Mode_ESS_AC_Grid
```

- **ON** → používá INT32 registry **2716/2717**;
- **OFF** → používá starší INT16 registr **2700**.

ESS widget zobrazuje živě oba setpointy a zvýrazňuje právě aktivní cestu. fileciteturn4file7L186-L188

Podrobnosti o periodickém zápisu jsou v `MODBUS.md`.

## Set Point

Konfigurace:

```javascript
nGridConsumptionEnable
nSetGridValue
```

Zapíná nucený Grid Point. `nSetGridValue` je požadovaný setpoint ve W.

Ve widgetu je hodnota editovatelná pouze při zapnuté funkci. Záporná hodnota znamená dodávku do sítě, kladná odběr ze sítě.

## Energy Threshold Injector

Konfigurace:

```javascript
nEnergyThresholdInjector
```

Pracuje s přebytkem FV:

```text
Zbytek FV = výroba - (spotřeba domu + Balancing Reserve)
```

Pokud jsou splněny podmínky funkce, může nastavit Grid Point na definovaný AC setpoint. Současný widget tuto funkci výslovně označuje jako proprietární/specifickou pro danou instalaci.

## Spot-Grid Charging

Konfigurace:

```javascript
switchSpotGridCharging
```

Automatické nabíjení baterie ze sítě podle SPOT ceny.

Algoritmus hledá nejlevnější souvislý blok hodin. Délku bloku určuje nastavení `SPOT-GRID Charging` v energetické části. Nabíjení probíhá výkonem `MAX Grid Point`, pokud je aktuální cena pod nastaveným cenovým limitem.

## Non Battery Priority Mode

Konfigurace:

```javascript
nNonBatteryPriorityMode
```

Režim prioritizuje jinou spotřebu před vybíjením baterie. V současném projektu byl vytvořen zejména pro nabíjení elektromobilu.

Pokud FV nepokryje spotřebu domu a výkon baterie, chybějící výkon je požadován ze sítě místo dalšího vybíjení baterie. Tato vazba je přímo součástí centrální ESS logiky. fileciteturn4file4L53-L55

## Delay Charging

Konfigurace:

```javascript
switchDelayCharging
tStartBattery
tStopBattery
```

Odložené nabíjení baterie.

V definovaném časovém okně se omezuje běžné nabíjení baterie a přebytek FV může být místo toho exportován do sítě. Smyslem je vytvořit v baterii prostor pro očekávanou pozdější výrobu.

Funkce spolupracuje s Prediction Threshold a volitelně se SOC Delta Before Export.

## SOC Delta Before Export

Konfigurace:

```javascript
nSOCDeltaBeforeExport_Switch
nSOCDeltaBeforeExport
```

Je-li aktivní, Delay Charging nepovolí export přebytku, dokud SOC baterie nedosáhne nastavené minimální hodnoty.

## Dynamic SOC Reserve

Konfigurace:

```javascript
nDynamicSOC_Reserve
```

Dynamicky upravuje minimální rezervu SOC pro ranní prodej.

Výpočet používá čas do začátku očekávané výroby, `Sunrise Production Offset` a odhad hodinové spotřeby domu. Výsledkem je vyšší minimální SOC v situaci, kdy musí baterie pokrýt delší období do začátku výroby.

## Morning Peak Battery Sales

Konfigurace:

```javascript
nMorningPeakBatterySales
nMorningSOC_sales
```

Povoluje prodej energie z baterie během vypočtené ranní cenové špičky.

Prodej vyžaduje mimo jiné:

- čas uvnitř ranního peak okna;
- dostatečné SOC;
- povolené přetoky;
- cenu nad limitem;
- dostatečnou predikci výroby.

Při aktivní Dynamic SOC Reserve se místo pevného ranního SOC použije dynamicky vypočtená rezerva. fileciteturn4file1L20-L22

## Prediction Threshold

Konfigurace:

```javascript
nPredictionThreshold
sPredictionThresholdKW
```

Zapíná kontrolu predikce výroby.

Ranní prodej a Delay Charging se aktivují pouze tehdy, pokud je očekávaná výroba dostatečná. Flow porovnává predikci s potřebami objektu a nastaveným limitem.

Při vypnutém přepínači se tato podmínka nepoužije.

## Evening Peak Battery Sales

Konfigurace:

```javascript
nEveningPeakBatterySales
nEveningSOC_sales
```

Povoluje prodej baterie ve večerní SPOT špičce.

Podmínky zahrnují:

- aktuální čas ve večerním peak okně;
- SOC nad `nEveningSOC_sales`;
- povolené přetoky;
- SPOT cenu nad nastaveným limitem.

Na rozdíl od ranního prodeje se zde predikce výroby nekontroluje. fileciteturn4file2L31-L33

## Grid Charging

Konfigurace:

```javascript
sGridChargingSwitch
nGridChargingSOC
```

Okamžité nabíjení baterie ze sítě výkonem `MAX Grid Point` do zadaného cílového SOC.

Po dosažení cílového SOC se funkce automaticky vypne. Nejde o regulátor, který by baterii trvale udržoval na zadaném SOC.

## MAX Grid Point

Konfigurace:

```javascript
nMAX_Grid_Point
```

Maximální výkon používaný pro vybrané operace nabíjení/prodeje. Logika si podle směru operace upravuje znaménko.

## Balancing Reserve

Konfigurace:

```javascript
nBalancingReserve
```

Rezerva ve W odečítaná při výpočtu dostupného přebytku FV. Pomáhá zabránit kmitání regulace kolem nulového toku sítě.

## Grid Feed limit – registr 2706

Limit dodávky do sítě. Řídicí větev jím ořezává požadovaný export před zápisem výsledného Grid Pointu.

## Kde se změny ukládají

ESS widget nepíše přímo do Modbusu. Hodnoty posílá do `ESS :: Widget_Handler`, který povoluje pouze definované konfigurační klíče a zapisuje je do centrální konfigurace. Seznam povolených boolean a numeric klíčů je explicitně definován ve flow. fileciteturn4file0L9-L11

Řídicí logika následně tuto konfiguraci načte a podle ní vypočítá požadovaný Grid Point.

```text
UI přepínač
    ↓
Widget_Handler
    ↓
config
    ↓
ESS / AC LOAD logika
    ↓
limitace
    ↓
volba 2700 nebo 2716/2717
    ↓
Modbus
```

---

[← Dokumentace LINEA](README.md)
