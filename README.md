# LINEA - Ovládání fotovoltaické elektrárny pomocí Node-RED

<img width="1142" height="857" alt="image" src="https://github.com/user-attachments/assets/8c4bf9e9-d408-4afd-a879-3800c1791052" />

## 📑 Obsah
- [🔗 Nejnovější verze FLOW](#nejnovejsi-flow)
- [📌 Úvod](#úvod)
- [❓ FAQ - Často kladené otázky](#faq)
- [💻 Předpoklady a instalace](#předpoklady-a-instalace)
- [🛠️ Funkčnost a pluginy](#funkčnost-a-pluginy)
- [📊 Popis funkcí ovládacího panelu](#popis-funkcí-ovládacího-panelu)
- [⚙️ Vysvětlení funkce registrů](#vysvětlení-funkce-registrů)
- [📝 Historie verzí](#historie-verzí)
- [📦 Nainstalované NODE](#nainstalované-node)
- [⚠️ Zřeknutí se odpovědnosti](#zřeknutí-se-odpovědnosti)

## 🔗 [Nejnovější verze FLOW](#nejnovejsi-flow)  <- Zkratka na nejnovější verzi FLOW.

## Úvod

FLOW je rozšíření pro Node-RED, které poskytuje komplexní sadu funkcí pro řízení a monitorování fotovoltaických elektráren (FVE VICTRON) a bateriových úložišť.

### Klíčové detaily:

- **Hostování a nastavení:** FLOW běží v rámci Node-RED, který je nutné nainstalovat na hostujícím zařízení, jako je NAS, notebook nebo PC, které je trvale zapnuto.

Budu rád za jakoukoliv zpětnou odezvu a případnou opravu chyb a vylepšení.

## FAQ

### Jak nainstalovat FLOW LINEA poprvé?
- Krok 1: Instalace závislých knihoven
  -  Před instalací FLOW LINEA je nezbytné nainstalovat všechny závislé knihovny
  -  Seznam potřebných knihoven najdete na konci této stránky
  -  Ujistěte se, že všechny závislosti jsou správně nainstalovány
- Krok 2: Import FLOW LINEA
  - Importujte soubory FLOW LINEA do vašeho prostředí
  - Po importu proveďte DEPLOY
  - Pokud proběhlo DEPLOY bez chyby, import se povedl
- Krok 3: Základní konfigurace
  - Na kartě FVE nastavte základní přesun ovládacích prvků a požadované hodnoty
  - Toto nastavení bude použito jako výchozí (defaultní)
  - Přejděte na kartu CONFIG a vyplňte minimálně:
    - IP adresu FVE
    - Přihlašovací údaje pro VRM 
- DŮLEŽITÉ: Nezapomeňte uložit konfiguraci!
- Bez uložené konfigurace nebude FLOW fungovat korektně

### Jak aktualizovat FLOW LINEA na novou verzi?
- Krok 1: Kontrola a aktualizace knihoven
  - Zkontrolujte, zda jsou k dispozici novější verze závislých knihoven
  - Aktualizujte všechny knihovny, které mají novější verze
- Krok 2: Odstranění staré verze
  - Smažte všechny existující karty FLOW LINEA
- Krok 3: Import nové verze
  - Importujte soubory nové verze FLOW LINEA
  - Proveďte DEPLOY
  - Úspěšný DEPLOY bez chyb znamená, že import proběhl v pořádku
- Krok 4: Konfigurace po aktualizaci
  - Na kartě FVE nastavte základní přesun ovládacích prvků a požadované hodnoty
  - Přejděte na kartu CONFIG a:
    - Vyplňte IP adresu FVE
    - Přihlašovací údaje pro VRM nemusíte vyplňovat, pokud již byly nastaveny v předchozí verzi
- Krok 5: Uložení konfigurace
  - KRITICKY DŮLEŽITÉ: I když jste neprovedli žádné změny v konfiguraci, vždy musíte provést uložení konfigurace
  - Uložením konfigurace se aktivují i nové položky, které předchozí verze FLOW neobsahovala
  - Bez tohoto kroku nebude FLOW fungovat správně
- Poznámka: Pokud konfigurace není uložena po jakékoli instalaci nebo aktualizaci, FLOW LINEA nebude fungovat korektně.

### Nemám povolení přetoky
V tomto případě je jakékoli řízení zbytečné, není co řídit.

### Mám povolené přetoky
- Pokud mám povolené přetoky od distributora a chci prodávat přetoky, je toto řešení FLOW ideální.
- Nebo se můžete podívat po jiném řešení (např. Delte Green, řízení přímo ve VRM).

### Přetoky už ovládá jiné řešení
Je možné použít jen jedno ovládání přetoků, jinak se budou různá ovládání přetahovat o řízení.

### Mám omezené povolené přetoky
Nevadí, nastavte v Cerbu maximální dovolené povolené přetoky, minus nějaká rezerva, třeba 500 W.

### Jak se FVE bude ovládat
- První prioritou je dodávka do spotřeby, tu nelze ovlivnit.
- Zbytek, který zůstane, už můžete ovlivnit, co se s ním stane.
- Buď jde do baterie, nebo na prodej.
- Prodej můžete nastavit různě:
  - Možnost prodat do GRIDu.
  - Možnost prodat energii v baterii v ranní nebo večerní špičce do nastaveného SOC, případně obojí.
  - Prodávat přebytek na SPOTu do nastavené hodnoty.
  - Možností je dost.
  - Registr 2706 (Maximum System Grid Feed In)
    - Pokud jsou povoleny přetoky (registr 2707, pouze DC!), je třeba mít na paměti, že FLOW LINEA ovládá pouze přetoky DC, tedy z fotovoltaických panelů. Pokud máte generátor nebo jiné zařízení připojené na AC, FLOW LINEA s ním pracovat neumí. Nicméně, lze toto nastavení relativně snadno rozšířit pomocí registru 2708.
- Při rozumném nastavení FLOW funguje zcela rozumně a automaticky.

### Jak se chová FVE bez tohoto FLOW
- Nejprve se uspokojí spotřeba z panelu.
- Zbytek jde do baterie, až se baterie nabije, tak potom jde zbytek na prodej, pokud je povolen prodej (základní nastavení v Cerbu).
- Bez povolení FLOW nikdy nic neprodá.
- Tam se dá i nastavit omezení, kolik maximálního proudu pošle na GRID.
- Nastavení v registru 2706 je akceptováno. To znamená, že co tam je nastaveno, to se při přetoku pošle, pokud je plná baterie a není žádný odběr. Pak Cerbo omezuje výrobu z MPPT.

### Co se stane, když nemám žádné řízení FVE a prodávám energii
- To záleží na tom, jaký máte sjednaný výkup:
  - Za fixní ceny, tam je řízení zcela zbytečné.
  - Za spotové ceny:
    - Pokud máte SPOT záporný, tak za dodávku elektřiny platíte (platí, pokud nemáte žádné řízení FVE).
    - Pokud je nastavena "Limitní cena" menší než aktuální SPOT, tak prodáváte.
    - Pokud je nastavena "Limitní cena" větší než aktuální SPOT, tak se nic neprodává.

## Předpoklady a instalace

### Předpoklady:

- **Aktivace Node-RED:** Ujistěte se, že je Node-RED aktivován buď v CERBO (řídící jednotka pro FVE Victron) nebo hostován na jiném zařízení.
- **Přístup k FVE:** Musíte mít přihlašovací údaje pro přístup k FVE.
- **Konfigurace sítě:** Node-RED by měl být ve stejné síti jako FVE. Pokud ne, je třeba nastavit příslušná pravidla na firewallu/routeru nebo vytvořit trvalý most (např. OpenVPN) pro jejich propojení.
- Doporučuje se instalovat nejnovější verzi FLOW.

### Instalace a konfigurace:

- **Instalace Node-RED:** Postupujte podle oficiálního průvodce instalací Node-RED pro váš konkrétní operační systém.
- **Nainstalujde závislé knihovny** pro běh FLOW (seznam je na konci této stránky). Může se stát, že bude třeba restartovat Node-Red, postupujte podle pokynu na obrazovce. Zde je návod jak postupovat: [Node-RED Palette Manager](https://nodered.org/docs/user-guide/editor/palette/manager)
- **Nainstalujte FLOW prostřednictvím importu:**
  - Importujte soubor `SELECTION_flows_xxx.json` do Node-RED (kde `xxx` nahrazuje číslo verze). Zde je návod jak postupovat: [Node-RED Import/Export](https://nodered.org/docs/user-guide/editor/workspace/import-export)
  - Nebo: Klikněte kdekoliv na prázdné místo FLOW levým tlačítkem myši a vyberte: 
  
  ![Import menu](https://github.com/user-attachments/assets/644a08ea-4e1f-48ce-9c42-d9b74f0a1a06)
  
  - Na Import nodes vyberte kartu CLIPBOARD, klikněte na tlačítko select a file import, vyberte váš FLOW soubor a vše potvrďte.
- **Konfigurace FLOW:** Konfigurace se provádí z UI FLOW, to znamená: `http://IP:PORT/ui/` (příklad: `http://192.168.8.10:1881/ui/`)
  - `IP` je adresa vaší instalace Node-Red
  - `PORT` je hodnota kterou jste zadali při instalaci Node-Redu
- Přejděte na kartu CONFIG. Tam zadejte IP adresu (bez `http`) vaší FVE a klikněte na tlačítko CONNECT, v nastavení VRM propojíte FLOW s VRM. Pokud toto neprovedete, FLOW funguje nadále, jen nejsou zobrazovány tyto údaje: 

![VRM data](https://github.com/user-attachments/assets/0ab15b2c-12f5-42dc-b8d7-0cd2e5f728cd)

- Po nastavení provedte uložení konfigurace. Vše je popsáno níže v tomto textu.

## Funkčnost a pluginy

### Funkčnost:

- **Monitorování:** Monitorování výroby energie, spotřeby a úrovně úložiště v reálném čase.
- **Řízení:** Vzdálené ovládání parametrů a nastavení systému.
- **Pluginy**

### Pluginy:
- Chlazení měniču a MPTT trackerů: https://github.com/hacesoft/Cooling_Trackers_Rack
- HW Plugin - teploměry pro Cerbo: Pluginy: https://github.com/hacesoft/LM335
- Návod na obládání bojleru pomocí třeba NODE-RED: https://github.com/hacesoft/Clever_boiler
- ModBus Scanner: https://github.com/hacesoft/Scanner_ModBus
- Body for Victron 7 inch display: https://github.com/hacesoft/7_inch_display_for_victron


### Funkce a ovládací prvky:

- **Automatické řízení podle SPOTu:** Sleduje aktuální cenu spotu a řídí přetoky.
- **Přetoky:** Zapíná nebo vypíná přetoky.
- **Posunutí nabíjení baterie:** Nastavuje časové období pro nabíjení baterie.
- **Prodej baterie:** Funkce pro prodej baterie v ranní a večerní špičce.
- **Možnost konfigurace přímo z UI, bez zasahu do samotného FLOW.**
- **Ukládání a načítání defaultní konfigurace.**
- **Indikátory stavu připojení a chyb.**

> Doporučuji vždy instalovat nejnovější verzi FLOW. Instalace se provádí importem FLOW do Node-RED, ale nejprve nezapomeňte nainstalovat závislé knihovny (ty jsou definované na konci této stránky, včetně verzí, na které je FLOW stavěno a testováno).

## Popis funkcí ovládacího panelu

### 1. Sekce Real Data - Detailní přehled (karta FVE)

#### Zátěž a spotřeba po fázích

- **Total AC zátěž:** Celková aktuální spotřeba všech fází v W
- **L1:** Aktuální spotřeba na fázi L1 v W (v závorce denní spotřeba v kWh a teplota měniče ve °C)
- **L2:** Aktuální spotřeba na fázi L2 v W (v závorce denní spotřeba v kWh a teplota měniče ve °C)
- **L3:** Aktuální spotřeba na fázi L3 v W (v závorce denní spotřeba v kWh a teplota měniče ve °C)

#### Síť

- **Total Síť:** Celkový aktuální odběr/dodávka do sítě v W (kladná hodnota = odběr, záporná = dodávka)
- **L1:** Aktuální odběr/dodávka na fázi L1 v W (v závorce denní čítač Wh a Wh)
- **L2:** Aktuální odběr/dodávka na fázi L2 v W (v závorce denní čítač Wh a Wh)
- **L3:** Aktuální odběr/dodávka na fázi L3 v W (v závorce denní čítač Wh a Wh)

#### Informace o provozu

- **Doba provozu:** Jak dlouho FLOW běží od posledního restartu
- **Údaje o instalaci:** Aktuální datum a čas systému

#### Přetoky a spotřeba

- **Do sítě:** Množství energie dodané do sítě v kWh
- **Ze sítě:** Množství energie odebrané ze sítě v kWh
- **Spotřeba predikce:** Předpokládaná celková spotřeba v kWh
- **Spotřeba celkem:** Aktuální celková spotřeba v kWh
- **Solární predikce:** Předpokládaná výroba ze solárních panelů v kWh
- **Solární energie celkem:** Aktuální výroba ze solárních panelů v kWh

#### Stav baterie

- **Baterie (Limit SoC):** Aktuální stav nabití baterie v procentech s informací o minimálním nastaveném limitu SoC
- **Vybíjení baterie:** Aktuální výkon vybíjení baterie v W (záporná hodnota = vybíjení)
- **Vybíjecí proud baterie:** Aktuální proud vybíjení v A
- **Napětí baterie:** Aktuální napětí baterie ve V
- **Battery efficiency:** Účinnost baterie v procentech (poměr odebrané energie k dodané)

#### Čítače energie:
- **První ikona baterie:** Celkové množství energie dodané do baterie v Wh
- **Druhá ikona baterie:** Celkové množství energie odebrané z baterie v Wh

#### Stav regulátorů a ventilace

- **FW regulátory:** Celkový výkon FW regulátorů v W
- **MPTT FAN:** Indikátor stavu ventilátoru pro MPPT regulátory (zelená = zapnuto)
- **CHANGER FAN:** Indikátor stavu ventilátoru pro měnič (zelená = zapnuto)

**Poznámka:** Údaje o teplotách měničů a predikce jsou dostupné pouze při aktivním propojení s VRM portálem. Čítače energie u baterie se nulují pouze při restartu Node-RED.

### 2. Sekce řízení přetoků a SPOT (karta FVE)

#### Základní údaje

- **Přetoky:** Aktuální stav povolení přetoků do sítě ("ZAKÁZÁNY" nebo "POVOLENY")
- **Automaticky podle SPOTu:** Přepínač pro aktivaci automatického řízení podle ceny na SPOTu
- **Připojení na SPOT:** Indikátor stavu připojení k datovému zdroji SPOT cen
- **Aktuální SPOT cena:** Zobrazuje aktuální cenu elektřiny na SPOTu v Kč/kWh
- **Limitní cena:** Nastavitelná cenová hranice pro aktivaci přetoků v Kč/kWh
- **Přetoky: Vyp / Zap:** Manuální přepínač pro povolení přetoků

#### Denní přehled SPOT cen

- **Dnes: Min / Max:** Minimální a maximální cena SPOTu pro aktuální den v Kč/kWh
- **Dnešní SPOT:** Graf zobrazující průběh SPOT cen během aktuálního dne
  - Oranžová křivka: Průběh SPOT cen (Kč/kWh)
  - Zelená linie: Nastavená limitní cena
  - Červená značka: Aktuální pozice v čase

#### Budoucí SPOT ceny

- **Zítra: Min / Max:** Minimální a maximální očekávaná cena SPOTu pro následující den
- **Zítřejší SPOT:** Graf očekávaných SPOT cen pro následující den

**Poznámka:** Data o SPOT cenách pro následující den jsou zveřejňována Operátorem trhu s elektřinou (OTE) kolem 14:00 hodin předchozího dne. Do té doby zobrazuje systém informaci "nedostupná data". OTE je zodpovědný za organizaci krátkodobého trhu s elektřinou a plynem v České republice a zveřejňuje tyto údaje na základě výsledků obchodování na denním trhu.

### 3. Karta FVE - Další funkce

#### Funkce pro řízení baterie

- **Posunutí nabíjení baterie:** Přepínač pro aktivaci odložení nabíjení baterie
  - Umožňuje posunout nabíjení na pozdější dobu, kdy je nízká spotřeba elektřiny
  - Využívá se, když je výhodnější prodat elektřinu než ji ukládat do baterie

#### Funkce prodeje baterie

- **Prodej baterie ráno:** Přepínač pro aktivaci prodeje energie z baterie v ranní špičce
  - Nastavitelný časový interval a cílové SOC
  - Ideální pro využití vyšších cen elektřiny v ranních hodinách

- **Prodej baterie večer:** Přepínač pro aktivaci prodeje energie z baterie ve večerní špičce
  - Nastavitelný časový interval a cílové SOC
  - Vhodné pro prodej energie v době večerní špičky

#### NON Battery Priority Mode

- Přepínač pro aktivaci režimu, který upřednostňuje použití energie ze sítě nebo FV panelů před energií z baterie
- Ochrana baterie při nabíjení elektromobilu nebo jiné energeticky náročné spotřeby

#### Energy Threshold Injector

- Funkce se chová stejně jako funkce Set Point, ale s tím rozdílem, že je aktivní pouze po dobu dodávky z FV panelů
- Pokud je nedostatečná dodávka z FV panelů, funkce nic nedělá
- Využívá se pro optimalizaci toků energie jen v době, kdy máme dostatečnou výrobu

#### Set Point

- Nastavuje konstantní hodnotu výkonu (kladnou nebo zápornou) dle nastavovacího prvku AcPowerSetPoint
- Výhodné pro minimalizaci nežádoucího odběru ze sítě - například nastavením na -100W
- Systém se bude snažit dostat na tuto hodnotu a nebude oscilovat kolem 0, ale kolem nastavené hodnoty

#### GRID CHARGING

- Přepínač pro aktivaci nabíjení baterie ze sítě
- Využitelné při očekávaném výpadku elektřiny
- Nastavitelné cílové SOC, po jehož dosažení se nabíjení automaticky vypne

### 4. Control Mode: ESS / AC Grid

- Funkce pro přepínání mezi dvěma režimy řízení fotovoltaického systému
- Pozice je indikována zeleným trojúhelníkem
- Přepíná mezi registry 2700 a 2716

#### ESS Mode (registr 2700):

- Implementován ve FLASH paměti
- Řídí tok energie mezi bateriemi, sítí a spotřebou
- Klasický režim pro běžné použití

#### AC Grid Mode (registr 2716):

- Implementován v RAM paměti oproti původnímu registru 2700, který je implementován ve FLASH
- Registr 2716 je 32 bitový
- Umožňuje přímé řízení výkonu dodávaného do sítě
- Výhoda: šetří životnost FLASH paměti, která má omezený počet zápisů

**Důležitá poznámka:** Pro použití registru 2716 je nutné mít CERBO aktualizované na minimálně verzi 3.50 a v měničích MultiPlus-II 48 je třeba minimální firmware v510.

### 5. Podrobný popis specifických funkcí

#### Energy Threshold Injector

- Funguje stejně jako Set Point, ale aktivuje se pouze při dostatečné dodávce energie z FV panelů
- Při nedostatečné výrobě z panelů funkce neprovádí žádnou činnost
- Umožňuje nastavit parametry pro řízení přetoků bez ovlivnění systému v době nízké výroby
- Vhodné pro zajištění stabilních přetoků pouze při dostatečné výrobě z FV panelů

#### NON Battery Priority Mode
Tato funkce umožňuje upřednostnit využití elektřiny přímo ze sítě nebo z FV panelů před čerpáním z baterií:

- **Primární účel:** Ochrana baterie, zejména při nabíjení elektromobilu
- **Princip fungování:**
  - Pokud je zapnutá, systém preferuje napájení ze sítě nebo z FV panelů
  - Energie z baterie se používá pouze tehdy, když ostatní zdroje nestačí pokrýt spotřebu
  - Při nabíjení elektromobilu nebo jiné náročné spotřeby se šetří bateriový systém

- **Výpočet velikosti výkonu:**
  - Systém vypočítává rozdíl mezi aktuální výrobou FV, celkovou spotřebou a vyvažovací rezervou
  - Pokud je výsledek záporný, převede se na kladnou hodnotu pomocí funkce Math.abs()
  - Tato hodnota se nastaví jako požadavek na odběr ze sítě

- **Výhody:**
  - Prodloužení životnosti baterie díky menšímu počtu cyklů
  - Ochrana před hloubkovým vybíjením při náročných spotřebách
  - Ideální pro pravidelné nabíjení elektromobilu

### 6. Karta CONFIG - Nastavení systému

#### Sekce FILE

- **SAVE:** Uložení aktuální konfigurace do souboru
- **LOAD:** Načtení konfigurace ze souboru
- **DELETE CONFIG:** Smazání konfiguračního souboru

#### Sekce TCP

- **TCP Adresa:** IP adresa vaší FVE (bez http://)
- **TCP Port:** Port pro TCP komunikaci (standardně 502)
- **TCP ID:** ID zařízení pro Modbus komunikaci (standardně 100)
- **CONNECT:** Tlačítko pro připojení k FVE se stavovým indikátorem

#### Sekce Config

- **ESS IP Address:** IP adresa ESS systému
- **ESS IP Address Checking:** Adresa pro kontrolu dostupnosti ESS
- **IP ESS Modbus:** IP adresa pro Modbus komunikaci s ESS
- **IP Esp-01:** IP adresa ESP modulu (pokud je použit)
- **Maximum Grid Point:** Maximální výkon dodávaný do sítě z baterií v W
- **nBalancingReserve:** Hodnota vyvažovací rezervy v W
- **Maximum Grid Feed In:** Nastavení maximálního výkonu pro dodávku do sítě (registr 2706) v W
- **SET GRID FEED IN:** Tlačítko pro nastavení hodnoty v registru 2706
- **GET GRID FEED IN:** Tlačítko pro načtení aktuální hodnoty z registru 2706

#### Sekce VRM

- **Email:** Přihlašovací email do VRM portálu
- **Name your Installation:** Název vaší instalace v systému VRM
- **VRM ID:** Identifikační číslo vaší instalace z URL ve VRM
- **Bearer Token:** Vygenerovaný token pro API přístup
- **SUBMIT:** Tlačítko pro odeslání údajů a vygenerování tokenu

### 7. Doporučené postupy

- Nejprve nakonfigurujte TCP připojení k vaší FVE v kartě CONFIG
- Propojte FLOW s VRM portálem pro rozšířené možnosti monitorování
- Nastavte limitní cenu SPOTu podle aktuálních tržních podmínek
- Uložte konfiguraci pomocí tlačítka SAVE
- Pravidelně sledujte predikce výroby a spotřeby pro optimální nastavení
- Při používání pokročilých funkcí jako Control Mode: ESS / AC Grid ověřte, zda máte potřebné verze firmware

### 8. Řešení problémů

- Nedostupné predikce: Zkontrolujte připojení k VRM portálu
- Chyby připojení: Ověřte správnost IP adresy a portu
- Neaktuální data SPOT: Zkontrolujte připojení k datovému zdroji OTE
- Problémy s řízením přetoků: Ujistěte se, že nemáte aktivní jiné systémy řízení přetoků
- Nefunkční AC Grid Mode: Zkontrolujte verzi firmware v CERBO (min. 3.50) a měničích MultiPlus-II 48 (min. v510)

## Vysvětlení funkce registrů

### Modbus registry použité ve FLOW

- **Registr 2700 – ESS Control Loop Setpoint**
  - Tento registr slouží k nastavení cílového výkonu pro řízení energetického systému (ESS).
  - Určuje, kolik energie se má dodávat nebo odebírat ze sítě.
  - Používá se k řízení toku energie mezi bateriemi, sítí a spotřebou.
  - **Kladná hodnota** → ESS dodává energii do sítě
  - **Záporná hodnota** → ESS odebírá energii ze sítě
  - **Nula (0)** → ESS se snaží dosáhnout rovnováhy

- **Registr 2706 – Maximum System Grid Feed-In**
  - Tento registr definuje **maximální povolený výkon**, který může systém dodávat do sítě.
  - Slouží k zajištění, že systém **nepřekročí limity stanovené distributorem nebo legislativou**.

- **Registr 2707 – Grid Feed-in Enable**
  - Tento registr povoluje nebo zakazuje dodávku energie do sítě z DC zdrojů (fotovoltaických panelů).
  - **0** → Dodávka energie do sítě zakázána
  - **1** → Dodávka energie do sítě povolena
  - Ovládá pouze DC přetoky, ne přetoky z AC zdrojů jako jsou generátory

- **Registr 2708 – AC Feed-in Enable**
  - Podobný registru 2707, ale ovládá dodávku energie do sítě z AC zdrojů.
  - Ve FLOW LINEA není standardně implementován, ale lze ho doplnit

- **Registr 2716 – AC Grid Set Point**
  - Novější alternativa k registru 2700, implementovaná v RAM paměti místo FLASH
  - Je 32-bitový, což poskytuje větší rozsah hodnot
  - Umožňuje přímé řízení výkonu dodávaného do sítě
  - Šetří životnost FLASH paměti díky omezenému počtu zápisů
  - Vyžaduje CERBO firmware min. verze 3.50 a měniče MultiPlus-II 48 s firmware min. v510
  - Má stejnou funkci jako registr 2700, ale s výhodou implementace v RAM

### Jak to funguje dohromady?

1. **ESS nastavuje výkon (Registr 2700 nebo 2716)** → Například +2000 W znamená, že chce posílat 2 kW do sítě.
2. **Kontrola proti max. limitu (Registr 2706)** → Pokud je např. -1500 W, tak se výkon omezí na tuto hodnotu.
3. **Systém zajistí, aby nikdy nepřekročil limit feed-in do sítě.**

Toto funguje, pokud **systém není řízen** nebo je **plná baterie a není odběr elektřiny**.

Pokud ale **FVE řídíte** a chcete zajistit, aby **nepřekročila limit feed-in do sítě**, je třeba **dynamicky nastavovat hodnotu registru 2700/2716** podle registru 2706.

📌 **Při dosažení limitu v registru 2706 může hodnota krátkodobě překmitnout přes nastavený limit feed-in.**

👉 **Proto doporučujeme vždy nastavit hodnotu nižší než maximální povolený výkon definovaný distributorem nebo legislativou.**

### Důležitá poznámka:

- **🔹 FLOW LINEA**
  - Dynamicky řídí registr **2700** nebo **2716** podle aktuální situace a vybraného režimu.
  - **Registr 2706** je nastavitelný **pouze v CONFIG** pomocí tlačítka **SET GRID FEED-IN** nebo přímo v **CERBU**.
  - **Nikde jinde se nenastavuje.** Pokud se vám hodnota registru 2706 mění, pravděpodobně ji mění jiné řízení vaší FVE.

- **🔹 MAX GRID POINT**
  - Slouží **pouze** pro nastavení **maximálního výkonu dodávaného do sítě z baterií**, **ne** z FV panelů!

- **🔹 MAXIMUM GRID FEED-IN**
  - Nastavuje hodnotu **registru 2706**.

![Schéma registrů](https://github.com/user-attachments/assets/3396f3c3-941c-488b-9dd7-ac92a83a57a4)

<a name="nejnovejsi-flow"></a>

## Historie verzí

Důležitá poznámka: Od verze SELECTION_flows_04052025 je nutné mít CERBO aktualizované na minimálně verzi 3.50 a v měničích MultiPlus-II 48 minimální verze FW v510.

### Verze: 📌 SELECTION_flows_26072025.json

#### ✅ **Opravy a vylepšení:**
- Opraveny různé chyby v logice a zpracování dat
- Vylepšena stabilita a výkon systému
- Optimalizace kódu pro lepší čitelnost

#### 🎨 **Vizuální vylepšení:**
- **Config karta:** Parametry jsou nyní seskupeny podle barev pro lepší orientaci
- Přidány barevné oddělovače pro jednotlivé funkční celky
- Zlepšena uživatelská přívětivost rozhraní

#### ⚡ **NOVÁ FUNKCE: Spot-Grid Charging (Chytré nabíjení ze sítě)**

**Umístění:** Config karta → Zelené nastavovací prvky

**Jak to funguje:**
- Systém automaticky najde **nejlevnější souvislý časový úsek** pro nabíjení ze sítě
- Při dosažení optimálního času aktivuje nabíjení s nastavením `-MAX GRID POINT`

**⚠️ DŮLEŽITÉ UPOZORNĚNÍ:**
> **Nastavte například 4000W, ale systém může nabíjet pouze 2500W!**  
> Toto je dáno nastavením v měniči. FLOW Linea **nemůže přenastavit** konfiguraci měniče.  
> Pro změnu maximálního nabíjecího výkonu musíte **upravit nastavení přímo v měničích**.

**Nastavitelné parametry:**
- **⏱️ Délka nabíjení:** Lze nastavit dobu nákupu ze SPOTu (max. 20 hodin)
- **💰 Cenový trigger:** Maximální přijatelná cena za kterou chcete nakupovat
- **📊 Informační displej:** Real-time zobrazení optimálního nabíjecího okna
  - ⚠️ **Poznámka:** Label se aktualizuje pouze při zapnuté funkci Spot-Grid Charging


### Verze: 📌 SELECTION_flows_28062025.json

#### Opravy a vylepšení:
- **Opraveny chyby v časové synchronizaci**
  - Po spuštění FLOW se uskuteční první startovací sekvence v rozmezí cca 20 sekund, takže do minuty a půl je plně FLOW načteno a synchronizováno.
  - Čas se synchronizuje se skutečným časem - tato chyba byla patrná především při řízení SPOTu, kdy FLOW reagoval nesprávně na časové události.

- **Vylepšení konfigurace na kartě Config**
  - Parametry jsou rozděleny do více kategorií pro lepší přehlednost
  - Přidána možnost konfigurace délky peaku pro ranní a večerní prodej baterie (nastavitelné hodnoty od 1 do 4 hodin)
  - Přidáno zobrazení časů peaku, které FLOW LINEA automaticky vypočítá pro aktuální den

#### Nová funkce: Dynamic SOC Reserve 🔋
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

#### Požadavky pro funkci Dynamic SOC Reserve:
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

### Verze: 📌 SELECTION_flows_20052025.json
- Přidána funkce:
    - SOC delta před exportem :
      - Po zapnutí je aktivní ovládací prvek: SOC delta v %, kde nastavíte práh SOC baterie odkdy má posílat přebytky do sítě.
      - Je to dobré k tomu, že máte ráno baterii na třeba 20 SOC a chcete aby se baterka trošku nabila, třeba na SOC 30 a pak zbytek šel do sítě do uvedené doby, třeba
do 11:00 hodin, kdy třeba naskočí bojler.

### Verze: 📌 SELECTION_flows_04052025.json
- Opraveny drobné chyby napříč celého FLOW.
- Přídány funkce: 
  - Control Mode: ESS / AC Grid
    - Funkce pro přepínání mezi dvěma režimy řízení fotovoltaického systému
    - Pozice je indikována zeleným trojúhelníkem
    - Přepíná mezi registry 2700 a 2716
  - NON Battery Priority Mode :
    - Přepínač pro aktivaci režimu, který upřednostňuje použití energie ze sítě nebo FV panelů před energií z baterie
    - Ochrana baterie při nabíjení elektromobilu nebo jiné energeticky náročné spotřeby

### Verze: 📌 SELECTION_flows_14042025.json
- Opraveny chyby v rozdělení hodin.
- Opraveny chyby v parsování SPOTu.
- Opravena chyba s VRM portálem.

### Verze: 📌 SELECTION_flows_05042025.json
- Opravené nalezené chyby.
- Opravena práce s hodinami (CLK).
- Přepracováno flow SPOTu.
- Přidána funkce: Energy Threshold Injector. Funkci nebudu blíže komentovat, slouží pro mé účely. :)

### Verze: 📌 Selection_flows_28032025.json
<details>
<summary>Zobrazit detaily</summary>

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
</details>

### Verze: 📌 Linea_flows_15032025.json
<details>
<summary>Zobrazit detaily</summary>

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
</details>

### Verze: 📌 SELECTION_flows_20022025.json
<details>
<summary>Zobrazit detaily</summary>

- Opraveny drobné překlepy. Odstraněny grafy, které nikdy správně nefungovaly. Kód pro grafy je součástí FLOW, ale je pouze zakomentován. Odstraněn FLOW pro řízení chlazení měničů a FW regulátorů, které jsou ovládány přes Shelly plugin. Od této chvíle budou exportovány pouze relevantní karty, a to s pomocí doplňku: node-red-contrib-flow-manager.
</details>

### Verze: 📌 K_ALL_flows_09022025.json
<details>
<summary>Zobrazit detaily</summary>

- Upravená verze práce s tokenem. Token lze vygenerovat přímo ve VRM a manuálně vložit do souboru, nebo nechat vygenerovat pomocí FLOW. Toto je možnost, pokud z nějakého důvodu nemáte možnost generovat token. To může nastat, pokud nemáte dostatečná práva ve VRM pro integraci tohoto FLOW. Doporučujeme mít práva ADMIN, protože práva jako Technician nebo User nemusí být dostatečná. Ve všech ostatních verzích FLOW se používá bezpečnější BearerToken. Tato verze  (K_ALL_flows_09022025) pravděpodobně nebude dále rozvíjena.
</details>

### Verze 📌 07022025
<details>
<summary>Zobrazit detaily</summary>

- Drobné úpravy kódu.
- Přidány ikony.
- Přidány výpočty úložení elektrické energie do a z baterie.
</details>

### Verze 📌 02112024
<details>
<summary>Zobrazit detaily</summary>

- Byl přidán indikátor na kartě FVE::Real Data - doba provozu, který indikuje, jak dlouho dané FLOW běží. Při jakékoli změně se počitadlo nuluje.
- Flow umí vyhodnotit, kde je spuštěno, a podle toho samo nastaví cestu k úložišti.
</details>

### Verze 📌 14102014
<details>
<summary>Zobrazit detaily</summary>

- Upravena logika u funkcí prodej baterie v ranní i odpolední špičce.
</details>

### Verze 📌 28082024
<details>
<summary>Zobrazit detaily</summary>

- Na kartě config tlačítko CONNECT je signalizováno indikátorem, zda je zařízení připojeno na danou IP adresu. Chyba v samotné knihovně node-red-contrib-modbus stále existuje. Celý fígl spočívá v tom, že flow se pokouší přečíst SN instalace. Pokud neuspěje nebo cca 4 sekundy nepřijde žádná informace o SN čísle, indikátor změní barvu na šedou. Po připojení indikátor změní barvu na zelenou. Komponenta node connection nevysílá žádné zprávy, tudíž jsem zavedl do flow časové razítko a pokud je platné, není dostupné zařízení na dané IP adrese.
- Přidána defaultní konfigurace. Po spuštění flow na kartě config vidíte tlačítka DEFAULT LOAD a DEFAULT SAVE. Nastavte si vše, jak potřebujete, a proveďte uložení. Toto je základní nastavení, abyste nemuseli měnit nic v globální struktuře a při updatu na novou verzi opět vše měnit. Vše děláte jen v UI. Po tomto uložení tlačítka změní názvy na CONFIG LOAD a CONFIG SAVE, zde si nastavte, co potřebujete. Celý smysl toho je, že při restartu node-red se načte defaultní konfigurace. Takže tam doporučuji nastavit IP adresy a login k VRM, včetně základního nastavení přepínačů funkcí.
</details>

### Verze 📌 15092024
<details>
<summary>Zobrazit detaily</summary>

- Přidána funkce GRID CHARGING nabíjení baterie z GRIDu. Nabíjecí proud se nastavuje na kartě CONFIG, položka MAX GRID POINT, a je jedno, zda uvedené číslo je kladné nebo záporné. Patřičné funkce si to upraví dle svého požadavku. Nastavíte, do jakého SOC má nabíjet, a po dosažení se automaticky vypne a dál nepokračuje. Není to funkce na udržení baterie na daném SOC, ale je to spíše míněno pro nouzové nabití baterie pro očekávaný nadcházející výpadek elektřiny. Do konfigurace se ukládá jen nastavená hodnota SOC.
- Přidány diagnostické funkce ohledně vypadávání načítání dat z VRM. Možná bude hlásit chyby uložení souboru.
- Opravena chyba ztráty tokenu pro VRM. Oprava je provedena takto: V případě ztráty tokenu je načtena konfigurace a vezme se jen hodnota tokenu, zbytek nastavení je ignorován.
</details>

### Verze 📌 25082024
<details>
<summary>Zobrazit detaily</summary>

- Opraveny drobnosti v tooltipu.
- Změna cesty ukládání konfiguračního souboru do root/mode_modules.
- Token se ukládá do konfiguračního souboru. Když Node-Red běží v kontejneru, tak se často restartuje, a pak se ztratí připojení na VRM.
- V Inmout boxu pro zadání Tokenu je primitivní test validace tokenu.
</details>

### Verze 📌 14082024
<details>
<summary>Zobrazit detaily</summary>

- Opraveny drobné chyby v CSS profilu.
- Opraveny drobné chyby ve FLOW.
- Možnost konfigurovat konstantu: nBalancingReserve. Hodnota ve watech slouží k přičtení hodnoty po rozdílu mezi celkovou zátěží a aktuální výrobou z FV panelu. Rozdíl se pošle na GRID, aby se zamezilo kolísání nabíjení/vybíjení baterie. Tato konstanta má u mě hodnotu 230W. U jiného systému možná bude třeba upravit.
- Přidán k LABELu "Údaje o instalaci:" aktuální čas a datum.
</details>

### Verze 📌 13082024
<details>
<summary>Zobrazit detaily</summary>

- Konečně je dokončena funkce pro večerní prodej baterie.
- Na kartě CONFIG je nyní možné nastavit i maximální vybíjecí proud ve W, pro ranní a večerní špičku dohromady.
- Drobné opravy, hlavně interpretace hodnot FALSE a TRUE, pomocí dvou negací za sebou (příklad: !!fGetConfigProperty()).
- Na kartě config nastavíte přístup k VRM. Zde zadáte své přihlašovací údaje (ukládá se pouze email, heslo nikoli). Také zadáte název vaší instalace, ke kterému se přidá nějaký náhodný řetězec. Dále je vyžadováno číslo vaší instalace, které najdete v URL vaší VRM instalace. Po připojení se vygeneruje token, který se nikam neukládá, ale existuje v globální proměnné tak dlouho, dokud nezrestartujete Cergo nebo kontejner, kde běží Node-RED. Pak je třeba provést novou žádost o token. Vygenerovaný token pak uvidíte ve vaší instalaci ve VRM v nastavení: "Předvolby/Integrace/Přístupové Tokeny". Na kartě FVE v sloupci Real Data máte informace o vaší instalaci.
- Karta RealTime Power je neustále ve vývoji, sice něco ukazuje, ale zatím se na to nedá spolehnout.
</details>

### Verze 📌 03082024
<details>
<summary>Zobrazit detaily</summary>

- Opravena kritická chyba selhání SPOTU, po odstranění knihovny node-red-contrib-config, flow vyžadovalo větší upravu.
</details>

### Verze 📌 31072024
<details>
<summary>Zobrazit detaily</summary>

- Odstranění zavislosti na knihovně: node-red-contrib-config.
- Odstranění závislosti na knihovně: node-red-contrib-victron-modbus. Tuto knihovnu jsem nikdy nepoužil, a nakonec jsem ji úplně zavrhl.
- FIX: Některé ovládací prvky při nahrání konfigurace řádně nereagovaly na aktuální nastavení.
- Upozornění: Přepínač "Přetoky: Zap / Vyp" není a nebude ukládán do globální struktury, a tudíž nebude uložen s konfigurací, aby se zabránilo nechtěnému zapnutí přetoků. Toto je řízeno přepínačem automaticky spot a limitní cenou (trigger).
</details>

### Verze 📌 29072024
<details>
<summary>Zobrazit detaily</summary>

- Přidána karta Config - z karty zatím funguje nastavení TCP, kde zadáte IP adresu vaší FVE, port a ID, které většinou nebudete měnit. Port je defaultně 502 a ID je defaultně 100. Potom dáte connect. Jelikož je v knihovně modbus chyba, nefunguje signalizace stavu připojení a pořád uvidíte CONNECTING…
- Také je funkční nastavení FILE, kde když máte první instalaci, tak pokud neexistuje na disku (v ROOT adresáři node-red) konfigurační soubor, tak si ho vytvoří. Parametry nastavení si můžete upravit dle libosti a uložit tlačítkem SAVE CONFIG. Tlačítko DELETE CONFIG je dobré, když instalujete novou verzi FLOW, aby se načetly korektně defaultní hodnoty.
- Napříč FLOW byly změněny proměnné ze samostatných definic na globální strukturu, která pak jde uložit na disk.
</details>

![Config panel](https://github.com/user-attachments/assets/ef521fc2-1faa-478d-a702-8a99e7f5978a)

### Verze 📌 20072024
<details>
<summary>Zobrazit detaily</summary>

- FIX CCS profil - Drobný detail, při zavírání karet se sloupce pohybovaly.
- FIX flow chlazení FVE - Přidán node delay 3s pro posun paketu.
</details>

### Verze 📌 19072024
<details>
<summary>Zobrazit detaily</summary>

- FIX funkce CopyOnChange_2707 - Teď se do registru 2707 zapisují a posílají jen změny, nikoliv stejná hodnota.
- FIX function GLOBAL FUNCTION - Funkce sExtractTime a mExtractTime mají stejný časový základ pomocí funkce fSetFixedDate a nemůže se stát, že budou mít rozdílné hodnoty.
- Nejvíce je přepracován flow chlazení FVE:
  - Kde jednak jsou indikátory, zda se ventilátory točí, je to odezva od PLUGINu SHELLY, která potvrdí přijetí příkazu. Není implementováno monitorování odběru ele.i.
  - Zapnutí ventilátoru je okamžité, jakmile dosáhne hodnoty triggeru, ale vypínám je až po přijetí 20x za sebou příkazu STOP. Tím se vyhneme nějakému mraku.
</details>

### Verze 📌 15072024
<details>
<summary>Zobrazit detaily</summary>

- FIX funkce convert signet to unsigned na flow battery control.
- FIX funkce convert unsigned to signet na flow battery control.
- Částečně přepracován flow Spot Excess Control. Část node předělána do function node: Logical write register 2707.
</details>

### Verze 📌 14072024
<details>
<summary>Zobrazit detaily</summary>

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
</details>

## Nainstalované NODE

- node-red - 4.0.3
- node-red-contrib-modbus - 5.42.0
- node-red-dashboard - 3.6.5

---

## ⚠️ Pracujte pečlivě! Dvakrát měř, jednou řež...

---

## Zřeknutí se odpovědnosti

Autor tohoto projektu neposkytuje žádné záruky, výslovné ani implicitní, ohledně správnosti, spolehlivosti, funkčnosti nebo vhodnosti k jakémukoli účelu. Veškeré použití tohoto softwaru, kódu, schémat, návodů, technických řešení, produktů a jakýchkoli dalších poskytnutých materiálů je na vlastní odpovědnost uživatele.

Autor nenese žádnou odpovědnost za jakékoli škody, ztráty, finanční náklady, přímé či nepřímé škody vzniklé v důsledku použití těchto materiálů, a to včetně, ale nejen, ztráty dat, poškození zařízení, výpadků systému, poruchy elektrických či jiných instalací, požárů, ztrát příjmů nebo jiných nepředvídatelných následků.

Uživatel bere na vědomí, že jakékoli úpravy, sestavování, instalace, zapojení či implementace na základě poskytnutých informací provádí výhradně na vlastní riziko. Autor neposkytuje žádné garance funkčnosti, bezpečnosti ani souladu s platnými právními normami a předpisy.

Uživatel se zavazuje, že nevyužije žádné právní kroky vůči autorovi v souvislosti s jakýmikoli škodami nebo jinými nároky vyplývajícími z používání tohoto softwaru, produktů, schémat nebo návodů. Jakékoli právní nároky vůči autorovi jsou tímto výslovně vyloučeny a nevymahatelné, a to i soudní cestou.

Použitím těchto materiálů uživatel potvrzuje svůj souhlas s výše uvedenými podmínkami. Pokud s nimi nesouhlasíte, nepoužívejte tento software, schémata, návody ani jiné poskytnuté materiály.

