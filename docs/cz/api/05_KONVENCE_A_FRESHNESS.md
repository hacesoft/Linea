[🇨🇿 Česky](05_KONVENCE_A_FRESHNESS.md) | [🇬🇧 English](../../en/api/05_CONVENTIONS_AND_FRESHNESS.md)

---

# Konvence, freshness a dostupnost

## Znaménka

### Síť

```text
kladný výkon = import ze sítě
záporný výkon = export do sítě
```

### Baterie

```text
kladný výkon = nabíjení
záporný výkon = vybíjení
```

Jednotka výkonu je W. Konvence jsou publikovány také přímo v API odpovědi.

## Freshness hlavního snapshotu

R2.3.3 považuje hlavní LINEA snapshot za stale přibližně po **30 sekundách**. `system.ageMs` udává stáří zdrojového snapshotu a `system.stale` výsledné vyhodnocení.

## Různé zdroje mají různou rychlost

- hlavní Modbus/energetika: řádově sekundy;
- Shelly: event-driven MQTT;
- UPS: řádově sekundy;
- Daikin: několik minut kvůli cloud API limitům;
- VRM/forecast/weather: pomalejší zdroje.

Jednotlivé pomalejší moduly mohou mít vlastní `updatedAt`, ale obecný per-module freshness kontrakt není součástí schema 3.

## Sleeping senzory

Shelly Smoke může dlouho spát. Vysoké `ageSec` samo o sobě neznamená poruchu a nesmí se na něj mechanicky aplikovat stejný stale limit jako na sekundový Modbus stream.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
