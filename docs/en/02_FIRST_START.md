[Česky](../cz/02_PRVNI_SPUSTENI.md) | [English](02_FIRST_START.md)

# First start

1. Compare PV power, house load, grid flow, battery SOC, current and voltage against GX/VRM.
2. Confirm signs: positive grid power means import, negative means export; positive battery power means charging.
3. Check register 2706, the intended export limit and a suitable operating margin. Verify finite limits and special register values before relying on the writer.
4. Choose the intended [control mode](13_REGISTERS_AND_MODES.md): OFF selects 2700; ON selects 2716/2717. Neither position is a master disable switch.
5. Test a small setpoint and verify both register readback and physical system response.
6. For 2716/2717, verify repeated writes even when the value is unchanged. Confirm timeout behavior on your firmware; no RBE/deduplication should suppress these refreshes.
7. Enable strategies individually and observe the result. Check prediction-loss and missing-price behavior described in [ESS control](09_ESS_CONTROL.md).
8. Ensure only one controller writes the grid setpoint. Save configuration and verify it reloads correctly.

Fresh API responses alone do not prove fresh Modbus measurements or successful writes.

[← Documentation](README.md)
