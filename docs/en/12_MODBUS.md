[Česky](../cz/12_MODBUS.md) | [English](12_MODBUS.md)

# Modbus communication

Modbus TCP reads telemetry and writes control registers. Use the appropriate service Unit IDs, host and port for your installation.

| Control Mode | Target | Reference writer behavior |
|---|---|---|
| OFF | 2700, INT16 | Send when value or output target changes. |
| ON | 2716/2717, INT32 | Send on every incoming control message, including unchanged values. |

The 32-bit branch writes `[highWord, lowWord]` starting at 2716. Its refresh depends on incoming messages; the writer does not create an independent timer. Do not add RBE/deduplication to this branch. Confirm timeout and fallback behavior on your actual firmware; do not treat a presumed return to 0 W as an independent safety guarantee.

`sendValueWithinLimits` reads input with `parseFloat(...) || 0`, limits negative requests against `-Math.abs(2706)` and converts the result. It does not fully validate finite values or INT16/INT32 bounds. The unlimited feed-in sentinel `-1` has no dedicated handling in that function. Use a verified finite limit and independently compare register readback with physical behavior.

2707 controls DC feed-in; the standard LINEA control does not normally use 2708 for AC feed-in. `nMAX_Grid_Point` is a strategy demand and is not the same as register 2706. See [registers](13_REGISTERS_AND_MODES.md).

[← Documentation](README.md)
