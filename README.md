# Linea
Control of photovoltaic power plant using Node-RED”


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

Zde potom zapracuji...Casem

Vysvětlení významu přepínačů:

- Automacitky podle SPOPTUu : tlačítko po aktivaci sleduje aktualní cenu spotu a v kolonce Limitni cena je trigger dokdy se mají posílat přetoky.
- Přetoky: Zap / Vyp : tlačítko zapne nebo vypne přetoky, není nikterak blokováno žádnou logikou, taková pojistka
- Zap/Vyp AcPowerSetPoint - zapnet tuto funkci. Pak nasledujícím ovladačem nastavujete požadovaný odber, Ja mám -100W, díky tomuto nastavení mám cca celkový odber do z GRIDu do 1kW za den, 
			    když nastavím 0 tak to déla daleko více. Tato funkce se automaticky vypne když je spot zakazaný... Časem předelám tak aby fungovalo tot nastavení 
			    i do zakázaného spotu ale do doby než bude spot zaporny.
- Posunutí nabíjení Baterie: nastavíte od - do ma tato funkce pracovat. Nastavuji od rána a do sepnutí bojleru, to je 11 hodin. Tato funkce pracuje tak, že si zjistí aktuální dodávku
			     z panelů, a aktuální dodběr a připočíta k tomu nejakou rezervy a kladný rozdíl nastaví do stejného registru jako funkce Zap/Vip AcPowerSetPoint.
- Prodej baterii - ranní špička - momentálne neimplementováno, viz predchozí text.
- Prodej baterii - večerní špička - momentálne neimplementováno, viz predchozí text.
				

Nainstalovane NODE:

- @victronenergy/node-red-contrib-victron - 1.5.17
- node-red - 4.0.0
- node-red-contrib-config - 1.2.1
- node-red-contrib-cron-plus - 2.1.0
- node-red-contrib-modbus - 5.31.0
- node-red-contrib-victron-modbus - 0.1.5
- node-red-dashboard - 3.6.5
- victron-vrm-api - 0.2.5


