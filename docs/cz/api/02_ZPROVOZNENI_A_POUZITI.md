[🇨🇿 **Česky**](02_ZPROVOZNENI_A_POUZITI.md) | [🇬🇧 English](../../en/api/02_INSTALLATION.md)

# 2. Zprovoznění a použití LINEA API

## 2.1 API se samostatně neinstaluje

LINEA API je od verze LINEA FLOW `01092026` integrováno přímo do LINEA FLOW. Samostatný instalační balíček API neexistuje.

Ve starších verzích LINEA FLOW API není k dispozici a z technických důvodů do nich nebude zpětně doplňováno.

## 2.2 Předpoklady

Pro použití API musí být spuštěna a funkční LINEA FLOW verze `01092026` nebo novější a klient musí mít síťový přístup k Node-RED, ve kterém LINEA běží.

## 2.3 Ověření dostupnosti

Základní kontrolu dostupnosti provádí:

```text
GET /api/v1/health
```

Aktuální monitorovací data poskytuje:

```text
GET /api/v1/status
```

API je pouze pro čtení. Prostřednictvím těchto koncových bodů se nemění nastavení LINEA, stav zařízení ani řídicí parametry ESS.

## 2.4 Co má klient kontrolovat

Klient by neměl posuzovat platnost dat pouze podle HTTP odpovědi. Pro vyhodnocení aktuálnosti má používat také položky `system.timestamp`, `system.sourceTimestamp`, `system.ageMs` a `system.stale`.

Jednotky numerických hodnot jsou uvedeny v objektu `units`. Význam, datový typ, rozlišení a omezení jednotlivých položek popisuje [Datový model](04_DATOVY_MODEL.md).

## 2.5 Další informace

- [Koncové body](03_KONCOVE_BODY.md)
- [Datový model](04_DATOVY_MODEL.md)
- [Konvence a stáří dat](05_KONVENCE_A_STARI_DAT.md)
- [Příklady](06_PRIKLADY.md)
