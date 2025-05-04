# LINEA - Control of photovoltaic power plant using Node-RED

![Node-RED for Victron systems](https://github.com/user-attachments/assets/834f8837-f44a-41ba-99a4-5de1432b4935)

## 📑 Contents
- [🔗 Latest FLOW Version](#latest-flow)
- [📌 Introduction](#introduction)
- [❓ FAQ - Frequently Asked Questions](#faq)
- [💻 Prerequisites and Installation](#prerequisites-and-installation)
- [🛠️ Functionality and Plugins](#functionality-and-plugins)
- [📊 Control Panel Functions Description](#control-panel-functions-description)
- [⚙️ Register Function Explanation](#register-function-explanation)
- [📝 Version History](#version-history)
- [📦 Installed NODE](#installed-node)
- [⚠️ Disclaimer](#disclaimer)

## 🔗 [Latest FLOW Version](#latest-flow)  <- Shortcut to the latest FLOW version.

## Introduction

FLOW is an extension for Node-RED that provides a comprehensive set of functions for controlling and monitoring photovoltaic power plants (VICTRON PV) and battery storage systems.

### Key Details:

- **Hosting and Configuration:** FLOW runs within Node-RED, which needs to be installed on a hosting device such as a NAS, laptop, or PC that is permanently powered on.

I welcome any feedback and possible bug fixes and improvements.

## FAQ

### I don't have permission for feed-in
In this case, any control is unnecessary as there is nothing to control.

### I have permission for feed-in
- If you have feed-in permission from your distributor and want to sell excess energy, this FLOW solution is ideal.
- Alternatively, you can look for another solution (e.g., Delte Green, control directly in VRM).

### Feed-in is already controlled by another solution
Only one feed-in control can be used, otherwise different controls will compete for management.

### I have limited feed-in permission
No problem, set the maximum allowed feed-in in Cerbo, minus some reserve, for example 500 W.

### How the PV system will be controlled
- The first priority is supply to consumption, which cannot be influenced.
- The remaining energy can be controlled as to what happens with it.
- It either goes to the battery or for sale.
- You can set up sales in various ways:
  - Option to sell to the GRID.
  - Option to sell energy in the battery during morning or evening peak to the set SOC, or both.
  - Sell excess on SPOT up to the set value.
  - There are many options.
  - Register 2706 (Maximum System Grid Feed In)
    - If feed-in is allowed (register 2707, DC only!), keep in mind that FLOW LINEA only controls DC feed-in, i.e., from photovoltaic panels. If you have a generator or other device connected to AC, FLOW LINEA cannot work with it. However, this setting can be relatively easily extended using register 2708.
- With reasonable settings, FLOW works entirely reasonably and automatically.

### How PV system behaves without this FLOW
- First, consumption from the panel is satisfied.
- The rest goes to the battery, and once the battery is charged, the remaining energy goes for sale if selling is allowed (basic setting in Cerbo).
- Without FLOW permission, it never sells anything.
- You can also set a limit on how much maximum current is sent to the GRID.
- The setting in register 2706 is accepted. This means that whatever is set there will be sent during feed-in if the battery is full and there is no consumption. Then Cerbo limits production from MPPT.

### What happens when I have no PV control and sell energy
- It depends on what type of purchase agreement you have:
  - For fixed prices, control is completely unnecessary.
  - For spot prices:
    - If you have a negative SPOT, you pay for electricity delivery (applies if you don't have any PV control).
    - If the "Limit Price" is set lower than the current SPOT, you sell.
    - If the "Limit Price" is set higher than the current SPOT, nothing is sold.

## Prerequisites and Installation

### Prerequisites:

- **Node-RED Activation:** Make sure Node-RED is activated either in CERBO (control unit for Victron PV) or hosted on another device.
- **Access to PV:** You must have login credentials for accessing the PV system.
- **Network Configuration:** Node-RED should be on the same network as the PV system. If not, you need to set appropriate firewall/router rules or create a permanent bridge (e.g., OpenVPN) to connect them.
- It is recommended to install the latest version of FLOW.

### Installation and Configuration:

- **Node-RED Installation:** Follow the official Node-RED installation guide for your specific operating system.
- **Install Dependent Libraries** for running FLOW (the list is at the end of this page). You may need to restart Node-Red; follow the instructions on the screen. Here's a guide on how to proceed: [Node-RED Palette Manager](https://nodered.org/docs/user-guide/editor/palette/manager)
- **Install FLOW through Import:**
  - Import the `SELECTION_flows_xxx.json` file into Node-RED (where `xxx` represents the version number). Here's a guide on how to proceed: [Node-RED Import/Export](https://nodered.org/docs/user-guide/editor/workspace/import-export)
  - Or: Click anywhere on an empty space in FLOW with the left mouse button and select: 
  
  ![Import menu](https://github.com/user-attachments/assets/644a08ea-4e1f-48ce-9c42-d9b74f0a1a06)
  
  - In Import nodes, select the CLIPBOARD tab, click on select a file import, choose your FLOW file, and confirm everything.
- **FLOW Configuration:** Configuration is done from the FLOW UI, which means: `http://IP:PORT/ui/` (example: `http://192.168.8.10:1881/ui/`)
  - `IP` is the address of your Node-Red installation
  - `PORT` is the value you entered during Node-Red installation
- Go to the CONFIG tab. Enter the IP address (without `http`) of your PV and click the CONNECT button; in the VRM settings, you'll connect FLOW with VRM. If you don't do this, FLOW continues to function, but these data will not be displayed: 

![VRM data](https://github.com/user-attachments/assets/0ab15b2c-12f5-42dc-b8d7-0cd2e5f728cd)

- After setting up, save the configuration. Everything is described below in this text.

## Functionality and Plugins

### Functionality:

- **Monitoring:** Real-time monitoring of energy production, consumption, and storage level.
- **Control:** Remote control of system parameters and settings.
- **Plugins**

### Plugins:
- Cooling for inverters and MPTT trackers: https://github.com/hacesoft/Cooling_Trackers_Rack
- HW Plugin - thermometers for Cerbo: Plugins: https://github.com/hacesoft/LM335
- Guide for controlling a boiler using Node-RED: https://github.com/hacesoft/Clever_boiler
- ModBus Scanner: https://github.com/hacesoft/Scanner_ModBus

### Functions and Controls:

- **Automatic control according to SPOT:** Monitors the current spot price and controls feed-in.
- **Feed-in:** Turns feed-in on or off.
- **Battery Charging Delay:** Sets the time period for battery charging.
- **Battery Selling:** Function for selling battery energy during morning and evening peak hours.
- **Configuration directly from UI, without interfering with the FLOW itself.**
- **Saving and loading default configuration.**
- **Connection status and error indicators.**

> I always recommend installing the latest version of FLOW. Installation is done by importing FLOW into Node-RED, but first, remember to install the dependent libraries (these are defined at the end of this page, including versions on which FLOW is built and tested).

## Control Panel Functions Description

### 1. Real Data Section - Detailed Overview (PV Tab)

#### Load and Consumption by Phase

- **Total AC Load:** Total current consumption of all phases in W
- **L1:** Current consumption on phase L1 in W (daily consumption in kWh and inverter temperature in °C in parentheses)
- **L2:** Current consumption on phase L2 in W (daily consumption in kWh and inverter temperature in °C in parentheses)
- **L3:** Current consumption on phase L3 in W (daily consumption in kWh and inverter temperature in °C in parentheses)

#### Grid

- **Total Grid:** Total current import/export to the grid in W (positive value = import, negative = export)
- **L1:** Current import/export on phase L1 in W (daily counter Wh and Wh in parentheses)
- **L2:** Current import/export on phase L2 in W (daily counter Wh and Wh in parentheses)
- **L3:** Current import/export on phase L3 in W (daily counter Wh and Wh in parentheses)

#### Operation Information

- **Operation Time:** How long FLOW has been running since the last restart
- **Installation Data:** Current system date and time

#### Feed-in and Consumption

- **To Grid:** Amount of energy supplied to the grid in kWh
- **From Grid:** Amount of energy taken from the grid in kWh
- **Consumption Prediction:** Expected total consumption in kWh
- **Total Consumption:** Current total consumption in kWh
- **Solar Prediction:** Expected production from solar panels in kWh
- **Total Solar Energy:** Current production from solar panels in kWh

#### Battery Status

- **Battery (Limit SoC):** Current battery charge state in percentage with information about the minimum set SoC limit
- **Battery Discharge:** Current battery discharge power in W (negative value = discharge)
- **Battery Discharge Current:** Current discharge current in A
- **Battery Voltage:** Current battery voltage in V
- **Battery Efficiency:** Battery efficiency in percentage (ratio of energy taken to energy supplied)

#### Energy Counters:
- **First Battery Icon:** Total amount of energy supplied to the battery in Wh
- **Second Battery Icon:** Total amount of energy taken from the battery in Wh

#### Regulator and Ventilation Status

- **FW Regulators:** Total power of FW regulators in W
- **MPTT FAN:** Fan status indicator for MPPT regulators (green = on)
- **CHANGER FAN:** Fan status indicator for the inverter (green = on)

**Note:** Inverter temperature data and predictions are available only with active connection to the VRM portal. Battery energy counters are reset only when Node-RED restarts.

### 2. Feed-in Control and SPOT Section (PV Tab)

#### Basic Data

- **Feed-in:** Current feed-in permission status to the grid ("FORBIDDEN" or "ALLOWED")
- **Automatic according to SPOT:** Switch for activating automatic control according to SPOT price
- **Connection to SPOT:** Status indicator of connection to the SPOT price data source
- **Current SPOT Price:** Displays the current electricity price on SPOT in CZK/kWh
- **Limit Price:** Adjustable price threshold for activating feed-in in CZK/kWh
- **Feed-in: Off / On:** Manual switch for allowing feed-in

#### Daily SPOT Price Overview

- **Today: Min / Max:** Minimum and maximum SPOT price for the current day in CZK/kWh
- **Today's SPOT:** Graph showing SPOT price progress during the current day
  - Orange curve: SPOT price progress (CZK/kWh)
  - Green line: Set limit price
  - Red mark: Current time position

#### Future SPOT Prices

- **Tomorrow: Min / Max:** Minimum and maximum expected SPOT price for the next day
- **Tomorrow's SPOT:** Graph of expected SPOT prices for the next day

**Note:** SPOT price data for the next day is published by the Electricity Market Operator (OTE) around 2:00 PM the previous day. Until then, the system displays "unavailable data". OTE is responsible for organizing the short-term electricity and gas market in the Czech Republic and publishes this data based on the results of trading on the daily market.

### 3. PV Tab - Additional Functions

#### Battery Control Functions

- **Battery Charging Delay:** Switch for activating delayed battery charging
  - Allows shifting charging to a later time when electricity consumption is low
  - Used when it's more advantageous to sell electricity than store it in the battery

#### Battery Selling Functions

- **Morning Battery Selling:** Switch for activating energy selling from the battery during morning peak
  - Adjustable time interval and target SOC
  - Ideal for utilizing higher electricity prices in the morning hours

- **Evening Battery Selling:** Switch for activating energy selling from the battery during evening peak
  - Adjustable time interval and target SOC
  - Suitable for selling energy during the evening peak

#### NON Battery Priority Mode

- Switch for activating a mode that prioritizes using energy from the grid or PV panels over battery energy
- Battery protection when charging an electric vehicle or other energy-intensive consumption

#### Energy Threshold Injector

- Function behaves the same as the Set Point function, but with the difference that it is active only during supply from PV panels
- If there is insufficient supply from PV panels, the function does nothing
- Used for optimizing energy flows only when we have sufficient production

#### Set Point

- Sets a constant power value (positive or negative) according to the AcPowerSetPoint adjustment element
- Advantageous for minimizing unwanted grid consumption - for example, by setting to -100W
- The system will try to reach this value and will not oscillate around 0, but around the set value

#### GRID CHARGING

- Switch for activating battery charging from the grid
- Useful for expected power outages
- Adjustable target SOC, after which charging automatically turns off

### 4. Control Mode: ESS / AC Grid

- Function for switching between two photovoltaic system control modes
- Position is indicated by a green triangle
- Switches between registers 2700 and 2716

#### ESS Mode (register 2700):

- Implemented in FLASH memory
- Controls energy flow between batteries, grid, and consumption
- Classic mode for common use

#### AC Grid Mode (register 2716):

- Implemented in RAM memory as opposed to the original register 2700, which is implemented in FLASH
- Register 2716 is 32-bit
- Allows direct control of power supplied to the grid
- Advantage: saves FLASH memory life due to limited number of writes

**Important Note:** To use register 2716, you need to have CERBO updated to at least version 3.50 and in MultiPlus-II 48 inverters, you need minimum firmware v510.

### 5. Detailed Description of Specific Functions

#### Energy Threshold Injector

- Functions the same as Set Point, but is activated only with sufficient energy supply from PV panels
- With insufficient production from panels, the function does nothing
- Allows setting parameters for controlling feed-in without affecting the system during low production
- Suitable for ensuring stable feed-in only with sufficient production from PV panels

#### NON Battery Priority Mode
This function allows prioritizing electricity use directly from the grid or PV panels over drawing from batteries:

- **Primary Purpose:** Battery protection, especially when charging an electric vehicle
- **Operating Principle:**
  - When turned on, the system prefers power from the grid or PV panels
  - Battery energy is used only when other sources are not sufficient to cover consumption
  - During electric vehicle charging or other demanding consumption, the battery system is preserved

- **Power Size Calculation:**
  - The system calculates the difference between current PV production, total consumption, and balancing reserve
  - If the result is negative, it is converted to a positive value using the Math.abs() function
  - This value is set as a request for grid consumption

- **Benefits:**
  - Extended battery life due to fewer cycles
  - Protection against deep discharge during demanding consumption
  - Ideal for regular electric vehicle charging

### 6. CONFIG Tab - System Settings

#### FILE Section

- **SAVE:** Saving the current configuration to a file
- **LOAD:** Loading configuration from a file
- **DELETE CONFIG:** Deleting the configuration file

#### TCP Section

- **TCP Address:** IP address of your PV system (without http://)
- **TCP Port:** Port for TCP communication (standard 502)
- **TCP ID:** Device ID for Modbus communication (standard 100)
- **CONNECT:** Button for connecting to the PV system with status indicator

#### Config Section

- **ESS IP Address:** IP address of the ESS system
- **ESS IP Address Checking:** Address for checking ESS availability
- **IP ESS Modbus:** IP address for Modbus communication with ESS
- **IP Esp-01:** IP address of the ESP module (if used)
- **Maximum Grid Point:** Maximum power supplied to the grid from batteries in W
- **nBalancingReserve:** Balancing reserve value in W
- **Maximum Grid Feed In:** Setting the maximum power for grid supply (register 2706) in W
- **SET GRID FEED IN:** Button for setting the value in register 2706
- **GET GRID FEED IN:** Button for reading the current value from register 2706

#### VRM Section

- **Email:** Login email to the VRM portal
- **Name your Installation:** Name of your installation in the VRM system
- **VRM ID:** Identification number of your installation from the URL in VRM
- **Bearer Token:** Generated token for API access
- **SUBMIT:** Button for submitting data and generating the token

### 7. Recommended Procedures

- First, configure the TCP connection to your PV system in the CONFIG tab
- Connect FLOW with the VRM portal for extended monitoring options
- Set the SPOT limit price according to current market conditions
- Save the configuration using the SAVE button
- Regularly monitor production and consumption predictions for optimal settings
- When using advanced functions like Control Mode: ESS / AC Grid, verify that you have the necessary firmware versions

### 8. Troubleshooting

- Unavailable predictions: Check the connection to the VRM portal
- Connection errors: Verify the correctness of the IP address and port
- Outdated SPOT data: Check the connection to the OTE data source
- Problems with feed-in control: Make sure you don't have other feed-in control systems active
- Non-functional AC Grid Mode: Check the firmware version in CERBO (min. 3.50) and MultiPlus-II 48 inverters (min. v510)

## Register Function Explanation

### Modbus Registers Used in FLOW

- **Register 2700 – ESS Control Loop Setpoint**
  - This register is used to set the target power for the energy system (ESS) control.
  - It determines how much energy should be supplied to or taken from the grid.
  - It's used to control energy flow between batteries, grid, and consumption.
  - **Positive value** → ESS supplies energy to the grid
  - **Negative value** → ESS takes energy from the grid
  - **Zero (0)** → ESS tries to achieve balance

- **Register 2706 – Maximum System Grid Feed-In**
  - This register defines the **maximum allowed power** that the system can supply to the grid.
  - It ensures that the system **doesn't exceed limits set by the distributor or legislation**.

- **Register 2707 – Grid Feed-in Enable**
  - This register enables or disables energy supply to the grid from DC sources (photovoltaic panels).
  - **0** → Grid energy supply forbidden
  - **1** → Grid energy supply allowed
  - Controls only DC feed-in, not feed-in from AC sources like generators

- **Register 2708 – AC Feed-in Enable**
  - Similar to register 2707, but controls energy supply to the grid from AC sources.
  - In FLOW LINEA, it's not standardly implemented, but it can be added

- **Register 2716 – AC Grid Set Point**
  - Newer alternative to register 2700, implemented in RAM memory instead of FLASH
  - It's 32-bit, which provides a larger range of values
  - Allows direct control of power supplied to the grid
  - Saves FLASH memory life due to limited number of writes
  - Requires CERBO firmware min. version 3.50 and MultiPlus-II 48 inverters with firmware min. v510
  - Has the same function as register 2700, but with the advantage of RAM implementation

### How Does It Work Together?

1. **ESS sets the power (Register 2700 or 2716)** → For example, +2000 W means it wants to send 2 kW to the grid.
2. **Check against max. limit (Register 2706)** → If it's e.g. -1500 W, then the power is limited to this value.
3. **The system ensures it never exceeds the feed-in limit to the grid.**

This works if the **system is not controlled** or the **battery is full and there is no consumption**.

But if you **control the PV system** and want to ensure it **doesn't exceed the feed-in limit to the grid**, you need to **dynamically set the value of register 2700/2716** according to register 2706.

📌 **When reaching the limit in register 2706, the value may temporarily overshoot the set feed-in limit.**

👉 **Therefore, we always recommend setting a value lower than the maximum allowed power defined by the distributor or legislation.**

### Important Note:

- **🔹 FLOW LINEA**
  - Dynamically controls register **2700** or **2716** according to the current situation and selected mode.
  - **Register 2706** is settable **only in CONFIG** using the **SET GRID FEED-IN** button or directly in **CERBO**.
  - **It's not set anywhere else.** If your register 2706 value changes, it's probably being changed by other control of your PV system.

- **🔹 MAX GRID POINT**
  - Serves **only** for setting the **maximum power supplied to the grid from batteries**, **not** from PV panels!

- **🔹 MAXIMUM GRID FEED-IN**
  - Sets the value of **register 2706**.

![Register Diagram](https://github.com/user-attachments/assets/3396f3c3-941c-488b-9dd7-ac92a83a57a4)

<a name="latest-flow"></a>

## Version History

Important Note: From version SELECTION_flows_04052025, it is necessary to have CERBO updated to at least version 3.50 and in MultiPlus-II 48 inverters, firmware version min. v510.

### Version: 📌 SELECTION_flows_04052025.json
- Fixed minor bugs across the entire FLOW.
- Added functions: 
  - Control Mode: ESS / AC Grid
    - Function for switching between two photovoltaic system control modes
    - Position is indicated by a green triangle
    - Switches between registers 2700 and 2716
  - NON Battery Priority Mode
    - Switch for activating a mode that prioritizes using energy from the grid or PV panels over battery energy
    - Battery protection when charging an electric vehicle or other energy-intensive consumption

### Version: 📌 SELECTION_flows_14042025.json
- Fixed bugs in hour division.
- Fixed bugs in SPOT parsing.
- Fixed bug with VRM portal.

### Version: 📌 SELECTION_flows_05042025.json
- Fixed found bugs.
- Fixed work with hours (CLK).
- Reworked SPOT flow.
- Added function: Energy Threshold Injector. I won't comment on the function in detail, it serves my purposes. :)

### Version: 📌 Selection_flows_28032025.json
<details>
<summary>Show details</summary>

- Control of register 2706 setting (Maximum System Grid Feed In):
  - If feed-in is allowed (register 2707, DC only!), keep in mind that FLOW LINEA only controls DC feed-in, i.e., from photovoltaic panels. If you have a generator or other device connected to AC, FLOW LINEA cannot work with it. However, this setting can be relatively easily extended using register 2708.
  - There are two modes:
    - The battery is fully charged and there is not enough consumption.
      - In this case, the setting from register 2706 applies, which means that the maximum amount of energy sent to the grid is given by the value in this register.
    - The battery is not fully charged, but we still want to send feed-in to the grid according to the value in register 2706 (Maximum System Grid Feed In).
      - FLOW LINEA solves this as follows:
         - Once per second (more frequent checking doesn't make sense), it subtracts consumption and balancing reserve from current production.
         - It sets the resulting value to register 2700 (ESS control loop setpoint).
         - If this value is greater than the value in register 2706, it sets a value in register 2700 that is identical to the value in register 2706.
         - This way, there may be overshooting of the electrical current supplied to the grid. This phenomenon lasts until Cerbo processes everything and the system balances according to the set values in the registers. Therefore, never set the same value in register 2700 (ESS control loop setpoint) that you have permitted from the grid distributor, but always a lower one.
         - When production from photovoltaic panels is less than consumption plus balancing reserve and yet the Battery Charging Delay function is turned on, nothing is sent to the grid.
         - When the morning or evening battery selling function is turned on, or both, the maximum current to the grid is set separately and doesn't take into account the setting of register 2706. Even if you set some large value, it may not be accepted by the inverters, which then send as much as they have set in the PV system configuration.
</details>

### Version: 📌 Linea_flows_15032025.json
<details>
<summary>Show details</summary>

- FLOW optimization
- Added counters for individual phases. For GRID, it's distinguished between consumption (first parameter) and feed-in (marked as **O (OUTPUT)**).
  - The calculation is performed as follows: the current sample (value) is divided by 3600 and added to the total value. Counters are reset at midnight.  
  - In time, these values will be logged to a database using a suitable **PLUGIN**.
- Added energy counters for the battery, separately for charging and discharging. These counters are not reset, they are reset only when **Node-RED** restarts.  
  - In time, they will be logged to a database.
- Added inverter temperatures – need to install thermometers according to the instructions: [hacesoft/LM335](https://github.com/hacesoft/LM335).
  - For this to work, you need to follow the thermometer names (**name L1** – any of your designations, space, and capital letter **L**, followed by the phase number).  
  - Another condition is that **FLOW** must be connected to the **VRM** account.  
  - The thermometer ID is loaded from **VRM**, and according to this ID, the values are read from **Modbus**.
</details>

### Version: 📌 SELECTION_flows_20022025.json
<details>
<summary>Show details</summary>

- Fixed minor typos. Removed graphs that never worked properly. The code for graphs is part of FLOW but is only commented out. Removed FLOW for controlling cooling of inverters and FW regulators, which are controlled via Shelly plugin. From now on, only relevant cards will be exported, using the plugin: node-red-contrib-flow-manager.
</details>

### Version: 📌 K_ALL_flows_09022025.json
<details>
<summary>Show details</summary>

- Modified version of working with the token. The token can be generated directly in VRM and manually inserted into the file, or generated using FLOW. This is an option if for some reason you don't have the ability to generate a token. This can happen if you don't have sufficient rights in VRM for the integration of this FLOW. We recommend having ADMIN rights because rights like Technician or User may not be sufficient. In all other FLOW versions, the more secure BearerToken is used. This version (K_ALL_flows_09022025) probably won't be further developed.
</details>

### Version 📌 07022025
<details>
<summary>Show details</summary>

- Minor code adjustments.
- Added icons.
- Added calculations of electrical energy storage to and from the battery.
</details>

### Version 📌 02112024
<details>
<summary>Show details</summary>

- Added an indicator on the FVE::Real Data card - operation time, which indicates how long the given FLOW has been running. With any change, the counter is reset.
- Flow can evaluate where it's running and sets the storage path accordingly.
</details>

### Version 📌 14102014
<details>
<summary>Show details</summary>

- Adjusted the logic for battery selling functions in both morning and afternoon peaks.
</details>

### Version 📌 28082024
<details>
<summary>Show details</summary>

- On the config card, the CONNECT button is signaled by an indicator whether the device is connected to the given IP address. The error in the node-red-contrib-modbus library itself still exists. The whole trick is that the flow tries to read the SN of the installation. If it fails or if no information about the SN number comes for about 4 seconds, the indicator changes color to gray. After connection, the indicator changes color to green. The node connection component doesn't emit any messages, so I introduced a timestamp into the flow, and if it's valid, the device at the given IP address is not available.
- Added default configuration. After starting flow on the config card, you see the DEFAULT LOAD and DEFAULT SAVE buttons. Set everything as you need and save. This is the basic setting so you don't have to change anything in the global structure and change everything again when updating to a new version. You do everything just in the UI. After this save, the buttons change names to CONFIG LOAD and CONFIG SAVE, where you can set what you need. The whole point is that when node-red restarts, the default configuration is loaded. So there I recommend setting IP addresses and login to VRM, including the basic setting of function switches.
</details>

### Version 📌 15092024
<details>
<summary>Show details</summary>

- Added GRID CHARGING function for charging the battery from the GRID. The charging current is set on the CONFIG card, item MAX GRID POINT, and it doesn't matter whether the specified number is positive or negative. The appropriate functions adjust it according to their requirements. You set to what SOC it should charge, and after reaching it, it automatically turns off and doesn't continue. It's not a function to maintain the battery at a given SOC, but it's rather meant for emergency battery charging for an expected upcoming power outage. Only the set SOC value is saved to the configuration.
- Added diagnostic functions regarding VRM data loading dropouts. It may report file save errors.
- Fixed the token loss bug for VRM. The fix is done as follows: In case of token loss, the configuration is loaded and only the token value is taken, the rest of the settings are ignored.
</details>

### Version 📌 25082024
<details>
<summary>Show details</summary>

- Fixed details in the tooltip.
- Changed the path for saving the configuration file to root/mode_modules.
- The token is saved to the configuration file. When Node-Red runs in a container, it often restarts, and then the connection to VRM is lost.
- In the Inmout box for entering the Token, there is a primitive token validation test.
</details>

### Version 📌 14082024
<details>
<summary>Show details</summary>

- Fixed minor bugs in the CSS profile.
- Fixed minor bugs in FLOW.
- Ability to configure constant: nBalancingReserve. The value in watts is used to add to the value after the difference between the total load and the current production from the PV panel. The difference is sent to the GRID to prevent fluctuations in battery charging/discharging. This constant has a value of 230W for me. In another system, it may need to be adjusted.
- Added to LABEL "Installation Data:" the current time and date.
</details>

### Version 📌 13082024
<details>
<summary>Show details</summary>

- Finally, the function for evening battery selling is completed.
- On the CONFIG card, it's now possible to set the maximum discharge current in W, for morning and evening peaks together.
- Minor fixes, mainly interpretation of FALSE and TRUE values, using two negations in a row (example: !!fGetConfigProperty()).
- On the config card, you set access to VRM. Here you enter your login credentials (only the email is saved, not the password). You also enter the name of your installation, to which some random string is added. Also required is the number of your installation, which you'll find in the URL of your VRM installation. After connecting, a token is generated, which is not saved anywhere but exists in a global variable as long as you don't restart Cergo or the container where Node-RED runs. Then you need to make a new token request. You'll then see the generated token in your installation in VRM in the settings: "Preferences/Integrations/Access Tokens". On the PV tab in the Real Data column, you have information about your installation.
- The RealTime Power card is still in development, it shows something but can't be relied on yet.
</details>

### Version 📌 03082024
<details>
<summary>Show details</summary>

- Fixed a critical bug in SPOT failure after removing the node-red-contrib-config library; the flow required a larger adjustment.
</details>

### Version 📌 31072024
<details>
<summary>Show details</summary>

- Removal of dependency on the library: node-red-contrib-config.
- Removal of dependency on the library: node-red-contrib-victron-modbus. I never used this library and eventually abandoned it completely.
- FIX: Some control elements didn't properly respond to the current settings when the configuration was loaded.
- Note: The "Feed-in: On / Off" switch is not and will not be saved to the global structure, and therefore will not be saved with the configuration, to prevent unwanted feed-in activation. This is controlled by the automatic spot switch and the limit price (trigger).
</details>

### Version 📌 29072024
<details>
<summary>Show details</summary>

- Added Config card - from the card, the TCP setting works so far, where you enter the IP address of your PV system, port, and ID, which you usually won't change. The port is 502 by default and the ID is 100 by default. Then you click connect. Since there's a bug in the modbus library, the connection status signaling doesn't work and you'll always see CONNECTING...
- The FILE setting is also functional, where if you have the first installation, if there is no configuration file on the disk (in the ROOT directory of node-red), it creates one for you. You can adjust the setting parameters as you like and save with the SAVE CONFIG button. The DELETE CONFIG button is good when you install a new version of FLOW to correctly load the default values.
- Across FLOW, variables were changed from individual definitions to a global structure, which can then be saved to disk.
</details>

![Config panel](https://github.com/user-attachments/assets/ef521fc2-1faa-478d-a702-8a99e7f5978a)

### Version 📌 20072024
<details>
<summary>Show details</summary>

- FIX CSS profile - Minor detail, columns were moving when closing cards.
- FIX PV cooling flow - Added node delay 3s for packet shifting.
</details>

### Version 📌 19072024
<details>
<summary>Show details</summary>

- FIX function CopyOnChange_2707 - Now only changes are written and sent to register 2707, not the same value.
- FIX function GLOBAL FUNCTION - Functions sExtractTime and mExtractTime have the same time base using the fSetFixedDate function and cannot have different values.
- The PV cooling flow is most reworked:
  - Where there are indicators if the fans are spinning, this is the response from the SHELLY PLUGIN, which confirms receipt of the command. Monitoring of electrical consumption is not implemented.
  - Fan activation is immediate once it reaches the trigger value, but I turn them off only after receiving the STOP command 20 times in a row. This way we avoid any cloud issues.
</details>

### Version 📌 15072024
<details>
<summary>Show details</summary>

- FIX function convert signet to unsigned on flow battery control.
- FIX function convert unsigned to signet on flow battery control.
- Partially reworked Spot Excess Control flow. Part of the node redesigned into function node: Logical write register 2707.
</details>

### Version 📌 14072024
<details>
<summary>Show details</summary>

- Added flow for direct fan control via Shelly plugin. For your purposes, you need to edit or completely delete it. It's not completely finished, especially the GUI is unfinished and impractical.
- Morning peak selling function now works, so far the morning peak is defined for a 2-hour section.
- Added test outputs, just turn DEBUG to true in the appropriate node and possibly adjust the desired variable output.
- Added global functions - so repeating functions are written only once and in other node functions are just loaded.
- Fixed time handling across flow. From this version, it's set in global variables on the flow GUI County_Code and TZidentifier.


```javascript
// Definition of timezone and Country code
// LIST CODES: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
const sCounty_Code ="cs-CZ";
const sTZidentifier = "Europe/Prague";
```
</details>

## Installed NODE

- node-red - 4.0.3
- node-red-contrib-modbus - 5.42.0
- node-red-dashboard - 3.6.5

---

## ⚠️ Work carefully! Measure twice, cut once...

---

## Disclaimer

The author of this project provides no warranties, express or implied, regarding the correctness, reliability, functionality, or suitability for any purpose. All use of this software, code, schematics, guides, technical solutions, products, and any other provided materials is at the user's own risk.

The author bears no responsibility for any damages, losses, financial costs, direct or indirect damages arising from the use of these materials, including, but not limited to, loss of data, equipment damage, system failures, electrical or other installation failures, fires, loss of income, or other unforeseen consequences.

The user acknowledges that any modifications, assembly, installation, connections, or implementations based on the provided information are carried out entirely at their own risk. The author provides no guarantees of functionality, safety, or compliance with applicable legal standards and regulations.

The user agrees not to take any legal action against the author in connection with any damages or other claims arising from the use of this software, products, schematics, or guides. Any legal claims against the author are hereby expressly excluded and unenforceable, including through court proceedings.

By using these materials, the user confirms their agreement with the above conditions. If you disagree with them, do not use this software, schematics, guides, or other provided materials.
