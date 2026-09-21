[🇨🇿 Česky](CHANGELOG_CZ.md) | [🇬🇧 **English**](CHANGELOG.md)

## Documentation review — 2026-09-21

- Added module repository links, Shelly MQTT setup and English chapters.
- Aligned API examples/OpenAPI with supplied flow: schema 1, 200/503 envelopes, 5/30-second freshness thresholds.
- Corrected versioning, Join, configuration loading, prediction bypass, SPOT fallback and energy-counter descriptions.
- Reference-code limitations and verification scope: [REVIEW_EN.md](REVIEW_EN.md).
- Documentation only; no control-flow changes. API remains 1.0.0/schema 1.

## LINEA API 1.0.0 — 2026-09-02

First public LINEA API release. The API is integrated directly into LINEA FLOW starting with version `01092026`, is read-only, uses data schema `1`, exposes explicit units and includes an OpenAPI contract.



## Documentation update — 2026-09-01
- UPS/NUT aligned with active module: central config, 5 s query, 15 s watchdog, test and events.
- Daikin current OIDC endpoints, initial OAuth authorization and rotating refresh token.
- Central `global.config` and Modbus/MQTT configuration boundary.
- Change-based UPS storage for future Nextcloud database.
- Disabled development branches are not treated as supported implementations.

## Documentation 1.0.0 — 30 August 2026

First public release of the reorganized LINEA / GridSight documentation.

- complete Czech and English documentation;
- `README_CZ.md` and `README.md` are the project introduction; there is no separate Getting Started chapter;
- consistently numbered main documentation chapters;
- CZ ↔ EN language switching;
- separate READ-ONLY LINEA API documentation;
- GitHub-compatible relative links.

> Documentation versioning is independent of LINEA FLOW and LINEA API versioning.

---

## LINEA FLOW History

**Important:** Since `SELECTION_flows_04052025`, CERBO must run at least version **3.50** and MultiPlus-II 48 inverters must run at least firmware **v510**.

## SELECTION_flows_27072025.json

#### Fixes and improvements
- Fixed various logic and data-processing issues.
- Improved system stability and performance.
- Optimized code for better readability.

#### Visual improvements
- Config-card parameters are grouped by color for easier orientation.
- Added colored separators for functional groups.
- Improved usability of the interface.

#### New feature: Spot-Grid Charging
The system finds the cheapest continuous charging window and, when the optimal period starts, activates grid charging using `-MAX GRID POINT`.

> Setting, for example, 4000 W does not mean the inverter will necessarily charge at 4000 W. The actual charging limit is defined by inverter configuration; LINEA does not change that inverter limit.

Configurable parameters include charging duration, maximum acceptable purchase-price trigger and a real-time display of the selected charging window. Trigger-based and peak-based selection can be used independently or together.

## SELECTION_flows_28062025.json

- Fixed time synchronization. After FLOW startup, the initial sequence runs within roughly 20 seconds and the FLOW becomes fully loaded and synchronized shortly afterwards.
- Corrected clock synchronization affecting SPOT control.
- Improved Config-card grouping.
- Added configurable Morning/Evening Peak duration from 1 to 4 hours.
- Added display of the peak times calculated by LINEA for the current day.

#### Dynamic SOC Reserve
Added Dynamic SOC Reserve for morning battery selling. The function calculates the energy reserve required until sunrise from configured hourly consumption, sunrise offset and SOC delta, and only sells energy above that reserve.

The function requires a fixed VRM installation location and VRM access. Sunrise data is obtained from the Sunrise-Sunset API once per day, after startup or shortly after midnight.

## SELECTION_flows_20052025.json

- Added **SOC delta before export**. After enabling it, the user defines the battery SOC threshold from which surplus may be exported to the grid. This allows the battery to charge to a selected reserve first and only then export surplus until the configured time.

## SELECTION_flows_04052025.json

- Fixed minor issues throughout the FLOW.
- Added **Control Mode: ESS / AC Grid**, switching control between register 2700 and register 2716.
- Added **NON Battery Priority Mode**, prioritizing grid/PV energy over battery energy, useful for protecting the battery during EV charging or other high loads.

## SELECTION_flows_14042025.json

- Fixed hour allocation.
- Fixed SPOT parsing.
- Fixed a VRM portal issue.

## SELECTION_flows_05042025.json

- Fixed identified issues.
- Corrected clock handling.
- Reworked the SPOT flow.
- Added **Energy Threshold Injector**, an internal-purpose function.

## Selection_flows_28032025.json
- Added control of register 2706 (**Maximum System Grid Feed In**).
- When DC feed-in is enabled through register 2707, LINEA controls DC/PV export. AC-connected generators or other AC sources are not included by this logic; support could be extended through register 2708.
- When the battery is full, register 2706 limits maximum grid export.
- When the battery is not full but export is requested, LINEA periodically calculates PV production minus consumption and balancing reserve and writes the resulting Grid Point to register 2700.
- If the calculated value exceeds the register-2706 limit, the value is capped accordingly.
- Short transient overshoot may occur while Cerbo and the ESS settle. Therefore the configured software limit should remain below the distributor's contractual export limit.
- Morning/Evening battery-selling functions use their own maximum export setting and are additionally constrained by inverter configuration.
## Linea_flows_15032025.json
- FLOW optimization.
- Added per-phase energy counters. GRID distinguishes import and export (`O / OUTPUT`).
- Added battery energy counters for charging and discharging. These battery counters reset when Node-RED restarts.
- Added inverter temperatures using LM335 sensors. Sensor naming must contain `L1`, `L2`, or `L3` as appropriate.
- FLOW must be connected to VRM; thermometer IDs are obtained from VRM and values are then read through Modbus.
## SELECTION_flows_20022025.json
- Fixed minor typos.
- Removed graphs that never worked reliably; their code remains commented in the FLOW.
- Removed the inverter/FW-regulator cooling FLOW controlled through the Shelly plugin.
- From this version, only relevant cards are exported using `node-red-contrib-flow-manager`.
## K_ALL_flows_09022025.json
- Modified token handling. A VRM token can be generated directly in VRM and inserted manually, or generated through the FLOW when sufficient VRM permissions are available.
- ADMIN rights are recommended because Technician/User permissions may be insufficient.
- Other FLOW variants use the safer BearerToken method.
- This `K_ALL` branch was not expected to receive further development.
## 07022025
- Minor code changes.
- Added icons.
- Added calculations for energy stored into and discharged from the battery.
## 02112024
- Added a runtime indicator to `FVE::Real Data`.
- FLOW detects the environment in which it runs and automatically selects the storage path.
## 14102024
- Updated morning and afternoon battery-selling logic.
## 28082024
- Added connection-state indication to the Config-card CONNECT button by checking installation serial-number communication because the underlying Modbus node does not provide a reliable state event.
- Added default configuration handling with DEFAULT LOAD / DEFAULT SAVE and later CONFIG LOAD / CONFIG SAVE.
- The default configuration is loaded after Node-RED restart; it is intended to store basic values such as IP addresses, VRM login data and default function-switch states.
## 15092024
- Added **GRID CHARGING** for charging the battery from the grid. `MAX GRID POINT` defines the requested charging power and the user selects the target SOC.
- The function stops after the target SOC is reached; it is intended for deliberate charging, for example before an expected outage, not for permanently maintaining SOC.
- Added VRM data-loading diagnostics.
- Fixed VRM token loss by recovering the token from stored configuration when needed.
## 25082024
- Fixed minor tooltip issues.
- Changed the configuration-file storage path.
- Token is stored in the configuration file so VRM connectivity can survive Node-RED/container restarts.
- Added basic token validation to the token input.
## 14082024
- Fixed minor CSS and FLOW issues.
- Added configurable `nBalancingReserve` in watts to reduce battery charge/discharge oscillation when calculating the difference between load and PV production.
- Added current date and time to the installation-information label.
## 13082024
- Completed the evening battery-selling function.
- Added configuration of maximum battery discharge power shared by morning and evening peak functions.
- Fixed boolean interpretation in several places.
- Added VRM access configuration and installation information to the Config/FVE cards.
- RealTime Power remained under development in this version.
## 03082024
- Fixed a critical SPOT failure after removal of `node-red-contrib-config`, which required substantial FLOW changes.
## 31072024
- Removed dependency on `node-red-contrib-config`.
- Removed unused dependency on `node-red-contrib-victron-modbus`.
- Fixed UI controls that did not correctly react after loading configuration.
- The **Feed-in On/Off** switch is intentionally not persisted in the global configuration to prevent accidental feed-in activation; it is controlled by SPOT automation and its price trigger.
## 29072024
- Added the Config card with TCP settings for FVE IP address, port and unit ID.
- Added configuration-file creation and SAVE/DELETE CONFIG handling.
- Converted variables throughout the FLOW from standalone definitions to a global structure that can be saved to disk.
## 20072024
- Fixed a CSS issue that caused columns to move when cards were closed.
- Fixed FVE cooling FLOW by adding a 3-second delay node.
## 19072024
- Fixed `CopyOnChange_2707` so register 2707 is written only when the value changes.
- Fixed global time functions so `sExtractTime` and `mExtractTime` use the same time base.
- Substantially reworked FVE cooling: fan-state feedback comes from the Shelly plugin, fan start is immediate at the trigger threshold, and stop requires repeated STOP conditions to avoid rapid switching caused by short PV fluctuations.
## 15072024
- Fixed signed/unsigned conversion functions in Battery Control.
- Partially reworked Spot Excess Control and moved register-2707 write logic into a Function node.
## 14072024
- Added direct fan control through the Shelly plugin; this FLOW was still incomplete and installation-specific.
- Added the first working morning-peak battery-selling function with a two-hour morning peak.
- Added optional DEBUG outputs.
- Added global reusable functions.
- Corrected time handling throughout the FLOW and introduced `County_Code` and `TZidentifier` global GUI settings.
