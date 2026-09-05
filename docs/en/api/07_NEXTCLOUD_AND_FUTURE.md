[🇨🇿 Česky](../../cz/api/07_NEXTCLOUD_A_BUDOUCNOST.md) | [🇬🇧 English](07_NEXTCLOUD_AND_FUTURE.md)

---

# Nextcloud, history and future work

## Nextcloud LINEA Monitor & Analytics

The planned application is a read-only monitoring and analytics layer: `LIVE` displays `/api/v1/status`; `HISTORY` uses a Nextcloud background collector and database; `ANALYTICS` provides charts, statistics, economics and correlations. The browser must not be responsible for long-term collection.

A working retention concept is high-resolution short history, 15-minute aggregates for longer history, hourly aggregates after roughly a week, and daily long-term aggregates. Analog values may store avg/min/max/sample count, power is integrated over time to energy, and states use transitions/duration/events. Final retention policy will be decided during database implementation.

## Future battery-counter restore

`nBatteryALL_input_Wh` and `nBatteryALL_output_Wh` reset after a Node-RED restart. A future Nextcloud database service is intended to allow a one-time restore of the last stored values after a fresh LINEA start. Restore must be guarded by an internal flag so it cannot occur repeatedly during normal operation. This is **not part of API 1.0.0**.

## What not to change now

Do not create another API revision only for cosmetic changes. Long-term history, database aggregation, battery-counter restore, generic per-module freshness, economic statistics, event timeline and correlations remain deferred until the client implementation creates a concrete need.

## Possible future Shelly control

The main LINEA API remains read-only. If very limited control of selected Shelly devices is implemented later, it must be a **separate Control API** with an explicit allowlist, not a universal Shelly-control endpoint. ESS, HDO, battery, inverters and safety/control elements must not be exposed. The resulting state should be verified through read-only `/api/v1/status`.

---

[← LINEA API](README.md) · [← Main documentation](../README.md)

## Change-based storage for slowly changing state
UPS and similar state blocks do not need duplicate rows every few seconds. When unchanged, Nextcloud can extend the previous record's `valid_to` and create a new row only on change. Historical state is reconstructed from `valid_from`–`valid_to`; events and fast energy measurements may use separate retention policies.
