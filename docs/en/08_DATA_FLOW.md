[Česky](../cz/08_TOK_DAT.md) | [English](08_DATA_FLOW.md)

# Data flow

The reference Join node `AC CON.` builds an object keyed by `msg.topic`, with count **24**, timeout **1 second**, and `accumulate: false`. It can emit an incomplete set after timeout; this is not an atomic measurement of every register.

AC LOAD reads the assembled payload and cached values, computes strategy results and stores `lineaDecisionState`. Some inputs use `parseFloat(value) || cachedValue`, which replaces valid zero as well as invalid numbers. `fCheckValue` filters zero for grid/PV values until ten consecutive zero samples. A fresh decision timestamp can therefore contain older measurements.

The physical energy formula is `ΔE [Wh] = P [W] × Δt [s] / 3600`. The reference `Battery W counter` adds `P / 3600` per call, assuming one-second calls; it does not measure actual elapsed time. Gaps or timing changes affect accuracy. Cumulative counters are not calendar-day totals and restart behavior depends on context persistence/reset.

Strategies use PV power, house load, battery state, feed-in status, SPOT, prediction and configured time windows. A commonly used surplus is `PV − (house load + balancing reserve)`. The resulting grid request passes to the output writer, which applies its feed-in calculation and register selection. See [writer limitations](12_MODBUS.md).

The [API](api/README.md) reads the resulting snapshot and independent module states; it does not synchronize their physical measurements.

[← Documentation](README.md)
