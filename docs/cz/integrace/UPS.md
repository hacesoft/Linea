[🇨🇿 Česky](UPS.md) | 🇬🇧 English *(bude doplněno)*

---

# Samostatný projekt: Eaton UPS pro Node-RED

> Návrh dokumentace budoucího samostatného projektu.

## Cíl

UPS modul nemá být součástí LINEA CORE. Monitoring UPS je obecná Node-RED funkce a může být použit v libovolném projektu.

## Modul musí být soběstačný

Samostatný release má obsahovat:

- zdroj dat z UPS;
- parsování;
- normalizovaný stav;
- výpočet zbývající výdrže;
- vstupní/výstupní napětí;
- frekvenci;
- výkon/zátěž;
- stav baterie;
- historii;
- Dashboard 2.0;
- diagnostiku komunikace.

## Rozhraní vůči LINEA

LINEA může UPS data pouze zobrazovat nebo použít jako doplňkový stav. LINEA CORE nesmí přestat řídit FVE jen proto, že UPS modul není nainstalován.

## README samostatného repozitáře musí popsat

1. podporovaný způsob komunikace;
2. podporované modely;
3. potřebné Node-RED balíčky;
4. konfiguraci adresy/portu/protokolu;
5. význam všech hodnot;
6. polling interval;
7. timeout;
8. chování při odpojení UPS;
9. Dashboard;
10. import/export;
11. troubleshooting.

Před publikací bude tato dokumentace doplněna podle skutečného odděleného UPS flow.

---

[← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
