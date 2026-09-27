[🇨🇿 Česky](07_NEXTCLOUD_GRIDSIGHT.md) | [🇬🇧 English](../../en/api/07_NEXTCLOUD_GRIDSIGHT.md)

# GridSight v Nextcloudu

[GridSight](https://github.com/hacesoft/GridSight) je funkční aplikace pro monitoring LINEA v Nextcloudu. Provozní data získává přes read-only LINEA API.

## Instalace, nastavení a funkce

Aktuální návod je veden přímo v repozitáři aplikace:

- [Český manuál GridSight](https://github.com/hacesoft/GridSight/blob/main/README_CZ.md) — požadavky, instalace, konfigurace a popis funkcí.
- [Anglický manuál GridSight](https://github.com/hacesoft/GridSight/blob/main/README.md).
- [Repozitář GridSight](https://github.com/hacesoft/GridSight) — zdrojové soubory a dokumentace aplikace.

Při instalaci nebo aktualizaci GridSight postupujte podle tohoto manuálu. Verze Nextcloudu, závislosti a instalační postup patří do dokumentace GridSight; LINEA má samostatný flow a API.

## Rozdělení odpovědností

| Součást | Úloha |
| --- | --- |
| LINEA / Node-RED | Komunikace se zařízeními, výpočty a řízení ESS, bezpečnostní pravidla a aktuální provozní stav. |
| LINEA API | Poskytování aktuálních snapshotů přes HTTP pouze pro čtení. |
| GridSight / Nextcloud | Monitoring a zobrazení dat v Nextcloudu; funkce a jejich nastavení popisuje manuál GridSight. |

Historie, ukládání a agregace na straně klienta nejsou koncovými body LINEA API. Jejich nastavení se řídí dokumentací aplikace. LINEA API samo neobsahuje databázi historie.

## Připojení k LINEA

1. V Node-RED ověřte dostupnost `GET /api/v1/health` a `GET /api/v1/status`; API je součástí LINEA flow.
2. Ověřte síťové spojení ze serveru nebo kontejneru Nextcloudu. Dostupnost pouze z prohlížeče na PC nestačí.
3. V GridSight nastavte připojení podle jeho manuálu. Použijte adresu Node-RED dostupnou z Nextcloudu a zohledněte případný prefix reverse proxy nebo `httpNodeRoot`.
4. Zkontrolujte odpověď API, stáří dat a dostupnost volitelných modulů.

Pro diagnostiku nahraďte `LINEA_HOST:1880` skutečnou adresou:

```bash
curl -i 'http://LINEA_HOST:1880/api/v1/health'
curl -i 'http://LINEA_HOST:1880/api/v1/status'
```

Podrobnosti: [zprovoznění a ochrana API](02_ZPROVOZNENI_A_POUZITI.md), [koncové body](03_KONCOVE_BODY.md) a [datový model](04_DATOVY_MODEL.md).

## Dostupnost a stáří dat

- HTTP `503` při inicializaci znamená, že LINEA ještě nemá připravený hlavní stav. Chybějící měření není nulová spotřeba ani nulový výkon.
- HTTP `200` samo nezaručuje čerstvost dat. Sledujte `system.stale`, časové značky a pravidla [stáří dat](05_KONVENCE_A_STARI_DAT.md).
- Nepřítomný volitelný modul se může projevit jako `available: false`; nemusí znamenat nefunkční připojení celé aplikace.
- Častější dotazování API nezrychlí aktualizaci dat cloudových služeb, například klimatizace Daikin.

## Hranice rozhraní

LINEA API je pouze pro čtení. Připojení GridSight přes toto API nemění nastavení ESS, nezapisuje do Modbus registrů a neobnovuje bateriové čítače. Řídicí a bezpečnostní logika zůstává v Node-RED. Popis GridSight nepřidává do LINEA API nové příkazy ani koncové body.

[← LINEA API](PREHLED.md) · [← Hlavní dokumentace](../README.md)
