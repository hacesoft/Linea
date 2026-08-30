[🇨🇿 **Česky**](14_SPOT_ENERGY_PREDIKCE.md) | [🇬🇧 English](../en/14_SPOT_ENERGY_PREDICTION.md)

---

# SPOT, Energy a predikce

## SPOT data

SPOT ceny vstupují do několika nezávislých funkcí:

```text
SPOT
 ├─ automatické řízení přetoků
 ├─ Morning Peak
 ├─ Evening Peak
 └─ Spot-Grid Charging
```

## Automatické řízení podle SPOTu

Přepínač:

```javascript
isSpotAutoCtrlEnabled
```

Cenový práh:

```javascript
spotTresholdPrice
```

Aktuální cena je porovnávána s nastaveným limitem a podle výsledku se řídí možnost ekonomického exportu.

Dashboard zobrazuje aktuální cenu, denní minimum/maximum a průběh cen.

## Morning/Evening Peak

Peak algoritmy pracují s denní sadou cen a vybírají cenová okna vhodná pro prodej baterie.

Parametr:

```javascript
nSetPeak
```

určuje maximální délku peak okna.

Morning Peak navíc může být podmíněn predikcí a dynamickou SOC rezervou.

## Spot-Grid Charging

Parametry:

```javascript
switchSpotGridCharging
nCharging_DurationGRID
nAcceptable_Price_GRID
nMAX_Grid_Point
```

Postup:

```text
SPOT ceny
   ↓
validace
   ↓
nejlevnější souvislý blok N hodin
   ↓
aktuální čas uvnitř bloku
   ↓
cena splňuje limit
   ↓
nabíjení ze sítě
```

## Predikce

Predikce výroby a spotřeby je doplňkový rozhodovací vstup.

`Prediction Threshold`:

```javascript
nPredictionThreshold
sPredictionThresholdKW
```

ovlivňuje zejména Morning Peak a Delay Charging.

Pokud je Prediction Threshold aktivní, závislá strategie vyžaduje platná predikční data a splnění nastavené podmínky.

## Časové pásmo

Časové funkce vyžadují správné lokální časové pásmo Node-RED. Pro českou instalaci se typicky používá:

```text
TZ=Europe/Prague
```

To ovlivňuje SPOT hodiny, Morning/Evening Peak, Delay Charging i další časové funkce.

---

[← Dokumentace LINEA](README.md)
