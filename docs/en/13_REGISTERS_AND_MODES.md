[Česky](../cz/13_REGISTRY_A_REZIMY.md) | [English](13_REGISTERS_AND_MODES.md)

# Registers and modes

| Register | Purpose in LINEA |
|---|---|
| 2700 | ESS grid setpoint, INT16 branch; unchanged values suppressed. |
| 2706 | Maximum system grid feed-in, read for output limiting. |
| 2707 | DC feed-in enable. |
| 2708 | AC feed-in enable; not normally controlled by the standard branch. |
| 2716/2717 | External AC grid setpoint, two words for INT32; repeated refresh. |

`nControl_Mode_ESS_AC_Grid` selects the output: false → 2700, true → 2716/2717. It is a mode selector, not a disable switch. Confirm support, units, special values and timeout behavior for your GX/device firmware before enabling writes. See [Modbus limitations](12_MODBUS.md).

The export limit and a strategy's requested power are separate. Neither a configured number nor a successfully sent command proves the measured export stayed within the intended limit.

[← Documentation](README.md)
