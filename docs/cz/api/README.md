[🇨🇿 Česky](README.md) | [🇬🇧 English](../../en/api/README.md)

---

# LINEA API

LINEA API je integrační vrstva uvnitř Node-RED, která zpřístupňuje již existující provozní stav LINEA dalším aplikacím. Aktuální produkčně ověřený kontrakt je **LINEA API `1.0-r2.3.3`, schema `3`**.

> **Zásadní pravidlo:** hlavní LINEA API je striktně **READ-ONLY**. Publikuje hotový stav LINEA, neprovádí ESS rozhodování, nepřepočítává řídicí logiku a nezapisuje Modbus registry.

## Obsah

- [Přehled a architektura](01_PREHLED_A_ARCHITEKTURA.md)
- [Instalace a připojení](02_INSTALACE.md)
- [Endpointy](03_ENDPOINTY.md)
- [Datový model schema 3](04_DATOVY_MODEL.md)
- [Konvence, freshness a dostupnost](05_KONVENCE_A_FRESHNESS.md)
- [Příklady odpovědí](06_PRIKLADY.md)
- [Nextcloud, historie a budoucí rozšíření](07_NEXTCLOUD_A_BUDOUCNOST.md)

## Veřejné endpointy

```text
GET /api/v1/health
GET /api/v1/status
```

Řídicí endpointy `/set`, `/control`, `/write` nejsou součástí API a bez konkrétní potřeby se nemají přidávat.

## Rozdělení odpovědností

**LINEA / Node-RED** je autorita pro aktuální stav, Modbus, ESS řízení, bezpečnost a provozní rozhodování. **LINEA API** pouze publikuje výsledek. Budoucí **Nextcloud LINEA Monitor & Analytics** je určen pro historii, databázi, agregace, statistiky, analytiku a vizualizaci.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
