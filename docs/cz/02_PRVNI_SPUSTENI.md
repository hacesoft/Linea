[🇨🇿 **Česky**](02_PRVNI_SPUSTENI.md) | [🇬🇧 English](../en/02_FIRST_START.md)

---

# První spuštění a bezpečné ověření

## 1. Ověřte komunikaci

Zkontrolujte FV výkon, AC Load, Grid Point, SOC, výkon baterie, stav feed-in a používaná externí data.

## 2. Ověřte znaménka

V Dashboardu LINEA:

```text
kladná hodnota  → odběr ze sítě
záporná hodnota → dodávka do sítě
```

Porovnejte je s GX/VRM.

## 3. Ověřte 2706

Načtěte maximální feed-in a zkontrolujte bezpečnostní rezervu.

## 4. Vyberte Control Mode

```text
OFF → 2700
ON  → 2716/2717
```

Viz [Registry a režimy](13_REGISTRY_A_REZIMY.md).

## 5. Začněte malým Set Pointem

Nejdříve ověřte malou řídicí hodnotu a skutečnou reakci systému.

## 6. Ověřte heartbeat 2716/2717

Stejný setpoint musí odcházet periodicky. Na této větvi nesmí být RBE ani deduplikace.

## 7. Strategie aktivujte jednotlivě

Doporučené pořadí: Set Point → feed-in → Delay Charging → GRID Charging → Morning/Evening Peak → Spot-Grid Charging → Dynamic SOC Reserve.

## 8. Vylučte druhý regulátor

Pokud Grid Point mění jiný systém, určete jediný autoritativní controller.

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)
