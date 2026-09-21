[Česky](../cz/17_TROUBLESHOOTING.md) | [English](17_TROUBLESHOOTING.md)

# Troubleshooting

| Symptom | Checks |
|---|---|
| 2716/2717 setpoint returns to zero | Incoming control messages, write frequency, RBE/deduplication and actual firmware timeout. |
| 2700 repeats constantly | Mode selection and send-on-change context; changing requests still cause writes. |
| UI changes but PV does not respond | Follow config → AC LOAD → requested setpoint → limiter → selected write branch → register readback. |
| Zero is replaced or delayed | Reference inputs use cached fallbacks and zero filters; distinguish implementation behavior from measurement. |
| Energy counters drift | The reference battery counter assumes one-second calls (`P/3600`); check clock and gaps, units, reset and context persistence. |
| Peak window is wrong | Timezone, source date, price-array length and `nSetPeak`. |
| Charging runs with missing prices | The block search can select first-hour fallback. Disable the strategy until source data are valid. |
| Missing forecast does not block sales | Invalid prediction bypasses the filter in the reference flow. |
| Configuration disappears | Writable persistent paths and actual save/load wiring. |
| Shelly cannot connect | Edit MQTT broker host/port/auth/TLS in the flow; device IP is a separate setting. |
| API 503 | Wait for a main decision snapshot; check AC LOAD and HTTP wiring. |
| API 200 but old values | Inspect stale/module timestamps and upstream Modbus/cached source state. |

Do not expose tokens in debug output or public reports. For module-specific faults use the [module manuals](06_INTEGRATIONS_AND_TOOLS.md).

[← Documentation](README.md)
