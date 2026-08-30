[🇨🇿 **Česky**](04_FAQ.md) | [🇬🇧 English](../en/04_FAQ.md)

---

# FAQ

## Mám posunutý čas

Nastavte správné časové pásmo Node-RED, například `TZ=Europe/Prague`, a Node-RED restartujte.

## Nejde uložit konfigurace

Ověřte persistentní `/data` a práva k zápisu. V Dockeru musí být datový adresář připojen jako read/write.

## Jak LINEA nainstaluji?

Viz [Instalace](01_INSTALACE.md).

## Jak LINEA aktualizuji?

Viz [Aktualizace](03_AKTUALIZACE.md).

## Nemám povolené přetoky

Prodejní a feed-in strategie nejsou použitelné; monitorovací části mohou fungovat dál.

## Přetoky řídí jiné řešení

Nenechávejte dva systémy současně měnit Grid Point.

## Proč se 2716/2717 zapisuje periodicky?

Jde o externí provozní setpoint s heartbeat/fail-safe chováním. Viz [Modbus](12_MODBUS.md).

## Proč se 2700 nezapisuje periodicky?

Jde o nevolatilní parametr; LINEA používá zápis při změně.

## 2716/2717 se po chvíli vrátí na 0

Zkontrolujte periodický refresh, RBE, deduplikaci a interval zápisu.

## GRID Charging má nižší výkon než MAX Grid Point

`nMAX_Grid_Point` je požadavek strategie. Skutečný výkon mohou omezit měniče, BMS nebo další nastavení.

## Co je rozdíl MAX Grid Point a Maximum Grid Feed-In?

MAX Grid Point používají vybrané strategie. Maximum Grid Feed-In je limit registru 2706.

## Musím používat VRM?

Ne všechny základní Modbus funkce VRM vyžadují. Funkce používající cloudová data nebo metadata instalace jej mohou potřebovat.

## Musím instalovat Daikin, UPS nebo Cooling?

Ne. Jsou volitelné a LINEA CORE na nich nemá povinnou závislost.

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)
