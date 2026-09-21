[🇨🇿 **Česky**](README_CZ.md) | [🇬🇧 English](README.md)

---

# ⚡ LINEA

<img width="2043" height="1085" alt="image" src="https://github.com/user-attachments/assets/699968f0-5245-4c0f-8270-250f52725571" />


LINEA je projekt v Node-RED pro monitorování a řízení fotovoltaických instalací Victron a bateriových ESS. Nad standardní Victron ESS přidává externí rozhodovací a řídicí vrstvu a spojuje aktuální data z FVE, baterie, spotřeby a sítě se SPOT cenami a predikčními daty.

## Co LINEA dělá

LINEA vyhodnocuje aktuální energetický stav a podle aktivních strategií vypočítává požadovaný Grid Point. Lze ji použít pro řízení přetoků, strategie nabíjení a vybíjení baterie, provoz podle ceny elektřiny a řízení s využitím predikce.

Victron systém, firmware měničů, BMS a nastavené hardwarové limity zůstávají poslední bezpečnostní vrstvou.

## Hlavní funkce

- automatické řízení podle SPOT ceny elektřiny;
- Delay Charging;
- Morning Peak a Evening Peak;
- Dynamic SOC Reserve;
- GRID Charging a Spot-Grid Charging;
- Non Battery Priority;
- řízení Grid Point;
- Modbus komunikace přes registr 2700 nebo 2716/2717;
- zpracování energie a predikce;
- read-only [LINEA API](docs/cz/api/PREHLED.md) pro externí monitorovací klienty.

## Základní princip

FV výroba pokrývá spotřebu objektu. Podle strategie a limitů může přebytek nabíjet baterii nebo odcházet do sítě; při nedostatku se využívá baterie nebo síť.

LINEA podle aktivní strategie a stavu systému rozhoduje, jak má být dostupná energie využita.

## Instalace a první spuštění

Pokračujte kapitolou [Instalace](docs/cz/01_INSTALACE.md) a poté [První spuštění](docs/cz/02_PRVNI_SPUSTENI.md). Instalaci nastavte podle [Konfigurace](docs/cz/15_KONFIGURACE.md) a uživatelské rozhraní ověřte podle kapitoly [Dashboard](docs/cz/16_DASHBOARD.md).

Před aktivací automatického řízení ověřte Modbus komunikaci, znaménkovou konvenci, podporované registry a skutečné chování konkrétní instalace.

## Řízení ESS

Uživatelské přepínače a strategie jsou popsány v [Ovládání ESS](docs/cz/10_OVLADANI_ESS.md). Vnitřní rozhodovací logiku popisuje [ESS řídicí logika](docs/cz/09_ESS_RIZENI.md).

Podrobnosti komunikace jsou v kapitolách [Modbus](docs/cz/12_MODBUS.md) a [Registry a režimy](docs/cz/13_REGISTRY_A_REZIMY.md).

## LINEA API

[LINEA API](docs/cz/api/PREHLED.md) je samostatné **READ-ONLY** rozhraní pro externí monitorovací aplikace. Publikuje stav, který již existuje v Node-RED, a neposkytuje řídicí endpointy.

Aktuální API: **1.0.0 · schema 1**

## Modulární architektura

LINEA CORE zůstává zaměřena na řízení FVE/ESS. Volitelné integrace a samostatné moduly jsou popsány v [Integracích a doplňcích](docs/cz/06_INTEGRACE_A_DOPLNKY.md).

## Dokumentace

Kompletní česká dokumentace je dostupná v [rozcestníku dokumentace](docs/cz/README.md).

Pro diagnostiku použijte [FAQ](docs/cz/04_FAQ.md) a [Troubleshooting](docs/cz/17_TROUBLESHOOTING.md). Před uvedením řízení do provozu si přečtěte [Bezpečnost](docs/cz/18_BEZPECNOST.md).

## Repozitáře modulů

Úplné návody k instalaci a exporty jednotlivých modulů:

| Modul | Repozitář |
|---|---|
| Shelly | [node-red-shelly](https://github.com/hacesoft/node-red-shelly) |
| Daikin ONECTA | [node-red-daikin](https://github.com/hacesoft/node-red-daikin) |
| Eaton UPS / NUT | [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups) |
| Cooling | [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack) |

Nextcloud monitoring je ve vývoji; odkaz na repozitář a náhled aplikace zatím čekají na doplnění.

[Přehled oprav a rozsah kontroly](REVIEW_CZ.md)
