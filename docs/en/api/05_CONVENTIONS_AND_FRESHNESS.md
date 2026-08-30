[🇨🇿 Česky](../../cz/api/05_KONVENCE_A_FRESHNESS.md) | [🇬🇧 English](05_CONVENTIONS_AND_FRESHNESS.md)

---

# Conventions, freshness and availability

## Sign conventions

Grid: positive power = import, negative power = export. Battery: positive power = charging, negative power = discharging. Power is in W. These conventions are also published in the API response.

## Main snapshot freshness

R2.3.3 marks the main LINEA snapshot stale after approximately **30 seconds**. `system.ageMs` is source age and `system.stale` is the evaluated result.

Source update rates differ: main Modbus/energy data are on the order of seconds; Shelly is event-driven MQTT; UPS is seconds; Daikin is several minutes due to cloud API limits; VRM/forecast/weather are slower. Some slower modules have their own `updatedAt`, but a generic per-module freshness contract is not part of schema 3.

Shelly Smoke devices may sleep for long periods. A high `ageSec` alone is not a fault and must not be evaluated with the same stale threshold as a second-level Modbus stream.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)
