[Česky](../cz/18_BEZPECNOST.md) | [English](18_SAFETY.md)

# Safety and responsibility

LINEA actively controls an energy system. Wrong registers, signs, types or limits can cause unintended energy flow. Verify hardware, firmware, configuration and the permitted operating limits before enabling control. Use only one authoritative grid-setpoint controller.

The software is supplied without a guarantee of correctness, reliability or suitability for a particular installation. The operator is responsible for checking actual behavior, hardware protections, BMS limits and applicable installation requirements.

The mode selector is not a master stop. Missing prediction data bypass the reference prediction filter, and missing prices can trigger a fallback block selection. Read the [ESS behavior](09_ESS_CONTROL.md) and [Modbus limitations](12_MODBUS.md). Do not rely on an API freshness flag as a safety interlock.

Protect Node-RED editor, Dashboard and HTTP API access appropriately. Keep credentials out of public exports. See [API access](api/02_INSTALLATION.md).

[← Documentation](README.md)
