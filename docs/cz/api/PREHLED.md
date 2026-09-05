[🇨🇿 **Česky**](PREHLED.md) | [🇬🇧 English](../../en/api/README.md)

# LINEA API 1.0.0

LINEA API je rozhraní pouze pro čtení určené pro předávání provozních a monitorovacích dat LINEA externím aplikacím.

## Dostupnost API

**LINEA API je integrováno přímo do LINEA FLOW od verze `01092026`.**

Ve starších verzích LINEA FLOW API není k dispozici a z technických důvodů do nich nebude zpětně doplňováno. Pro použití LINEA API je proto nutné používat LINEA FLOW verze `01092026` nebo novější.

LINEA API není samostatný modul ani samostatná aplikace a **neinstaluje se odděleně**. Je nedílnou součástí LINEA FLOW.

## Verze

Aktuální veřejná verze rozhraní je **LINEA API `1.0.0`**.

Datové schéma této verze je **`schema: 1`**. Číslo schématu označuje verzi struktury dat vracených API.


## Dokumentace

1. [Přehled a architektura](01_PREHLED_A_ARCHITEKTURA.md)
2. [Zprovoznění a použití](02_ZPROVOZNENI_A_POUZITI.md)
3. [Koncové body](03_KONCOVE_BODY.md)
4. [Datový model](04_DATOVY_MODEL.md)
5. [Konvence a stáří dat](05_KONVENCE_A_STARI_DAT.md)
6. [Příklady](06_PRIKLADY.md)
7. [Nextcloud a budoucí rozšíření](07_NEXTCLOUD_A_BUDOUCNOST.md)

## Strojově čitelný popis

Soubor [OpenAPI](openapi.yaml) obsahuje strojově čitelný kontrakt LINEA API. OpenAPI je název technického standardu, proto název souboru `openapi.yaml` zůstává zachován.

Běžná odpověď `/api/v1/status` obsahuje data a kompaktní mapu `units`. Dlouhé popisy, datové typy, rozlišení a omezení jsou vedeny v [Datovém modelu](04_DATOVY_MODEL.md) a v OpenAPI specifikaci.
