[🇨🇿 **Česky**](09_ESS_RIZENI.md) | [🇬🇧 English](../en/09_ESS_CONTROL.md)

---

# ESS řízení

## Princip

ESS algoritmus rozhoduje, jak má být dostupná energie rozdělena mezi spotřebu, baterii a distribuční síť.

Základní priorita fyzického systému je spotřeba objektu. LINEA řídí zejména další využití přebytku a požadovaný tok vůči síti.

## Hlavní vstupy

- FV výkon;
- AC Load;
- Grid Point;
- SOC;
- výkon baterie;
- stav feed-in;
- SPOT cena;
- predikce;
- časová okna;
- konfigurační přepínače.

## Set Point

Při aktivním `nGridConsumptionEnable` se používá `nSetGridValue` jako požadovaný Grid Point.

Znaménková konvence LINEA:

```text
kladná hodnota  → odběr ze sítě
záporná hodnota → dodávka do sítě
0 W             → nulový tok
```

## Non Battery Priority

`nNonBatteryPriorityMode` omezuje vybíjení baterie při vysoké spotřebě. Pokud FV výkon nestačí pokrýt požadovanou spotřebu, může být chybějící výkon požadován ze sítě místo dalšího vybíjení baterie.

## Delay Charging

`switchDelayCharging` používá časové okno `tStartBattery` až `tStopBattery`. V tomto období lze odložit běžné nabíjení baterie a ponechat v ní prostor pro pozdější FV výrobu.

Chování může být dále omezeno:

- Prediction Threshold;
- SOC Delta Before Export;
- stavem přetoků.

## Morning Peak

`nMorningPeakBatterySales` umožňuje prodej energie z baterie v ranním cenovém okně.

Podmínky zahrnují:

- čas v ranním peak okně;
- dostatečný SOC;
- povolené přetoky;
- cenovou podmínku;
- Prediction Threshold, pokud je aktivní.

Minimální SOC určuje `nMorningSOC_sales`, případně Dynamic SOC Reserve.

## Dynamic SOC Reserve

Dynamická rezerva určuje minimální ranní SOC podle doby, kterou musí baterie pokrýt do očekávaného začátku použitelné FV výroby.

Do výpočtu vstupuje zejména:

- čas východu slunce;
- `nSunriseProductionOffset`;
- `nHouseHourlyConsumption`.

Výsledná rezerva omezuje Morning Peak tak, aby v baterii zůstala energie pro spotřebu do nástupu výroby.

## Evening Peak

`nEveningPeakBatterySales` umožňuje prodej energie z baterie ve večerním cenovém okně. Minimální SOC určuje `nEveningSOC_sales`.

## GRID Charging

`sGridChargingSwitch` spustí nabíjení ze sítě do `nGridChargingSOC`. Po dosažení cílového SOC se funkce vypne.

Výkon je odvozen od `nMAX_Grid_Point` a skutečný výkon může být dále omezen FVE, BMS nebo nabíjecími limity.

## Spot-Grid Charging

`switchSpotGridCharging` hledá nejlevnější souvislý blok o délce `nCharging_DurationGRID`.

Nabíjení je povoleno pouze při splnění cenové podmínky `nAcceptable_Price_GRID` a ostatních provozních podmínek.

## Výstup

Všechny aktivní strategie jsou vyhodnoceny v ESS logice a výsledkem je jeden požadovaný Grid Point. Ten následně prochází bezpečnostními limity a Modbus výstupem.

---

[← Dokumentace LINEA](README.md)
