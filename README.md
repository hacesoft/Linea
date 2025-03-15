# Linea
Control of photovoltaic power plant using Node-RED” for Node-RED v4.0.2

>> Budu rád za jakoukoliv zpětnou odezvu a případnou opravu chyb a vylepšení.

![image](https://github.com/user-attachments/assets/429b1479-c846-463c-9bec-24ca7c06de2f)



### Popis FLOW LINEA pro Node-RED

> FLOW je rozšíření pro Node-RED, které poskytuje komplexní sadu funkcí pro řízení a monitorování fotovoltaických elektráren (FVE VICTRON) a bateriových úložišť.

### Klíčové detaily:

- **Hostování a nastavení:** FLOW běží v rámci Node-RED, který je nutné nainstalovat na hostujícím zařízení, jako je NAS, notebook nebo PC, které je trvale zapnuto.

### FAQ:

- **Nemám povolení přetoky:**
  - V tomto případě je jakékoli řízení zbytečné, není co řídit.

- **Mám povolené přetoky:**
  - Pokud mám povolené přetoky od distributora a chci prodávat přetoky, je toto řešení FLOW ideální.
  - Nebo se můžete podívat po jiném řešení (např. Delte Green, řízení přímo ve VRM).

- **Přetoky už ovládá jiné řešení:**
  - Je možné použít jen jedno ovládání přetoků, jinak se budou různá ovládání přetahovat o řízení.

- **Mám omezené povolené přetoky:**
  - Nevadí, nastavte v Cerbu maximální dovolené povolené přetoky, minus nějaká rezerva, třeba 500 W.

- **Jak se FVE bude ovládat:**
  - První prioritou je dodávka do spotřeby, tu nelze ovlivnit.
  - Zbytek, který zůstane, už můžete ovlivnit, co se s ním stane.
  - Buď jde do baterie, nebo na prodej.
  - Prodej můžete nastavit různě:
    - Možnost prodat do GRIDu.
    - Možnost prodat energii v baterii v ranní nebo večerní špičce do nastaveného SOC, případně obojí.
    - Prodávat přebytek na SPOTu do nastavené hodnoty.
    - Možností je dost.
  - Při rozumném nastavení FLOW funguje zcela rozumně a automaticky.

- **Jak se chová FVE bez tohoto FLOW:**
  - Nejprve se uspokojí spotřeba z panelu.
  - Zbytek jde do baterie, až se baterie nabije, tak potom jde zbytek na prodej, pokud je povolen prodej (základní nastavení v Cerbu).
  - Bez povolení FLOW nikdy nic neprodá.
  - Tam se dá i nastavit omezení, kolik maximálního proudu pošle na GRID.

- **Co se stane, když nemám žádné řízení FVE a prodávám energii:**
  - To záleží na tom, jaký máte sjednaný výkup:
    - Za fixní ceny, tam je řízení zcela zbytečné.
    - Za spotové ceny:
      - Pokud máte SPOT záporný, tak za dodávku elektřiny platíte (platí, pokud nemáte žádné řízení FVE).
      - Pokud je nastavena "Limitní cena" menší než aktuální SPOT, tak prodáváte.
      - Pokud je nastavena "Limitní cena" větší než aktuální SPOT, tak se nic neprodává.

### Předpoklady:

- **Aktivace Node-RED:** Ujistěte se, že je Node-RED aktivován buď v CERBO (řídící jednotka pro FVE Victron) nebo hostován na jiném zařízení.
- **Přístup k FVE:** Musíte mít přihlašovací údaje pro přístup k FVE.
- **Konfigurace sítě:** Node-RED by měl být ve stejné síti jako FVE. Pokud ne, je třeba nastavit příslušná pravidla na firewallu/routeru nebo vytvořit trvalý most (např. OpenVPN) pro jejich propojení.
- Doporučuje se instalovat nejnovější verzi FLOW.

### Funkčnost:

- **Monitorování:** Monitorování výroby energie, spotřeby a úrovně úložiště v reálném čase.
- **Řízení:** Vzdálené ovládání parametrů a nastavení systému.
- **Pluginy**

### Pluginy:
- Chlazení měniču a MPTT trackerů: https://github.com/hacesoft/Cooling_Trackers_Rack
- HW Plugin - teploměry pro Cerbo: Pluginy: https://github.com/hacesoft/LM335

### Instalace a konfigurace:

- **Instalace Node-RED:** Postupujte podle oficiálního průvodce instalací Node-RED pro váš konkrétní operační systém.
- **Nainstalujde závislé knihovny** pro běh FLOW (seznam je na konci této stránky). Může se stát, že bude třeba restartovat Node-Red, postupujte podle pokynu na obrazovce. Zde je návod jak postupovat: [Node-RED Palette Manager](https://nodered.org/docs/user-guide/editor/palette/manager)
- **Nainstalujte FLOW prostřednictvím importu:**
  - Importujte soubor `ALL_flow_xxx.json` do Node-RED (kde `xxx` nahrazuje číslo verze). Zde je návod jak postupovat: [Node-RED Import/Export](https://nodered.org/docs/user-guide/editor/workspace/import-export)
  - Nebo: Klikněte kdekoliv na prázdné místo FLOW levým tlačítkem myši a vyberte: ![image](https://github.com/user-attachments/assets/644a08ea-4e1f-48ce-9c42-d9b74f0a1a06)
  - Na Import nodes vyberte kartu CLIPBOARD, klikněte na tlačítko select a file import, vyberte váš FLOW soubor a vše potvrďte.
- **Konfigurace FLOW:** Konfigurace se provádí z UI FLOW, to znamená: `http://IP:PORT/ui/` (příklad: `http://192.168.8.10:1881/ui/`)
  - `IP` je adresa vaší instalace Node-Red
  - `PORT` je hodnota kterou jste zadali při instalaci Node-Redu
- Přejděte na kartu CONFIG. Tam zadejte IP adresu (bez `http`) vaší FVE a klikněte na tlačítko CONNECT, v nastavení VRM propojíte FLOW s VRM. Pokud toto neprovedete, FLOW funguje nadále, jen nejsou zobrazovány tyto údaje: ![image](https://github.com/user-attachments/assets/0ab15b2c-12f5-42dc-b8d7-0cd2e5f728cd)
- Po nastavení provedte uložení konfigurace. Vše je popsáno níže v tomto textu.
- FLOW je komplexní nástroj pro řízení a monitorování FVE a bateriových úložišť, který neustále přináší nové funkce a vylepšení. Pro další informace a aktualizace sledujte vývojové záznamy a dokumentaci.

### Funkce a ovládací prvky:

- **Automatické řízení podle SPOTu:** Sleduje aktuální cenu spotu a řídí přetoky.
- **Přetoky:** Zapíná nebo vypíná přetoky.
- **Posunutí nabíjení baterie:** Nastavuje časové období pro nabíjení baterie.
- **Prodej baterie:** Funkce pro prodej baterie v ranní a večerní špičce.
- **Možnost konfigurace přímo z UI, bez zasahu do samotného FLOW.**
- **Ukládání a načítání defaultní konfigurace.**
- **Indikátory stavu připojení a chyb.**

>> Doporučuji vždy instalovat nejnovější verzi FLOW. Instalace se provádí importem FLOW do Node-RED, ale nejprve nezapomeňte nainstalovat závislé knihovny (ty jsou definované na konci této stránky, včetně verzí, na které je FLOW stavěno a testováno).

### Popis jednotlivých verzí:
 
- **Verze: Linea_flows_15032025.json**
  - Optimalizace FLOW
  - Přidání čítačů pro jednotlivé fáze. Pro GRID je rozlišeno na odběr (první parametr) a přetoky (označeno jako **O (OUTPUT)**).
    - Výpočet je prováděn následovně: aktuální vzorek (hodnota) je dělen 3600 a přičten k celkové hodnotě. Čítače se nulují o půlnoci.  
    - Časem budou tyto hodnoty logovány do databáze pomocí vhodného **PLUGINu**.
  - Přidány čítače energie pro baterii, zvlášť pro nabíjení a vybíjení. Tyto čítače se nenulují, vynulují se až při restartu **Node-RED**.  
    - Časem budou logovány do databáze.
  - Přidány teploty měničů – nutnost instalace teploměrů dle návodu: [hacesoft/LM335](https://github.com/hacesoft/LM335).
    - Aby to fungovalo, je třeba dodržet názvy teploměrů (**name L1** – jakékoliv vaše označení, mezera a velké písmeno **L**, následované číslem fáze).  
    - Další podmínkou je, že **FLOW** musí být propojeno s **VRM** účtem.  
    - ID teploměru se načítá z **VRM**, a podle tohoto ID se čtou hodnoty z **Modbusu**.

  
- **Verze: SELECTION_flows_20022025.json**
    - Opraveny drobné překlepy. Odstraněny grafy, které nikdy správně nefungovaly. Kód pro grafy je součástí FLOW, ale je pouze zakomentován. Odstraněn FLOW pro řízení chlazení měničů a FW regulátorů, které jsou ovládány přes Shelly plugin. Od této chvíle budou exportovány pouze relevantní karty, a to s pomocí doplňku: node-red-contrib-flow-manager.

- **Verze: K_ALL_flows_09022025.json:**
   - Upravená verze práce s tokenem. Token lze vygenerovat přímo ve VRM a manuálně vložit do souboru, nebo nechat vygenerovat pomocí FLOW. Toto je možnost, pokud z nějakého důvodu nemáte možnost generovat token. To může nastat, pokud nemáte dostatečná práva ve VRM pro integraci tohoto FLOW. Doporučujeme mít práva ADMIN, protože práva jako Technician nebo User nemusí být dostatečná. Ve všech ostatních verzích FLOW se používá bezpečnější BearerToken. Tato verze  (K_ALL_flows_09022025) pravděpodobně nebude dále rozvíjena.

- **Verze 07022025:**
  - Drobné úpravy kódu.
  - Přidány ikony.
  - Přidány výpočty úložení elektrické energie do a z baterie.

- **Verze 02112024:**
  - Byl přidán indikátor na kartě FVE::Real Data - doba provozu, který indikuje, jak dlouho dané FLOW běží. Při jakékoli změně se počitadlo nuluje.
  - Flow umí vyhodnotit, kde je spuštěno, a podle toho samo nastaví cestu k úložišti.

- **Verze 14102014:**
  - Upravena logika u funkcí prodej baterie v ranní i odpolední špičce.

- **Verze 28082024:**
  - Na kartě config tlačítko CONNECT je signalizováno indikátorem, zda je zařízení připojeno na danou IP adresu. Chyba v samotné knihovně node-red-contrib-modbus stále existuje. Celý fígl spočívá v tom, že flow se pokouší přečíst SN instalace. Pokud neuspěje nebo cca 4 sekundy nepřijde žádná informace o SN čísle, indikátor změní barvu na šedou. Po připojení indikátor změní barvu na zelenou. Komponenta node connection nevysílá žádné zprávy, tudíž jsem zavedl do flow časové razítko a pokud je platné, není dostupné zařízení na dané IP adrese.
  - Přidána defaultní konfigurace. Po spuštění flow na kartě config vidíte tlačítka DEFAULT LOAD a DEFAULT SAVE. Nastavte si vše, jak potřebujete, a proveďte uložení. Toto je základní nastavení, abyste nemuseli měnit nic v globální struktuře a při updatu na novou verzi opět vše měnit. Vše děláte jen v UI. Po tomto uložení tlačítka změní názvy na CONFIG LOAD a CONFIG SAVE, zde si nastavte, co potřebujete. Celý smysl toho je, že při restartu node-red se načte defaultní konfigurace. Takže tam doporučuji nastavit IP adresy a login k VRM, včetně základního nastavení přepínačů funkcí.

- **Verze 15092024:**
  - Přidána funkce GRID CHARGING nabíjení baterie z GRIDu. Nabíjecí proud se nastavuje na kartě CONFIG, položka MAX GRID POINT, a je jedno, zda uvedené číslo je kladné nebo záporné. Patřičné funkce si to upraví dle svého požadavku. Nastavíte, do jakého SOC má nabíjet, a po dosažení se automaticky vypne a dál nepokračuje. Není to funkce na udržení baterie na daném SOC, ale je to spíše míněno pro nouzové nabití baterie pro očekávaný nadcházející výpadek elektřiny. Do konfigurace se ukládá jen nastavená hodnota SOC.
  - Přidány diagnostické funkce ohledně vypadávání načítání dat z VRM. Možná bude hlásit chyby uložení souboru.
  - Opravena chyba ztráty tokenu pro VRM. Oprava je provedena takto: V případě ztráty tokenu je načtena konfigurace a vezme se jen hodnota tokenu, zbytek nastavení je ignorován.

- **Verze 25082024:**
  - Opraveny drobnosti v tooltipu.
  - Změna cesty ukládání konfiguračního souboru do root/mode_modules.
  - Token se ukládá do konfiguračního souboru. Když Node-Red běží v kontejneru, tak se často restartuje, a pak se ztratí připojení na VRM.
  - V Inmout boxu pro zadání Tokenu je primitivní test validace tokenu.

- **Verze 14082024:**
  - Opraveny drobné chyby v CSS profilu.
  - Opraveny drobné chyby ve FLOW.
  - Možnost konfigurovat konstantu: nBalancingReserve. Hodnota ve watech slouží k přičtení hodnoty po rozdílu mezi celkovou zátěží a aktuální výrobou z FV panelu. Rozdíl se pošle na GRID, aby se zamezilo kolísání nabíjení/vybíjení baterie. Tato konstanta má u mě hodnotu 230W. U jiného systému možná bude třeba upravit.
  - Přidán k LABELu “Údaje o instalaci:” aktuální čas a datum.

- **Verze 13082024:**
  - Konečně je dokončena funkce pro večerní prodej baterie.
  - Na kartě CONFIG je nyní možné nastavit i maximální vybíjecí proud ve W, pro ranní a večerní špičku dohromady.
  - Drobné opravy, hlavně interpretace hodnot FALSE a TRUE, pomocí dvou negací za sebou (příklad: !!fGetConfigProperty()).
  - Na kartě config nastavíte přístup k VRM. Zde zadáte své přihlašovací údaje (ukládá se pouze email, heslo nikoli). Také zadáte název vaší instalace, ke kterému se přidá nějaký náhodný řetězec. Dále je vyžadováno číslo vaší instalace, které najdete v URL vaší VRM instalace. Po připojení se vygeneruje token, který se nikam neukládá, ale existuje v globální proměnné tak dlouho, dokud nezrestartujete Cergo nebo kontejner, kde běží Node-RED. Pak je třeba provést novou žádost o token. Vygenerovaný token pak uvidíte ve vaší instalaci ve VRM v nastavení: “Předvolby/Integrace/Přístupové Tokeny”. Na kartě FVE v sloupci Real Data máte informace o vaší instalaci.
  - Karta RealTime Power je neustále ve vývoji, sice něco ukazuje, ale zatím se na to nedá spolehnout.

- **Verze 03082024:**
  - Opravena kritická chyba selhání SPOTU, po odstranění knihovny node-red-contrib-config, flow vyžadovalo větší upravu.

- **Verze 31072024:**
  - Odstranění zavislosti na knihovně: node-red-contrib-config.
  - Odstranění závislosti na knihovně: node-red-contrib-victron-modbus. Tuto knihovnu jsem nikdy nepoužil, a nakonec jsem ji úplně zavrhl.
  - FIX: Některé ovládací prvky při nahrání konfigurace řádně nereagovaly na aktuální nastavení.
  - Upozornění: Přepínač “Přetoky: Zap / Vyp” není a nebude ukládán do globální struktury, a tudíž nebude uložen s konfigurací, aby se zabránilo nechtěnému zapnutí přetoků. Toto je řízeno přepínačem automaticky spot a limitní cenou (trigger).

- **Verze 29072024:**
  - Přidána karta Config - z karty zatím funguje nastavení TCP, kde zadáte IP adresu vaší FVE, port a ID, které většinou nebudete měnit. Port je defaultně 502 a ID je defaultně 100. Potom dáte connect. Jelikož je v knihovně modbus chyba, nefunguje signalizace stavu připojení a pořád uvidíte CONNECTING…
  - Také je funkční nastavení FILE, kde když máte první instalaci, tak pokud neexistuje na disku (v ROOT adresáři node-red) konfigurační soubor, tak si ho vytvoří. Parametry nastavení si můžete upravit dle libosti a uložit tlačítkem SAVE CONFIG. Tlačítko DELETE CONFIG je dobré, když instalujete novou verzi FLOW, aby se načetly korektně defaultní hodnoty.
  - Napříč FLOW byly změněny proměnné ze samostatných definic na globální strukturu, která pak jde uložit na disk.

![image](https://github.com/user-attachments/assets/ef521fc2-1faa-478d-a702-8a99e7f5978a)

- **Verze 20072024:**
  - FIX CCS profil - Drobný detail, při zavírání karet se sloupce pohybovaly.
  - FIX flow chlazení FVE - Přidán node delay 3s pro posun paketu.

- **Verze 19072024:**
  - FIX funkce CopyOnChange_2707 - Teď se do registru 2707 zapisují a posílají jen změny, nikoliv stejná hodnota.
  - FIX function GLOBAL FUNCTION - Funkce sExtractTime a mExtractTime mají stejný časový základ pomocí funkce fSetFixedDate a nemůže se stát, že budou mít rozdílné hodnoty.
  - Nejvíce je přepracován flow chlazení FVE:
    - Kde jednak jsou indikátory, zda se ventilátory točí, je to odezva od PLUGINu SHELLY, která potvrdí přijetí příkazu. Není implementováno monitorování odběru ele.i.
    - Zapnutí ventilátoru je okamžité, jakmile dosáhne hodnoty triggeru, ale vypínám je až po přijetí 20x za sebou příkazu STOP. Tím se vyhneme nějakému mraku.

- **Verze 15072024:**
  - FIX funkce convert signet to unsigned na flow battery control.
  - FIX funkce convert unsigned to signet na flow battery control.
  - Částečně přepracován flow Spot Excess Control. Část node předělána do function node: Logical write register 2707.

- **Verze 14072024:**
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


### Nainstalované NODE:

 - node-red - 4.0.3
 - node-red-contrib-modbus - 5.42.0
 - node-red-dashboard - 3.6.5

---

## Pracujte pečlivě! Dvakrát měř, jednou řež...

---

## Zřeknutí se odpovědnosti

Autor tohoto projektu neposkytuje žádné záruky, výslovné ani implicitní, ohledně správnosti, spolehlivosti, funkčnosti nebo vhodnosti k jakémukoli účelu. Veškeré použití tohoto softwaru, kódu, schémat, návodů, technických řešení, produktů a jakýchkoli dalších poskytnutých materiálů je na vlastní odpovědnost uživatele.

Autor nenese žádnou odpovědnost za jakékoli škody, ztráty, finanční náklady, přímé či nepřímé škody vzniklé v důsledku použití těchto materiálů, a to včetně, ale nejen, ztráty dat, poškození zařízení, výpadků systému, poruchy elektrických či jiných instalací, požárů, ztrát příjmů nebo jiných nepředvídatelných následků.

Uživatel bere na vědomí, že jakékoli úpravy, sestavování, instalace, zapojení či implementace na základě poskytnutých informací provádí výhradně na vlastní riziko. Autor neposkytuje žádné garance funkčnosti, bezpečnosti ani souladu s platnými právními normami a předpisy.

Uživatel se zavazuje, že nevyužije žádné právní kroky vůči autorovi v souvislosti s jakýmikoli škodami nebo jinými nároky vyplývajícími z používání tohoto softwaru, produktů, schémat nebo návodů. Jakékoli právní nároky vůči autorovi jsou tímto výslovně vyloučeny a nevymahatelné, a to i soudní cestou.

Použitím těchto materiálů uživatel potvrzuje svůj souhlas s výše uvedenými podmínkami. Pokud s nimi nesouhlasíte, nepoužívejte tento software, schémata, návody ani jiné poskytnuté materiály.

