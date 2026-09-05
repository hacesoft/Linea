[🇨🇿 Česky](README_CZ.md) | [🇬🇧 **English**](README.md)

---

# ⚡ LINEA
<img width="2043" height="1085" alt="image" src="https://github.com/user-attachments/assets/2b4bca32-302c-4d01-aefe-9fe53a0346dc" />


LINEA is a Node-RED project for monitoring and controlling Victron photovoltaic and battery ESS installations. It adds an external decision and control layer above the standard Victron ESS and combines live PV, battery, load and grid data with SPOT prices and prediction data.

## What LINEA does

LINEA evaluates the current energy state and, according to the enabled strategies, calculates the requested Grid Point. It can be used for feed-in management, battery charging and discharging strategies, price-based operation and prediction-assisted control.

The Victron system, inverter firmware, BMS and configured hardware limits remain the final safety layer.

## Main functions

- automatic control according to SPOT electricity price;
- Delay Charging;
- Morning and Evening Peak strategies;
- Dynamic SOC Reserve;
- GRID Charging and Spot-Grid Charging;
- Non Battery Priority;
- Grid Point control;
- Modbus communication using register 2700 or 2716/2717;
- energy and prediction processing;
- read-only [LINEA API](docs/en/api/README.md) for external monitoring clients.

## Basic principle

```text
PV production
   │
   ├─► site consumption
   │
   └─► available surplus
           │
           ├─► battery
           └─► grid
```

LINEA decides how the available energy should be handled according to the active strategy and system state.

## Installation and first start

Continue with [Installation](docs/en/01_INSTALLATION.md), then [First Start](docs/en/02_FIRST_START.md). Configure the installation using [Configuration](docs/en/15_CONFIGURATION.md) and verify the user interface in [Dashboard](docs/en/16_DASHBOARD.md).

Before enabling automatic control, verify Modbus communication, sign conventions, supported registers and the actual behaviour of the installation.

## ESS control

The user-facing switches and strategies are described in [ESS Controls](docs/en/10_ESS_CONTROLS.md). The internal decision process is described in [ESS Control Logic](docs/en/09_ESS_CONTROL.md).

For communication details see [Modbus](docs/en/12_MODBUS.md) and [Registers and Modes](docs/en/13_REGISTERS_AND_MODES.md).

## LINEA API

The [LINEA API](docs/en/api/README.md) is a separate READ-ONLY interface for external monitoring applications. It publishes state already available in Node-RED and does not provide control endpoints.

Current API: **1.0.0 · schema 1**

## Modular architecture

LINEA CORE remains focused on FVE/ESS control. Optional integrations and independent modules are described in [Integrations and Tools](docs/en/06_INTEGRATIONS_AND_TOOLS.md).

## Documentation

The complete English documentation is available in the [documentation index](docs/en/README.md).

For diagnostics use [FAQ](docs/en/04_FAQ.md) and [Troubleshooting](docs/en/17_TROUBLESHOOTING.md). Read [Safety](docs/en/18_SAFETY.md) before commissioning the control system.
