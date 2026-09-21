[🇨🇿 **Česky**](README.md) | [🇬🇧 English](../en/README.md)

---

# ⚡ LINEA / GridSight – dokumentace

LINEA je Node-RED projekt pro **monitorování a řízení fotovoltaické elektrárny Victron a bateriového ESS**. Spojuje aktuální stav FVE, baterie, spotřeby a sítě s řídicími strategiemi, SPOT cenami a predikčními daty.

Tato stránka je hlavní rozcestník české dokumentace.


**Verze dokumentace:** 1.0.0

## 🚀 Jsem tu poprvé

1. **[Hlavní stránka LINEA](../../README_CZ.md)** – úvod, princip projektu a základní informace.
2. **[Instalace](01_INSTALACE.md)** – Node-RED, síť, Modbus a import FLOW.
3. **[První spuštění](02_PRVNI_SPUSTENI.md)** – bezpečné ověření dat, znamének a limitů.
4. **[Konfigurace](15_KONFIGURACE.md)** – nastavení konkrétní FVE.
5. **[Dashboard](16_DASHBOARD.md)** – orientace v uživatelském rozhraní.

## 🎛️ Řízení ESS

- [Ovládání ESS](10_OVLADANI_ESS.md)
- [ESS řídicí logika](09_ESS_RIZENI.md)
- [Reference nastavení](11_REFERENCE_NASTAVENI.md)
- [SPOT, Energy a predikce](14_SPOT_ENERGY_PREDIKCE.md)
- [Registry a režimy](13_REGISTRY_A_REZIMY.md)
- [Modbus](12_MODBUS.md)

## 🔌 LINEA API

**[LINEA API](api/PREHLED.md)** je samostatné **READ-ONLY rozhraní**, které publikuje existující stav Node-RED pro externí klienty. API samo zařízení neřídí ani nemění nastavení LINEA.

Aktuální API: **1.0.0 · schema 1**

➡️ **[Otevřít dokumentaci LINEA API](api/PREHLED.md)**

## 🧠 Architektura a data

- [Architektura](07_ARCHITEKTURA.md)
- [Tok dat](08_TOK_DAT.md)
- [Modbus](12_MODBUS.md)
- [Registry a režimy](13_REGISTRY_A_REZIMY.md)
- [LINEA API](api/PREHLED.md)

## 🧩 Integrace a samostatné moduly

- [Integrace a doplňky](06_INTEGRACE_A_DOPLNKY.md)
- [Shelly a ruční nastavení MQTT](integrace/SHELLY.md)
- [Daikin](integrace/DAIKIN.md)
- [UPS](integrace/UPS.md)
- [Cooling](integrace/COOLING.md)

## 🛠️ Když něco nefunguje

- [FAQ](04_FAQ.md)
- [Troubleshooting](17_TROUBLESHOOTING.md)
- [Závislosti Node-RED](05_ZAVISLOSTI.md)
- [Aktualizace LINEA](03_AKTUALIZACE.md)

## 📚 Kompletní rozcestník

| Oblast | Dokument |
|---|---|
| Úvod | [Hlavní stránka LINEA](../../README_CZ.md) |
| Instalace | [Instalace](01_INSTALACE.md) |
| První spuštění | [První spuštění](02_PRVNI_SPUSTENI.md) |
| Aktualizace | [Aktualizace](03_AKTUALIZACE.md) |
| FAQ | [FAQ](04_FAQ.md) |
| Závislosti | [Node-RED závislosti](05_ZAVISLOSTI.md) |
| Integrace | [Integrace a doplňky](06_INTEGRACE_A_DOPLNKY.md) |
| Architektura | [Architektura](07_ARCHITEKTURA.md) |
| Data | [Tok dat](08_TOK_DAT.md) |
| ESS | [ESS řízení](09_ESS_RIZENI.md) |
| Ovládání | [Ovládání ESS](10_OVLADANI_ESS.md) |
| Nastavení | [Reference nastavení](11_REFERENCE_NASTAVENI.md) |
| Komunikace | [Modbus](12_MODBUS.md) |
| Registry | [Registry a režimy](13_REGISTRY_A_REZIMY.md) |
| Energie | [SPOT, Energy a predikce](14_SPOT_ENERGY_PREDIKCE.md) |
| Konfigurace | [Konfigurace](15_KONFIGURACE.md) |
| UI | [Dashboard](16_DASHBOARD.md) |
| API | **[LINEA API](api/PREHLED.md)** |
| Diagnostika | [Troubleshooting](17_TROUBLESHOOTING.md) |
| Bezpečnost | [Bezpečnost](18_BEZPECNOST.md) |
| Historie | [CHANGELOG](../../CHANGELOG.md) |

## ⚠️ Před aktivací řízení

Před zapnutím automatických strategií ověřte Modbus komunikaci, znaménkovou konvenci, podporu použitých registrů, limity distributora a skutečné chování konkrétní instalace.

➡️ **[Zpět na hlavní stránku LINEA →](../../README_CZ.md)**

## Repozitáře modulů

Úplné návody k instalaci a exporty jednotlivých modulů:

| Modul | Repozitář |
|---|---|
| Shelly | [node-red-shelly](https://github.com/hacesoft/node-red-shelly) |
| Daikin ONECTA | [node-red-daikin](https://github.com/hacesoft/node-red-daikin) |
| Eaton UPS / NUT | [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups) |
| Cooling | [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack) |

Nextcloud monitoring je ve vývoji; odkaz na repozitář a náhled aplikace zatím čekají na doplnění.

[Přehled oprav a rozsah kontroly](../../REVIEW_CZ.md)
