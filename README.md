# Linea
Control of photovoltaic power plant using Node-RED” for Node-RED v4.0.2

![image](https://github.com/user-attachments/assets/0028437e-37e0-4939-8d66-a8fb5432cde2)


Doporučuji vždy instalovat nejnovější verzi FLOW. Instalace se provádí importem FLOW do node-red, ale nejprve nezapomeňte nainstalovat závislé knihovny (ty jsou definované na konci této stránky, včetně verzí, na které je FLOW stavěno a testováno).

- Verze 15092024:
  - Přidána funkce GRID CHARGING nabíjení baterie z GRIDu. Nabíjecí proud se nastavuje na kartě CONFIG, položka MAX GRID POINT, a je jedno, zda uvedené číslo je kladné nebo záporné. Patřičné funkce si to upraví dle svého požadavku. Nastavíte, do jakého SOC má nabíjet, a po dosažení se automaticky vypne a dál nepokračuje. Není to funkce na udržení baterie na daném SOC, ale je to spíše míněno pro nouzové nabití baterie pro očekávaný nadcházející výpadek elektřiny. Do konfigurace se ukládá jen nastavená hodnota SOC.
  - Přidány diagnostické funkce ohledně vypadávání načítání dat z VRM. Možná bude hlásit chyby uložení souboru.
  - Opravena chyba ztráty tokenu pro VRM. Oprava je provedena takto: V případě ztráty tokenu je načtena konfigurace a vezme se jen hodnota tokenu, zbytek nastavení je ignorován.
        
- Verze 25082024:
  - Opraveny drobnosti v tooltipu.
  - Změna cesty ukládání konfiguračního souboru do root/mode_modules
  - Token se ukládá do konfiguračního souboru. Když Node-Red běží v kontejneru tak se často restartuje, a pak se ztratí připojení na VRM
  - V Inmout boxu pro zadání Tokenu je primitivní test validace tokenu.
   
- Verze 14082024:
  - Opraveny drobné chyby v CSS profilu.
  - Opraveny drobné chyby ve FLOW.
  - Možnost konfigurovat konstantu: nBalancingReserve. Hodnota ve watech slouží k přičtení hodnoty po rozdílu mezi celkovou zátěží a aktuální výrobou z FV panelu. Rozdíl se pošle na GRID, aby se zamezilo kolísání nabíjení/vybíjení baterie. Tato konstanta má u mě hodnotu 230W. U jiného systému možná bude třeba upravit.
  - Přidán k LABELu “Údaje o instalaci:” aktuální čas a datum.
 
- Verze 13082024:
  - Konečně je dokončena funkce pro večerní prodej baterie.
  - Na kartě CONFIG je nyní možné nastavit i maximální vybíjecí proud ve W, pro ranní a večerní špičku dohromady.
  - Drobné opravy, hlavně interpretace hodnot FALSE a TRUE, pomocí dvou negací za sebou (příklad: !!fGetConfigProperty()).
  - Na kartě config nastavíte přístup k VRM. Zde zadáte své přihlašovací údaje (ukládá se pouze email, heslo nikoli). Také zadáte název vaší instalace, ke kterému se přidá nějaký náhodný řetězec. Dále je vyžadováno číslo vaší instalace, které najdete v URL vaší VRM instalace. Po připojení se vygeneruje token, který se nikam neukládá, ale existuje v globální proměnné tak dlouho, dokud nezrestartujete Cergo nebo kontejner, kde běží Node-RED. Pak je třeba provést novou žádost o token. Vygenerovaný token pak uvidíte ve vaší instalaci ve VRM v nastavení: “Předvolby/Integrace/Přístupové Tokeny”. Na kartě FVE v sloupci Real Data máte informace o vaší instalaci.
  - Karta RealTime Power je neustále ve vývoji, sice něco ukazuje, ale zatím se na to nedá spolehnout.
 
- Verze 03082024:
   - Opravena kritická chyba selhání SPOTU, po odstranění knihovny node-red-contrib-config, flow vyžadovalo větší upravu :).

- Verze 31072024:
  - Odstranění zavislosti na knihovně: node-red-contrib-config.
  - Odstranění závislosti na knihovně: node-red-contrib-victron-modbus. Tuto knihovnu jsem nikdy nepoužil, a nakonec jsem ji úplně zavrhl.
  - FIX: Některé ovládací prvky při nahrání konfigurace řádně nereagovaly na aktuální nastavení.
  - Upozornění: Přepínač “Přetoky: Zap / Vyp” není a nebude ukládán do globální struktury, a tudíž nebude uložen s konfigurací, aby se zabránilo nechtěnému zapnutí přetoků. Toto je řízeno přepínačem automaticky spot a limitní cenou (trigger).
 
    
- Verze 29072024:
  - Přidána karta Config - z karty zatím funguje nastavení TCP, kde zadáte IP adresu vaší FVE, port a ID, které většinou nebudete měnit. Port je defaultně 502 a ID je defaultně 100. Potom dáte connect. Jelikož je v knihovně 	modbus chyba, nefunguje signalizace stavu připojení a pořád uvidíte CONNECTING…
  - Také je funkční nastavení FILE, kde když máte první instalaci, tak pokud neexistuje na disku (v ROOT adresáři node-red) konfigurační soubor, tak si ho vytvoří. Parametry nastavení si můžete upravit dle libosti a uložit 		tlačítkem SAVE CONFIG. Tlačítko DELETE CONFIG je dobré, když instalujete novou verzi FLOW, aby se načetly korektně defaultní hodnoty.
  - Napříč FLOW byly změněny proměnné ze samostatných definic na globální strukturu, která pak jde uložit na disk.

![image](https://github.com/user-attachments/assets/ef521fc2-1faa-478d-a702-8a99e7f5978a)


- Verze 20072024:
  - FIX CCS profil - Drobný detail, při zavírání karet se sloupce pohybovaly.
  - FIX flow chlazení FVE - Přidán node delay 3s pro posun paketu.
 
    
- Verze 19072024:
  - FIX funkce CopyOnChane_2707 - Teď se do registru 2707 zapisují a posílají jen změny, nikoliv stejná hodnota.
  - FIX function GLOBAL FUNCTION - Funkce sExtractTime a mExtractTime mají stejný časový základ pomocí funkce fSetFixedDate a nemůže se stát, že budou mít rozdílné hodnoty.
  - Nejvíce je přepracován flow chlazení FVE:
	 - Kde jednak jsou indikátory, zda se ventilátory točí, je to odezva od PLUGINu SHELLY, která potvrdí přijetí příkazu. Není implementováno monitorování odběru ele.i.
	 - Zapnutí ventilátoru je okamžité, jakmile dosáhne hodnoty triggeru, ale vypínám je až po přijetí 20x za sebou příkazu STOP. Tím se vyhneme nějakému mraku.
    		

- Verze 15072024:
  - FIX funkce convert signet to unsigned na flow battery control.
  - FIX funkce convert unsigned to signet na flow battery control.
  - Částečně přepracován flow Spot Excess Control. Část node předělána do function node: Logical write register 2707.
 
    
- Verze 14072024: 
  - Přidán flow pro přímé ovládání ventilátoru pomocí pluginu Shelly. Pro vaše účely je třeba upravit nebo úplně vymazat. Není to úplně dokončené, hlavně GUI je nedokončené a nepraktické.
  - Už funguje funkce prodeje ranní špičky, zatím je ranní špička definována na úsek 2 hodin.
  - Přidány testovací výpisy, stačí v příslušném node zapnout DEBUG na true a případně si upravit požadovaný výpis proměnné.
  - Přidány globální funkce - takže se opakující funkce napíšou jen jednou a v dalších node function se jen načtou.
  - Opravena práce s časem napříč flow. Od této verze se zadává v globálních proměnných na flow GUI County_Code a TZidentifier.

```javascript
// Definice timezone and Country code
// LIST CODES: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
const sCounty_Code ="cs-CZ";
const sTZidentifier = "Europe/Prague";
```


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
- INSTALACE: Klikněte kdekoliv na prázdné místo FLOW levým tlačítkem myši a vyberte: ![image](https://github.com/user-attachments/assets/644a08ea-4e1f-48ce-9c42-d9b74f0a1a06)
- Na Import nodes vyberte kartu CLIPBOARD, klikněte na tlačítko select a file import, vyberte váš FLOW soubor a vše potvrďte.


Nainstalované NODE:

- node-red - 4.0.2
- node-red-contrib-cron-plus - 2.1.0
- node-red-contrib-modbus - 5.40.0
- node-red-dashboard - 3.6.5
- victron-vrm-api - 0.2.6


