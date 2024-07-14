# Linea
Control of photovoltaic power plant using Node-RED”

- Verze 14072024:
  - Přidán flow na přímé ovládání pomocí shelly plychinu ventilátoru. Pro vaše účely je třeba upravit a nebo úplně vymazat. Není to úplně dodělané, hlavně GUI je nedodělané a nešikovné. 
  - Už funguje funkce prodej ranní špičky, zatím ranní špička je definována na údek 2 hodin.
  - Přidány testovací výpisy, stačí v patřičném node zapnout DEBUG na true a případně si upravit požadovaný výpis proměnné.
  - Přidány globální funkce - takže se opakující funkce napíšou jen jednou a v dalších node function se jen načtou.
  - Opraveno napříč flow práce s časem. Od této verze se zadá v globálních proměnných na flow GUI County_Code a TZidentifier. 

```javascript
// Definice timezone and Country code
// LIST CODES: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
const sCounty_Code ="cs-CZ";
const sTZidentifier = "Europe/Prague";
```
   

![image](https://github.com/user-attachments/assets/9ab14a0e-0656-4e6b-a26c-97137024f918)


Budu rád za jakoukoliv zpětnou odezvu a případnou opravu chyb a vylepšení.

Své flow provozuji na NASu, proto používám node-red-contrib-modbus. Sice to lze používat i na Cerbu, ale tím je jednak zpomalováno a zadruhé, 
flow vidí kdokoliv, kdo je admin FVE, a to se mi nelíbí, aby tam nahlížela instalační firma nebo někdo z www.deltagreen.cz. Node victron-vrm-api 
zatím není třeba instalovat, ještě ho nepoužívám. To platí i pro nadstavbu: node-red-contrib-victron-modbus a její příslušný attributes.csv soubor, 
který se musí nainstalovat dle pokynů. Soubor CCGX-Modbus-TCP-register-list-2.90.xlsx je seznam registrů a jejich funkcí. 

Do flow se naimportuje, po nainstalování potřebných node knihoven soubor: battery control.json, ten je převážně zodpovědný za manipulaci s registrem GRID POINT, 
tak i Spot excess.json, ten je zodpovědný za řízení SPOTu. Nově vzniklé flow GUI.json obsahuje záležitosti grafického zobrazení, 
zatím je to hybrid, postupně bude předěláváno. Zatím je vše tak nějak ve vývoji, něco už tam funguje, ale je to třeba dořešit. 
Potom je třeba předělat UI, zatím je to poházené… 

Funkce: Prodej baterii - ranní špička a Prodej baterii - večerní špička jsou v GUI připravené a také nalezení těchto úseků, mají je na starosti funkce:

```
  // Hleda ranni a odpoleni peaky
  // prvni promenna je pole aktualniho spot a druha promenna rika jak dlouhy peak muse bejt
  const morningPeak1 = findMorningPeakHours(todayHourlyPrices, 2);
  const afternoonPeak1 = findAfternoonPeakHours(todayHourlyPrices, 2);
```
Ale zatím nejsou zapracované do celkové logiky která je:

```
nSet_Grid_Point = fSet_Grid(nGridConsumptionEnable, nSetGridValue, isSpotAutoCtrlEnabled, spotTresholdPrice, currentPrice, nPretoky);    
    let nZbytekFV = pvPower - (totalACLoad + nRezerva);
    // funkce na posun nabijeni baterii
    if (tStartBatteryTime && tStopBatteryTime) {
        if (debug) {
            node.warn("tStartBattery: " + tStartBatteryTime);
            node.warn("tStopBattery: " + tStopBatteryTime);
            node.warn("oDate_CZ: " + oDate_CZ);
        }
        if (switchDelayCharging == true) { // true znamena ze je aktivovana funkce posunuti nabijeni baterie
            if (nPretoky == true) { // povolene pretoky
                if (oDate_CZ >= tStartBatteryTime && oDate_CZ <= tStopBatteryTime) { // Aktuální čas je v rozsahu
                    if (nZbytekFV > 0) { // Prebytek FV je kladna hodnota
                        nSet_Grid_Point = - nZbytekFV; // prevest na zapornou hodnotu
                    } else { // mala vyroba z FV panelu
                        nSet_Grid_Point = 0;
                    }
                } else { // Aktuální čas je mimo rozsah
                    // nSet_Grid_Point = fSet_Grid (nGridConsumptionEnable, nSetGridValue, isSpotAutoCtrlEnabled, spotTresholdPrice, currentPrice, nPretoky);    
                }
            } else { // Čas začátku nebo konce není nastaven
                nSet_Grid_Point = 0;
            }
        }
    }
```

Zde potom zapracuji… Časem

Vysvětlení významu přepínačů:

- Automaticky podle SPOTu: Tlačítko po aktivaci sleduje aktuální cenu spotu a v kolonce Limitní cena je trigger, dokud se mají posílat přetoky.
- Přetoky: Zap / Vyp: Tlačítko zapne nebo vypne přetoky, není nikterak blokováno žádnou logikou, taková pojistka.
			Zap/Vyp AcPowerSetPoint: Zapne tuto funkci. Pak následujícím ovladačem nastavujete požadovaný odběr. Já mám -100W, 
			díky tomuto nastavení mám cca celkový odběr do z GRIDu do 1kW za den. Když nastavím 0, tak to dělá daleko více. 
			Tato funkce se automaticky vypne, když je spot zakázán… Časem předělám tak, aby fungovalo toto nastavení i do zakázaného spotu, ale do doby, než bude spot záporný.
- Posunutí nabíjení Baterie: Nastavíte od - do má tato funkce pracovat. Nastavuji od rána a do sepnutí bojleru, to je 11 hodin. 
			Tato funkce pracuje tak, že si zjistí aktuální dodávku z panelů, a aktuální odběr a připočítá k tomu nějakou rezervu, 
			kladný rozdíl nastaví do stejného registru jako funkce Zap/Vyp AcPowerSetPoint.
- Prodej baterii - ranní špička - momentálně neimplementováno, viz předchozí text.
 -Prodej baterii - večerní špička - momentálně neimplementováno, viz předchozí text.


Pokud má někro rád kulaté dlaždice a chce toto: ![image](https://github.com/hacesoft/Linea/assets/53556265/a39f2a8a-10ad-4e8f-b15c-bd0ed1efdb8b)
Tak ve flow GUI je template: ![image](https://github.com/hacesoft/Linea/assets/53556265/e2cf93ab-71df-44d3-ac8a-e895fdfe1815)

a je třeba v CCS profilu vypnout okraje a zapnout zakulacení rohů:).

```
    .nr-dashboard-theme ui-card-panel {
        background-color: var(--background);
        border: 0px solid var(--okraj-karty);      //nastaveni okraje karet
    }

    /* Kulatění rohů */
    .Kulati_Nahore {
        border-top-left-radius: var(--radius-s);
        border-top-right-radius: var(--radius-s);
    }

    .Kulati_Dole {
        border-bottom-left-radius: var(--radius-s);
        border-bottom-right-radius: var(--radius-s);
    }
```

a takto vypadá původní nastavení:

```
    .nr-dashboard-theme ui-card-panel {
        background-color: var(--background);
        border: 2px solid var(--okraj-karty);      //nastaveni okraje karet
    }

    /* Kulatění rohů */
    .Kulati_Nahore {
       // border-top-left-radius: var(--radius-s);
       // border-top-right-radius: var(--radius-s);
    }

    .Kulati_Dole {
       // border-bottom-left-radius: var(--radius-s);
       // border-bottom-right-radius: var(--radius-s);
    }
```


Nainstalované NODE:

- @victronenergy/node-red-contrib-victron - 1.5.17
- node-red - 4.0.0
- node-red-contrib-config - 1.2.1
- node-red-contrib-cron-plus - 2.1.0
- node-red-contrib-modbus - 5.31.0
- node-red-contrib-victron-modbus - 0.1.5
- node-red-dashboard - 3.6.5
- victron-vrm-api - 0.2.5


