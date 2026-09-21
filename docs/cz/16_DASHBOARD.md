[🇨🇿 **Česky**](16_DASHBOARD.md) | [🇬🇧 English](../en/16_DASHBOARD.md)

---

# Dashboard 2.0

LINEA používá FlowFuse Dashboard 2.0 jako monitorovací a konfigurační rozhraní.

## FVE karta

Zobrazuje zejména:

- Total AC Load a fáze L1/L2/L3;
- Grid Point a fáze;
- FV výkon;
- SOC a stav baterie;
- napětí a proud baterie;
- energetické čítače;
- predikci výroby a spotřeby;
- SPOT cenu;
- stav přetoků;
- stav komunikace.

Znaménková konvence Grid Pointu:

```text
kladná hodnota  = odběr ze sítě
záporná hodnota = dodávka do sítě
```

## ESS ovládání

ESS karta obsahuje přepínače a hodnoty pro jednotlivé strategie. Kompletní popis je v:

[Ovládání ESS](10_OVLADANI_ESS.md) a [Reference nastavení](11_REFERENCE_NASTAVENI.md).

## Control Mode

Dashboard umožňuje přepnout:

```text
ESS / AC Grid
```

Tím se volí řídicí registr 2700 nebo 2716/2717.

## Informace o verzi

Toolbar zobrazuje lokální verzi a stav dostupnosti nové verze. Hlášení nové verze je klikací a otevírá hlavní repozitář LINEA.

## Fixní navigace

Levé navigační menu je fixní a má vlastní vertikální scroll:

```css
.v-navigation-drawer {
    position: fixed !important;
    top: 64px !important;
    bottom: 0 !important;
    height: calc(100vh - 64px) !important;
    overflow-y: auto !important;
}
```

Konkrétní rozmístění skupin a widgetů je uživatelská záležitost a není součástí funkční architektury LINEA.

---

[← Dokumentace LINEA](README.md)
