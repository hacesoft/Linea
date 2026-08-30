[🇨🇿 Česky](COOLING.md) | 🇬🇧 English *(bude doplněno)*

---

# Samostatný projekt: FVE Cooling

Chlazení je samostatná funkce a nemá být pevně svázána s LINEA CORE.

## Princip

Projekt má přijímat definované teplotní/stavové vstupy a podle vlastní logiky řídit externí ventilátory/chlazení.

## Opravy z auditu


- přenos `_general_stop`;
- okamžitý General STOP;
- oddělení požadovaného a potvrzeného stavu ventilátoru;
- retry při neúspěšném příkazu;
- hysteréze běžného řízení.

## General STOP

General STOP je bezpečnostní požadavek a nemá čekat na standardní hysterézi.

```text
GENERAL STOP
    │
    └──► okamžitý požadavek OFF
```

## Samostatnost

Cooling projekt musí dokumentovat své vstupy tak, aby zdrojem teplot mohl být:

- LINEA;
- jiný Node-RED flow;
- MQTT;
- Modbus;
- jiný senzorový systém.

LINEA má v dokumentaci pouze odkazovat na samostatný repozitář chlazení.

---

[← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
