[🇨🇿 **Česky**](CHANGELOG_CZ.md) | [🇬🇧 English](CHANGELOG.md)

---

# Historie verzí LINEA

## Dokumentace 1.0.0 — 30.08.2026

První veřejná verze nové dokumentace LINEA / GridSight.

- kompletní česká a anglická dokumentace;
- hlavní `README_CZ.md` a `README.md` slouží zároveň jako úvod projektu;
- samostatná kapitola „Začínáme / Getting Started“ byla zrušena;
- číslované kapitoly dokumentace;
- CZ ↔ EN přepínače;
- samostatná dokumentace READ-ONLY LINEA API;
- interní relativní odkazy vhodné pro GitHub.

> Verze dokumentace je nezávislá na verzi LINEA FLOW a LINEA API.

---

## Historie LINEA FLOW

Důležitá poznámka: Od verze SELECTION_flows_04052025 je nutné mít CERBO aktualizované na minimálně verzi 3.50 a v měničích MultiPlus-II 48 minimální verze FW v510.

## SELECTION_flows_27072025.json

### Opravy a vylepšení
- Opraveny různé chyby v logice a zpracování dat
- Vylepšena stabilita a výkon systému
- Optimalizace kódu pro lepší čitelnost

### Vizuální vylepšení
- **Config karta:** Parametry jsou nyní seskupeny podle barev pro lepší orientaci
- Přidány barevné oddělovače pro jednotlivé funkční celky
- Zlepšena uživatelská přívětivost rozhraní

### Nová funkce: Spot-Grid Charging (Chytré nabíjení ze sítě)

**Umístění:** Config karta → Zelené nastavovací prvky

**Jak to funguje:**
- Systém automaticky najde **nejlevnější souvislý časový úsek** pro nabíjení ze sítě
- Při dosažení optimálního času aktivuje nabíjení s nastavením `-MAX GRID POINT`

** DŮLEŽITÉ UPOZORNĚNÍ:**
> **Nastavte například 4000W, ale systém může nabíjet pouze 2500W!**
> Toto je dáno nastavením v měniči. FLOW Linea **nemůže přenastavit** konfiguraci měniče.
> Pro změnu maximálního nabíjecího výkonu musíte **upravit nastavení přímo v měničích**.

**Nastavitelné parametry:**
- ** Délka nabíjení:** Lze nastavit dobu nákupu ze SPOTu.
- ** Cenový trigger:** Maximální přijatelná cena za kterou chcete nakupovat
- ** Informační displej:** Real-time zobrazení optimálního nabíjecího okna
  -  **Poznámka:** Label se aktualizuje pouze při zapnuté funkci Spot-Grid Charging

### Dva způsoby řízení nákupu

Máte k dispozici dvě možnosti řízení nákupu elektřiny:

1. **Trigger (pevná hodnota)** - nastavíte konkrétní cenovou hranici
2. **Peak (pohyblivá hodnota)** - systém hledá nejlevnější hodiny v daném období

### Kdy použít který způsob

**Scénář 1 - Stabilní ceny:**
Když jsou ceny elektřiny relativně stabilní, funguje dobře trigger.

**Scénář 2 - Vysoké a kolísavé ceny:**
Představte si situaci, kdy je elektřina drahá - dnes nejlevněji za 5 Kč/kWh, zítra nejlevněji za 6 Kč/kWh. V tomto případě by trigger na nízkou hodnotu nepomohl, protože by se nikdy nespustil. Zde je výhodnější hledání peaku, které najde nejlevnější hodiny bez ohledu na absolutní cenu.

**Doporučené nastavení pro vysoké ceny:**
- Trigger nastavte na rozumnou horní hranici (např. 15 Kč/kWh)
- Rozumně nastavte délku peaku
- Můžete používat jeden způsob samostatně nebo kombinovat oba


## SELECTION_flows_28062025.json

### Opravy a vylepšení
- **Opraveny chyby v časové synchronizaci**
  - Po spuštění FLOW se uskuteční první startovací sekvence v rozmezí cca 20 sekund, takže do minuty a půl je plně FLOW načteno a synchronizováno.
  - Čas se synchronizuje se skutečným časem - tato chyba byla patrná především při řízení SPOTu, kdy FLOW reagoval nesprávně na časové události.

- **Vylepšení konfigurace na kartě Config**
  - Parametry jsou rozděleny do více kategorií pro lepší přehlednost
  - Přidána možnost konfigurace délky peaku pro ranní a večerní prodej baterie (nastavitelné hodnoty od 1 do 4 hodin)
  - Přidáno zobrazení časů peaku, které FLOW LINEA automaticky vypočítá pro aktuální den

### Nová funkce: Dynamic SOC Reserve
Tato pokročilá funkce je dostupná pouze pro ranní prodej baterie a umožňuje inteligentní řízení rezervy energie v baterii.

**Princip fungování:**
1. Na kartě Config nastavíte, jak dlouho má funkce pracovat po svítání (parametr: `Set nSunriseProductionOffset`)
2. Nastavíte typický odběr elektrické energie za hodinu
3. Funkce automaticky spočítá, kolik energie je třeba pro provoz domácnosti do svítání (než bude možno opět brát elektrickou energii z FV panelů)
4. Přebytečnou energii nad tuto vypočtenou potřebu prodá do sítě
5. Do výpočtu se zahrnuje také nastavená hodnota SOC delta pro dodatečnou rezervu

**Zobrazení dat:**
- Při zadávání parametrů vidíte aktuální vypočtené hodnoty
- Na kartě FVE u příslušné funkce je zobrazena stejná vypočtená hodnota
- U funkce "Prodej baterie" je vidět aktuální SOC, do kterého se baterie prodává
- Pokud není funkce Dynamic SOC Reserve zapnuta, vypočtená hodnota se nepřičítá k celkovému SOC

### Požadavky pro Dynamic SOC Reserve
**Povinné nastavení:**
- Ve VRM musí být nastavena **pevná poloha FVE** (GPS souřadnice)
- Funkce je určena pouze pro **pevné instalace** - nefunguje pro lodě a karavany s GPS modulem
- FLOW LINEA musí být **přihlášen do VRM** pro načtení polohy instalace

**Technické detaily:**
- Systém využívá FREE API službu https://api.sunrise-sunset.org pro získání času východu slunce
- Sunrise-Sunset Response Processor načítá komplexní data o slunečních časech, aktuálně je využíván pouze čas východu slunce
- Data se načítají pouze **jednou denně** - buď bezprostředně po startu FLOW (do jedné minuty), nebo minutu po půlnoci
- Díky tomuto přístupu nedochází k nadměrnému zatěžování internetového připojení opakovanými dotazy

**Výhody:**
- Automatická optimalizace prodeje baterie na základě skutečných podmínek
- Inteligentní řízení rezervy energie s ohledem na denní cyklus
- Minimální zatížení internetového připojení díky jednomu dotazu denně
- Přesné výpočty založené na geografické poloze a ročním období

## SELECTION_flows_20052025.json
- Přidána funkce:
    - SOC delta před exportem :
      - Po zapnutí je aktivní ovládací prvek: SOC delta v %, kde nastavíte práh SOC baterie odkdy má posílat přebytky do sítě.
      - Je to dobré k tomu, že máte ráno baterii na třeba 20 SOC a chcete aby se baterka trošku nabila, třeba na SOC 30 a pak zbytek šel do sítě do uvedené doby, třeba
do 11:00 hodin, kdy třeba naskočí bojler.

## SELECTION_flows_04052025.json
- Opraveny drobné chyby napříč celého FLOW.
- Přídány funkce:
  - Control Mode: ESS / AC Grid
    - Funkce pro přepínání mezi dvěma režimy řízení fotovoltaického systému
    - Pozice je indikována zeleným trojúhelníkem
    - Přepíná mezi registry 2700 a 2716
  - NON Battery Priority Mode :
    - Přepínač pro aktivaci režimu, který upřednostňuje použití energie ze sítě nebo FV panelů před energií z baterie
    - Ochrana baterie při nabíjení elektromobilu nebo jiné energeticky náročné spotřeby

## SELECTION_flows_14042025.json
- Opraveny chyby v rozdělení hodin.
- Opraveny chyby v parsování SPOTu.
- Opravena chyba s VRM portálem.

## SELECTION_flows_05042025.json
- Opravené nalezené chyby.
- Opravena práce s hodinami (CLK).
- Přepracováno flow SPOTu.
- Přidána funkce: Energy Threshold Injector. Funkci nebudu blíže komentovat, slouží pro mé účely. :)

## Selection_flows_28032025.json
- Ovládání nastavení registru 2706 (Maximum System Grid Feed In):
  - Pokud jsou povoleny přetoky (registr 2707, pouze DC!), je třeba mít na paměti, že FLOW LINEA ovládá pouze přetoky DC, tedy z fotovoltaických panelů. Pokud máte generátor nebo jiné zařízení připojené na AC, FLOW LINEA s ním pracovat neumí. Nicméně, lze toto nastavení relativně snadno rozšířit pomocí registru 2708.
  - Existují dva režimy:
    - Baterie je plně nabita a není dostatečný odběr.
      - V tomto případě se uplatní nastavení z registru 2706, což znamená, že maximální množství energie, které se pošle do sítě, je dáno hodnotou v tomto registru.
    - Baterie není plně nabita, ale přesto chceme posílat přetoky do sítě podle hodnoty v registru 2706 (Maximum System Grid Feed In).
      - Toto řeší FLOW LINEA následovně:
         - Jednou za sekundu (častější kontrola nemá smysl) odečte od aktuální výroby spotřebu a vyvažovací rezervu.
         - Výslednou hodnotu nastaví do registru 2700 (ESS control loop setpoint).
         - Pokud je tato hodnota větší než hodnota v registru 2706, nastaví do registru 2700 hodnotu shodnou s hodnotou v registru 2706.
         - Tímto způsobem může docházet k překmitům dodávaného elektrického proudu do sítě. Tento jev trvá, dokud Cerbo vše nezpracuje a systém nevyrovná podle nastavených hodnot v registrech. Proto také nikdy nenastavujte do registru 2700 (ESS control loop setpoint) shodnou hodnotu, kterou máte povolenou od distributora sítě, ale vždy nižší.
         - Když je výroba z fotovoltaických panelů menší než spotřeba plus vyvažovací rezerva a přesto je zapnutá funkce Posunutí nabíjení baterie, tak se na síť nic nepošle.
         - Když je zapnutá funkce ranního nebo večerního prodeje baterie, nebo obě, tak se maximální proud do sítě nastavuje zvlášť a nebere v potaz nastavení registru 2706. I když nastavíte nějakou velkou hodnotu, nemusí být akceptována měniči, které pak posílají tolik, kolik mají nastavené v konfiguraci systému FVE.
## Linea_flows_15032025.json
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
## SELECTION_flows_20022025.json
- Opraveny drobné překlepy. Odstraněny grafy, které nikdy správně nefungovaly. Kód pro grafy je součástí FLOW, ale je pouze zakomentován. Odstraněn FLOW pro řízení chlazení měničů a FW regulátorů, které jsou ovládány přes Shelly plugin. Od této chvíle budou exportovány pouze relevantní karty, a to s pomocí doplňku: node-red-contrib-flow-manager.
## K_ALL_flows_09022025.json
- Upravená verze práce s tokenem. Token lze vygenerovat přímo ve VRM a manuálně vložit do souboru, nebo nechat vygenerovat pomocí FLOW. Toto je možnost, pokud z nějakého důvodu nemáte možnost generovat token. To může nastat, pokud nemáte dostatečná práva ve VRM pro integraci tohoto FLOW. Doporučujeme mít práva ADMIN, protože práva jako Technician nebo User nemusí být dostatečná. Ve všech ostatních verzích FLOW se používá bezpečnější BearerToken. Tato verze  (K_ALL_flows_09022025) pravděpodobně nebude dále rozvíjena.
## 07022025
- Drobné úpravy kódu.
- Přidány ikony.
- Přidány výpočty úložení elektrické energie do a z baterie.
## 02112024
- Byl přidán indikátor na kartě FVE::Real Data - doba provozu, který indikuje, jak dlouho dané FLOW běží. Při jakékoli změně se počitadlo nuluje.
- Flow umí vyhodnotit, kde je spuštěno, a podle toho samo nastaví cestu k úložišti.
## 14102024
- Upravena logika u funkcí prodej baterie v ranní i odpolední špičce.
## 28082024
- Na kartě config tlačítko CONNECT je signalizováno indikátorem, zda je zařízení připojeno na danou IP adresu. Chyba v samotné knihovně node-red-contrib-modbus stále existuje. Celý fígl spočívá v tom, že flow se pokouší přečíst SN instalace. Pokud neuspěje nebo cca 4 sekundy nepřijde žádná informace o SN čísle, indikátor změní barvu na šedou. Po připojení indikátor změní barvu na zelenou. Komponenta node connection nevysílá žádné zprávy, tudíž jsem zavedl do flow časové razítko a pokud je platné, není dostupné zařízení na dané IP adrese.
- Přidána defaultní konfigurace. Po spuštění flow na kartě config vidíte tlačítka DEFAULT LOAD a DEFAULT SAVE. Nastavte si vše, jak potřebujete, a proveďte uložení. Toto je základní nastavení, abyste nemuseli měnit nic v globální struktuře a při updatu na novou verzi opět vše měnit. Vše děláte jen v UI. Po tomto uložení tlačítka změní názvy na CONFIG LOAD a CONFIG SAVE, zde si nastavte, co potřebujete. Celý smysl toho je, že při restartu node-red se načte defaultní konfigurace. Takže tam doporučuji nastavit IP adresy a login k VRM, včetně základního nastavení přepínačů funkcí.
## 15092024
- Přidána funkce GRID CHARGING nabíjení baterie z GRIDu. Nabíjecí proud se nastavuje na kartě CONFIG, položka MAX GRID POINT, a je jedno, zda uvedené číslo je kladné nebo záporné. Patřičné funkce si to upraví dle svého požadavku. Nastavíte, do jakého SOC má nabíjet, a po dosažení se automaticky vypne a dál nepokračuje. Není to funkce na udržení baterie na daném SOC, ale je to spíše míněno pro nouzové nabití baterie pro očekávaný nadcházející výpadek elektřiny. Do konfigurace se ukládá jen nastavená hodnota SOC.
- Přidány diagnostické funkce ohledně vypadávání načítání dat z VRM. Možná bude hlásit chyby uložení souboru.
- Opravena chyba ztráty tokenu pro VRM. Oprava je provedena takto: V případě ztráty tokenu je načtena konfigurace a vezme se jen hodnota tokenu, zbytek nastavení je ignorován.
## 25082024
- Opraveny drobnosti v tooltipu.
- Změna cesty ukládání konfiguračního souboru do root/mode_modules.
- Token se ukládá do konfiguračního souboru. Když Node-Red běží v kontejneru, tak se často restartuje, a pak se ztratí připojení na VRM.
- V Inmout boxu pro zadání Tokenu je primitivní test validace tokenu.
## 14082024
- Opraveny drobné chyby v CSS profilu.
- Opraveny drobné chyby ve FLOW.
- Možnost konfigurovat konstantu: nBalancingReserve. Hodnota ve watech slouží k přičtení hodnoty po rozdílu mezi celkovou zátěží a aktuální výrobou z FV panelu. Rozdíl se pošle na GRID, aby se zamezilo kolísání nabíjení/vybíjení baterie. Tato konstanta má u mě hodnotu 230W. U jiného systému možná bude třeba upravit.
- Přidán k LABELu "Údaje o instalaci:" aktuální čas a datum.
## 13082024
- Konečně je dokončena funkce pro večerní prodej baterie.
- Na kartě CONFIG je nyní možné nastavit i maximální vybíjecí proud ve W, pro ranní a večerní špičku dohromady.
- Drobné opravy, hlavně interpretace hodnot FALSE a TRUE, pomocí dvou negací za sebou (příklad: !!fGetConfigProperty()).
- Na kartě config nastavíte přístup k VRM. Zde zadáte své přihlašovací údaje (ukládá se pouze email, heslo nikoli). Také zadáte název vaší instalace, ke kterému se přidá nějaký náhodný řetězec. Dále je vyžadováno číslo vaší instalace, které najdete v URL vaší VRM instalace. Po připojení se vygeneruje token, který se nikam neukládá, ale existuje v globální proměnné tak dlouho, dokud nezrestartujete Cergo nebo kontejner, kde běží Node-RED. Pak je třeba provést novou žádost o token. Vygenerovaný token pak uvidíte ve vaší instalaci ve VRM v nastavení: "Předvolby/Integrace/Přístupové Tokeny". Na kartě FVE v sloupci Real Data máte informace o vaší instalaci.
- Karta RealTime Power je neustále ve vývoji, sice něco ukazuje, ale zatím se na to nedá spolehnout.
## 03082024
- Opravena kritická chyba selhání SPOTU, po odstranění knihovny node-red-contrib-config, flow vyžadovalo větší upravu.
## 31072024
- Odstranění zavislosti na knihovně: node-red-contrib-config.
- Odstranění závislosti na knihovně: node-red-contrib-victron-modbus. Tuto knihovnu jsem nikdy nepoužil, a nakonec jsem ji úplně zavrhl.
- FIX: Některé ovládací prvky při nahrání konfigurace řádně nereagovaly na aktuální nastavení.
- Upozornění: Přepínač "Přetoky: Zap / Vyp" není a nebude ukládán do globální struktury, a tudíž nebude uložen s konfigurací, aby se zabránilo nechtěnému zapnutí přetoků. Toto je řízeno přepínačem automaticky spot a limitní cenou (trigger).
## 29072024
- Přidána karta Config - z karty zatím funguje nastavení TCP, kde zadáte IP adresu vaší FVE, port a ID, které většinou nebudete měnit. Port je defaultně 502 a ID je defaultně 100. Potom dáte connect. Jelikož je v knihovně modbus chyba, nefunguje signalizace stavu připojení a pořád uvidíte CONNECTING…
- Také je funkční nastavení FILE, kde když máte první instalaci, tak pokud neexistuje na disku (v ROOT adresáři node-red) konfigurační soubor, tak si ho vytvoří. Parametry nastavení si můžete upravit dle libosti a uložit tlačítkem SAVE CONFIG. Tlačítko DELETE CONFIG je dobré, když instalujete novou verzi FLOW, aby se načetly korektně defaultní hodnoty.
- Napříč FLOW byly změněny proměnné ze samostatných definic na globální strukturu, která pak jde uložit na disk.
## 20072024
- FIX CCS profil - Drobný detail, při zavírání karet se sloupce pohybovaly.
- FIX flow chlazení FVE - Přidán node delay 3s pro posun paketu.
## 19072024
- FIX funkce CopyOnChange_2707 - Teď se do registru 2707 zapisují a posílají jen změny, nikoliv stejná hodnota.
- FIX function GLOBAL FUNCTION - Funkce sExtractTime a mExtractTime mají stejný časový základ pomocí funkce fSetFixedDate a nemůže se stát, že budou mít rozdílné hodnoty.
- Nejvíce je přepracován flow chlazení FVE:
  - Kde jednak jsou indikátory, zda se ventilátory točí, je to odezva od PLUGINu SHELLY, která potvrdí přijetí příkazu. Není implementováno monitorování odběru ele.i.
  - Zapnutí ventilátoru je okamžité, jakmile dosáhne hodnoty triggeru, ale vypínám je až po přijetí 20x za sebou příkazu STOP. Tím se vyhneme nějakému mraku.
## 15072024
- FIX funkce convert signet to unsigned na flow battery control.
- FIX funkce convert unsigned to signet na flow battery control.
- Částečně přepracován flow Spot Excess Control. Část node předělána do function node: Logical write register 2707.
## 14072024
- Přidán flow pro přímé ovládání ventilátoru pomocí pluginu Shelly. Pro vaše účely je třeba upravit nebo úplně vymazat. Není to úplně dokončené, hlavně GUI je nedokončené a nepraktické.
- Už funguje funkce prodeje ranní špičky, zatím je ranní špička definována na úsek 2 hodin.
- Přidány testovací výpisy, stačí v příslušném node zapnout DEBUG na true a případně si upravit požadovaný výpis proměnné.
- Přidány globální funkce - takže se opakující funkce napíšou jen jednou a v dalších node function se jen načtou.
- Opravena práce s časem napříč flow. Od této verze se zadává v globálních proměnných na flow GUI County_Code a TZidentifier.
