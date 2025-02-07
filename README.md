# Linea
Control of photovoltaic power plant using Node-RED” for Node-RED v4.0.2

![image](https://github.com/user-attachments/assets/21efd405-18f0-4cbd-80e1-cc43f975d301)

<h4>Popis FLOW LINEA pro Node-RED</h4>

> FLOW je rozšíření pro Node-RED, které poskytuje řadu funkcí pro řízení a monitorování fotovoltaických elektráren (FVE) a bateriových úložišť. Tento nástroj je neustále vyvíjen a aktualizován, aby zahrnoval nejnovější funkce a opravy chyb.

<h4>Hlavní funkce FLOW:</h4>
<h5>Instalace a závislosti:</h5>

>> Doporučuje se instalovat nejnovější verzi FLOW.
Instalace probíhá importem FLOW do Node-RED po nainstalování závislých knihoven.
 
<h4>Verze a aktualizace:</h4>

 - 07022025: Drobné úpravy kódu, přidány ikony a výpočty úložení elektrické energie do a z baterie.
 - 02112024: Přidán indikátor doby provozu na kartě FVE::Real Data.
 - 14102014: Upravena logika prodeje baterie v ranní a odpolední špičce.
 - 28082024: Přidán indikátor připojení zařízení na kartě config.
 - 15092024: Přidána funkce GRID CHARGING pro nabíjení baterie z GRIDu.
 - 25082024: Opraveny drobnosti v tooltipu a změna cesty ukládání konfiguračního souboru.
 - 14082024: Opraveny drobné chyby v CSS profilu a FLOW, možnost konfigurovat konstantu nBalancingReserve.
 - 13082024: Dokončena funkce pro večerní prodej baterie a možnost nastavit maximální vybíjecí proud.
 - 03082024: Opravena kritická chyba selhání SPOTU.
 - 31072024: Odstranění závislosti na knihovnách node-red-contrib-config a node-red-contrib-victron-modbus.
 - 29072024: Přidána karta Config pro nastavení TCP a FILE.
 - 20072024: Opraveny drobné chyby v CSS profilu a flow chlazení FVE.
 - 19072024: Opraveny funkce pro zápis změn do registru a synchronizace času.
 - 15072024: Opraveny funkce pro konverzi signed/unsigned hodnot.
 - 14072024: Přidán flow pro přímé ovládání ventilátoru pomocí pluginu Shelly a funkce prodeje ranní špičky.
   
<h4>Funkce a ovládací prvky:</h4>

- Automatické řízení podle SPOTu: Sleduje aktuální cenu spotu a řídí přetoky.
- Přetoky: Zapíná nebo vypíná přetoky.
- Posunutí nabíjení baterie: Nastavuje časové období pro nabíjení baterie.
- Prodej baterie: Funkce pro prodej baterie v ranní a večerní špičce (zatím neimplementováno).
- Konfigurace a uživatelské rozhraní:

- Možnost nastavit TCP a FILE konfiguraci.
- Ukládání a načítání defaultní konfigurace.
- Indikátory stavu připojení a chyb.

  
<h4>Diagnostika a opravy:</h4>

- Diagnostické funkce pro monitorování chyb a ztrát tokenů.
- Opravy chyb v logice a synchronizaci času.

<h4>Instalace FLOW:</h4>

- Nainstalujte potřebné knihovny: node-red, node-red-contrib-modbus, node-red-dashboard.
- Importujte soubor ALL_flow_xxx.json do Node-RED (kde xxx nahrazuje poslední verzei.
- Nastavte konfiguraci na kartě Config a spusťte FLOW.
- FLOW je komplexní nástroj pro řízení a monitorování FVE a bateriových úložišť, který neustále přináší nové funkce a vylepšení. Pro další informace a aktualizace sledujte vývojové záznamy a dokumentaci.



>> Doporučuji vždy instalovat nejnovější verzi FLOW. Instalace se provádí importem FLOW do node-red, ale nejprve nezapomeňte nainstalovat závislé knihovny (ty jsou definované na konci této stránky, včetně verzí, na které je FLOW stavěno a testováno).

<h4>Popis jednotlivých verzí:</h4>

- Verze 07022025:
   - Drobné úpravy kódu.
   - Přidány ikony.
   - Přidány výpočty úložení elektrické energie do a z baterie.

     
- Verze 02112024:
   - Byl přidán indikátor na kartě FVE::Real Data - doba provozu, který indikuje, jak dlouho dané FLOW běží. Při jakékoli změně se počitadlo nuluje.
   - Flow umí vyhodnotit, kde je spuštěno, a podle toho samo nastaví cestu k úložišti.


- Verze 14102014:
   - Upravena logika u funkcí prodej baterie v ranní i odpolední špičce.
 
     
- Verze 28082024:
  - Na kartě config tlačítko CONNECT je signalizováno indikátorem, zda je zařízení připojeno na danou IP adresu. Chyba v samotné knihovně node-red-contrib-modbus stále existuje. Celý fígl spočívá v tom, že flow se pokouší přečíst SN instalace. Pokud neuspěje nebo cca 4 sekundy nepřijde žádná informace o SN čísle, indikátor změní barvu na šedou. Po připojení indikátor změní barvu na zelenou. Komponenta node connection nevysílá žádné zprávy, tudíž jsem zavedl do flow časové razítko a pokud je platné, není dostupné zařízení na dané IP adrese.
  - Přidána defaultní konfigurace. Po spuštění flow na kartě config vidíte tlačítka DEFAULT LOAD a DEFAULT SAVE. Nastavte si vše, jak potřebujete, a proveďte uložení. Toto je základní nastavení, abyste nemuseli měnit nic v globální struktuře a při updatu na novou verzi opět vše měnit. Vše děláte jen v UI. Po tomto uložení tlačítka změní názvy na CONFIG LOAD a CONFIG SAVE, zde si nastavte, co potřebujete. Celý smysl toho je, že při restartu node-red se načte defaultní konfigurace. Takže tam doporučuji nastavit IP adresy a login k VRM, včetně základního nastavení přepínačů funkcí.

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

- node-red - 4.0.3
- node-red-contrib-modbus - 5.42.0
- node-red-dashboard - 3.6.5


