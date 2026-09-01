[🇨🇿 **Česky**](README.md) \| [🇬🇧 English](../../en/api/README.md)

# LINEA API

LINEA API je rozhraní **pouze pro čtení**. Zpřístupňuje aktuální
provozní stav LINEA dalším aplikacím. Samo nerozhoduje o řízení ESS a
nezapisuje do Modbus registrů.

Aktuální verze rozhraní je **`1.0-r2.3.3`**, datové schema **`3`**.

## Endpointy

``` text
GET /api/v1/health
GET /api/v1/status
```

## Dokumentace

1.  [Přehled a architektura](01_PREHLED_A_ARCHITEKTURA.md)
2.  [Instalace a připojení](02_INSTALACE.md)
3.  [Endpointy](03_ENDPOINTY.md)
4.  [Datový model -- vysvětlení každé položky](04_DATOVY_MODEL.md)
5.  [Konvence, stáří a dostupnost dat](05_KONVENCE_A_FRESHNESS.md)
6.  [Kompletní příklad JSON odpovědi](06_PRIKLADY.md)
7.  [Nextcloud, historie a budoucí
    rozšíření](07_NEXTCLOUD_A_BUDOUCNOST.md)

## Rozdělení odpovědností

**LINEA / Node-RED** řídí ESS, komunikuje přes Modbus a vytváří aktuální
provozní stav. **LINEA API** tento stav pouze zveřejňuje. Připravovaná
**Nextcloud aplikace** bude data číst, ukládat do databáze a vytvářet
historii, agregace, grafy a analytiku.

Řídicí endpointy typu `/set`, `/control` nebo `/write` nejsou součástí
tohoto API.

[← Hlavní dokumentace](../README.md)
