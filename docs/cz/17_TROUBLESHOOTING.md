[🇨🇿 **Česky**](17_TROUBLESHOOTING.md) | [🇬🇧 English](../en/17_TROUBLESHOOTING.md)

---

# Troubleshooting

## Grid Point se po přibližně 30 sekundách vrací na 0

Pokud používáte 2716/2717, ověřte periodický heartbeat. Stejný setpoint musí být zapisován opakovaně.

Zkontrolujte, zda na výstupní větvi není:

- RBE;
- deduplikace;
- send-only-on-change;
- příliš dlouhý interval zápisu.

## Registr 2700 se zapisuje neustále

2700 má používat zápis při změně. Ověřte volbu Control Mode a logiku potlačení stejné hodnoty.

## FVE nereaguje na přepínač v UI

Postupujte po datové cestě:

```text
UI
 ↓
config
 ↓
ESS logika
 ↓
nSet_Grid_Point
 ↓
limitace
 ↓
Control Mode
 ↓
Modbus write
```

Ověřte také živě přečtenou hodnotu cílového registru.

## Nula se mění na výchozí hodnotu

Nula je platný výkon/setpoint. Nepoužívejte pro takové hodnoty konstrukci:

```javascript
value || defaultValue
```

Použijte explicitní validaci `undefined`, `null` a `Number.isFinite()`.

## Energetické čítače nesedí

Ověřte:

- jednotku vstupního výkonu;
- čas mezi vzorky;
- časovou značku předchozího vzorku;
- vzorec `P × Δt / 3600`;
- nulování čítače.

## Morning/Evening Peak se spouští ve špatný čas

Ověřte:

- systémové časové pásmo;
- `TZ`;
- denní SPOT data;
- vypočtené peak okno;
- `nSetPeak`.

## Spot-Grid Charging se nespustí

Ověřte:

- `switchSpotGridCharging`;
- dostupnost validních SPOT cen;
- `nCharging_DurationGRID`;
- `nAcceptable_Price_GRID`;
- aktuální čas;
- SOC a další podmínky;
- `nMAX_Grid_Point`.

## Prediction Threshold blokuje strategii

Ověřte platnost predikčních dat a nastavenou hodnotu `sPredictionThresholdKW`.

## Konfigurace se po restartu ztrácí

Ověřte persistentní `/data` Node-RED a práva k zápisu konfiguračních souborů.

## Čas je posunutý

Node-RED musí používat správné lokální časové pásmo. Pro českou instalaci:

```text
TZ=Europe/Prague
```

---

[← Dokumentace LINEA](README.md)
