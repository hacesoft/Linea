[🇨🇿 **Česky**](08_TOK_DAT.md) | [🇬🇧 English](../en/08_DATA_FLOW.md)

---

# Tok dat v LINEA

## 1. Získání dat

LINEA periodicky získává provozní hodnoty z FVE, ESS a dalších datových zdrojů. Modbus část čte fyzické veličiny a provozní registry, VRM doplňuje cloudová data a další větve poskytují SPOT ceny, predikce a MQTT data.

## 2. Modbus snapshot

Hlavní Modbus snapshot je sestaven z **13 vstupních hodnot**. Join čeká na kompletní sadu, aby další zpracování dostalo konzistentní stav měření.

Snapshot odděluje asynchronní příchod jednotlivých Modbus odpovědí od řídicí logiky. Algoritmus tak pracuje se sestavenou sadou hodnot místo samostatných zpráv přicházejících v různých okamžicích.

## 3. Validace hodnot

U číselných veličin se rozlišuje platná nula od neplatné hodnoty.

```text
0         → platná hodnota
undefined → chybějící hodnota
null      → chybějící hodnota
NaN       → neplatné číslo
```

To je důležité zejména pro výkony a Grid Point, kde je 0 W běžný provozní stav.

## 4. Energetické čítače

Energie se integruje z okamžitého výkonu a skutečně uplynulého času mezi vzorky:

```text
ΔE [Wh] = P [W] × Δt [s] / 3600
```

Pro každý nový vzorek se určí čas od předchozího měření a vypočtený přírůstek se přičte do příslušného čítače.

Díky tomu výpočet nezávisí na přesně konstantním intervalu příchodu zpráv.

## 5. Vstupy ESS algoritmu

Do rozhodování vstupují zejména:

- aktuální FV výroba;
- AC Load;
- Grid Point;
- SOC a výkon baterie;
- stav přetoků;
- SPOT cena;
- predikce výroby a spotřeby;
- čas;
- uživatelské přepínače a limity.

## 6. Výpočet přebytku FV

Pro některé strategie se používá dostupný přebytek:

```text
Zbytek FV = FV výkon - (AC Load + Balancing Reserve)
```

`Balancing Reserve` vytváří regulační rezervu a omezuje kmitání kolem cílového Grid Pointu.

## 7. Strategie

Podle konfigurace mohou do výsledku zasáhnout například:

- pevný Set Point;
- Energy Threshold Injector;
- Non Battery Priority;
- Delay Charging;
- Morning Peak;
- Evening Peak;
- GRID Charging;
- Spot-Grid Charging;
- Prediction Threshold;
- SOC podmínky.

## 8. Výsledný Grid Point

Výstupem rozhodovací vrstvy je požadovaný Grid Point. Před zápisem je zkontrolován datový typ, rozsah a limit maximálního feed-in.

## 9. Modbus zápis

Cílový registr určuje `Control Mode: ESS / AC Grid`.

```text
OFF → 2700, INT16, zápis při změně
ON  → 2716/2717, INT32, periodický zápis
```

Periodický zápis 2716/2717 současně plní funkci heartbeat externího řízení.

---

[← Dokumentace LINEA](README.md)
## Výstup do LINEA API

LINEA API odebírá již připravené globals/snapshoty. Nesmí vytvářet paralelní ESS výpočet. Dlouhodobá historie a agregace patří do klientské databázové vrstvy, zejména plánovaného Nextcloud LINEA Monitor & Analytics. Viz [LINEA API](api/README.md).
