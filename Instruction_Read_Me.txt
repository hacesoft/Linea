//*****************************************************************************
//
// File Name        : 'Instrukce_ Cti_Me'
// Title            : Pokyny
// Author           : http://www.prochazka.zde.cz -> hacesoft 2025
// Created          : 22-06-2024, 11:00
// Revised          : 08-02-2025, 18:45
// Version          : 1.1
// Target Platfirm  : Node-RED
//
// This code is distributed under the GNU Public License
// Vsechny informace jsou zahrnuty pod GPL licenci, pokud není explicitne uveden jiný typ licence.
// Pouzivání techto stránek ke komercním �celum lze jen se souhlasem autora.
// Vsechna práva vyhrazena (c) 1997 - 2025 hacesoft.
//
//*****************************************************************************

# Linea
Control of photovoltaic power plant using Node-RED” for Node-RED v4.0.2

>> I would appreciate any feedback and corrections or improvements.

![image](https://github.com/user-attachments/assets/21efd405-18f0-4cbd-80e1-cc43f975d301)

## Description of FLOW LINEA for Node-RED

> FLOW is an extension for Node-RED that provides a comprehensive set of features for controlling and monitoring photovoltaic power plants (PV systems) and battery storage systems (Victron PV systems).

### Key Details:

- **Hosting and Setup:**
  - FLOW runs within Node-RED, which needs to be installed on a hosting device, such as a NAS, laptop, or PC that remains powered on.

### FAQ:

- **No Export Permission:**
  - In this case, any control is unnecessary as there is nothing to control.

- **Export Permission Granted:**
  - If you have export permission from the distributor and want to sell excess energy, this FLOW solution is ideal.
  - Alternatively, you can look for other solutions (e.g., Delte Green, direct control in VRM).

- **Export Already Controlled by Another Solution:**
  - Only one export control solution can be used; otherwise, different controls will conflict.

- **Limited Export Permission:**
  - No problem, set the maximum allowed export in Cerbo, minus some reserve, e.g., 500 W.

- **How the PV System Will Be Controlled:**
  - The first priority is supplying consumption, which cannot be influenced.
  - The remaining energy can be directed to the battery or sold.
  - Sales can be configured in various ways:
    - Option to sell to the grid.
    - Option to sell battery energy during morning or evening peaks to a set SOC, or both.
    - Sell surplus at the SPOT price up to a set value.
    - There are many options.
  - With reasonable settings, FLOW functions automatically and sensibly.

- **PV System Behavior Without This FLOW:**
  - First, consumption from the panels is satisfied.
  - Excess goes to the battery, and once the battery is full, the remaining excess is sold if export is permitted (basic setting in Cerbo).
  - Without FLOW permission, nothing will ever be sold.
  - You can also set a limit on the maximum current sent to the grid.

- **What Happens When There Is No PV System Control and Energy Is Sold:**
  - This depends on your buyback agreement:
    - For fixed prices, control is unnecessary.
    - For spot prices:
      - If the SPOT price is negative, you pay for the supply (applies if there is no PV system control).
      - If the "Limit Price" is set lower than the current SPOT price, you sell.
      - If the "Limit Price" is set higher than the current SPOT price, nothing is sold.

### Prerequisites:

- **Node-RED Activation:**
  - Ensure Node-RED is activated either in CERBO (Victron PV system control unit) or hosted on another device.

- **Access to PV System:**
  - You must have login credentials for the PV system.

- **Network Configuration:**
  - Node-RED should be on the same network as the PV system. If not, set appropriate firewall/router rules or create a permanent bridge (e.g., OpenVPN) to connect them.

- It is recommended to install the latest version of FLOW.

### Functionality:

- **Monitoring:**
  - Real-time monitoring of energy production, consumption, and storage levels.

- **Control:**
  - Remote control of system parameters and settings.

### Installation and Configuration:

- **Node-RED Installation:**
  - Follow the official Node-RED installation guide for your specific operating system.

- **Install Dependent Libraries:**
  - Install the dependent libraries required for FLOW to run (list at the end of this page). You may need to restart Node-RED; follow the on-screen instructions. Here is a guide: [Node-RED Palette Manager](https://nodered.org/docs/user-guide/editor/palette/manager)

- **Install FLOW via Import:**
  - Import the `ALL_flow_xxx.json` file into Node-RED (where `xxx` is the version number). Here is a guide: [Node-RED Import/Export](https://nodered.org/docs/user-guide/editor/workspace/import-export)
  - Or: Click anywhere on an empty spot in FLOW with the left mouse button and select: ![image](https://github.com/user-attachments/assets/644a08ea-4e1f-48ce-9c42-d9b74f0a1a06)
  - On the Import nodes tab, select CLIPBOARD, click the select and file import button, choose your FLOW file, and confirm everything.

- **Configure FLOW:**
  - Configuration is done via the FLOW UI, which means: `http://IP:PORT/ui/` e.g., `http://192.168.8.10/1881/ui/`
    - `IP` is the address of your Node-Red installation.
    - `PORT` is the value you entered during Node-Red installation.

- Go to the CONFIG tab. Enter the IP address (without `http`) of your PV system and click the CONNECT button. In the VRM settings, connect FLOW with VRM. If you do not do this, FLOW will continue to function, but these details will not be displayed: ![image](https://github.com/user-attachments/assets/0ab15b2c-12f5-42dc-b8d7-0cd2e5f728cd)

- After setup, save the configuration. Everything is explained below in this text.

- FLOW is a comprehensive tool for controlling and monitoring PV systems and battery storage, continuously bringing new features and improvements. For more information and updates, follow the development logs and documentation.

### Features and Controls:

- **Automatic Control Based on SPOT:**
  - Monitors the current SPOT price and controls exports.

- **Exports:**
  - Enables or disables exports.

- **Battery Charging Shift:**
  - Sets the time period for battery charging.

- **Battery Sales:**
  - Function for selling battery energy during morning and evening peaks.

- **Configuration Directly from UI:**
  - No need to modify the FLOW itself.

- **Save and Load Default Configuration:**

- **Connection Status and Error Indicators:**

>> I recommend always installing the latest version of FLOW. Installation is done by importing FLOW into Node-RED, but first, do not forget to install the dependent libraries (defined at the end of this page, including versions on which FLOW is built and tested).

### Version Descriptions:

- **Version 07022025:**
  - Minor code adjustments.
  - Icons added.
  - Added calculations for storing electrical energy to and from the battery.

- **Version 02112024:**
  - Added an indicator on the FVE::Real Data card - runtime, which shows how long a particular FLOW has been running. The counter resets with any change.
  - Flow can evaluate where it is running and set the storage path accordingly.

- **Version 14102014:**
  - Adjusted logic for selling battery energy during morning and afternoon peaks.

- **Version 28082024:**
  - On the config card, the CONNECT button is indicated by whether the device is connected to the given IP address. An error in the node-red-contrib-modbus library still exists. The issue is that the flow attempts to read the installation SN. If it fails or no SN information is received within about 4 seconds, the indicator turns gray. Upon connection, the indicator turns green. The node connection component does not send any messages, so a timestamp was added to the flow. If valid, the device at the given IP address is unavailable.

- **Version 15092024:**
  - Added GRID CHARGING function to charge the battery from the grid. The charging current is set on the CONFIG card, item MAX GRID POINT, regardless of whether the number is positive or negative. The respective functions will adjust it according to their requirements. Set the SOC to which it should charge, and it will automatically turn off and stop once reached. This is not a function to maintain the battery at a given SOC but is intended for emergency battery charging in anticipation of a power outage. Only the set SOC value is saved in the configuration.
  - Added diagnostic functions regarding data loading failures from VRM. May report file saving errors.
  - Fixed the issue of token loss for VRM. The fix is as follows: In case of token loss, the configuration is loaded, and only the token value is taken; the rest of the settings are ignored.

- **Version 25082024:**
  - Minor tooltip fixes.
  - Changed the path for saving the configuration file to root/mode_modules.
  - The token is saved in the configuration file. When Node-Red runs in a container, it often restarts, causing the VRM connection to be lost.
  - In the Inmout box for entering the Token, there is a primitive test for token validation.

- **Version 14082024:**
  - Fixed minor CSS profile errors.
  - Fixed minor FLOW errors.
  - Option to configure the constant: nBalancingReserve. The value in watts is added to the difference between the total load and the current production from the PV panel. The difference is sent to the grid to prevent battery charging/discharging fluctuations. This constant is set to 230W for me. Other systems may need adjustments.
  - Added the current time and date to the "Installation Details" label.

- **Version 13082024:**
  - Finally completed the function for evening battery sales.
  - On the CONFIG card, you can now set the maximum discharge current in W for both morning and evening peaks combined.
  - Minor fixes, mainly interpreting FALSE and TRUE values using double negations (e.g., !!fGetConfigProperty()).
  - On the config card, set up access to VRM. Enter your login credentials (only the email is saved, not the password). Also, enter the name of your installation, to which a random string is added. The installation number is also required, found in the URL of your VRM installation. Upon connection, a token is generated, stored in a global variable until Cergo or the Node-RED container is restarted. A new token request is then required. The generated token can be viewed in your VRM installation settings under "Preferences/Integrations/Access Tokens." On the PV system card, under Real Data, you will find information about your installation.
  - The RealTime Power card is still in development and may not be reliable yet.

- **Version 03082024:**
  - Fixed a critical SPOT failure error after removing the node-red-contrib-config library; flow required significant modifications.

- **Version 31072024:**
  - Removed dependency on the library: node-red-contrib-config.
  - Removed dependency on the library: node-red-contrib-victron-modbus. I never used this library and eventually discarded it.
  - FIX: Some controls did not react correctly to the current settings when loading the configuration.
  - NOTE: The "Exports: On/Off" switch is not and will not be saved to the global structure and thus will not be saved with the configuration to prevent unwanted enabling of exports. This is controlled by the automatic SPOT and limit price (trigger).

- **Version 29072024:**
  - Added the Config card - currently functional for setting TCP, where you enter the IP address of your PV system, port, and ID, which you usually won't change. The default port is 502, and the default ID is 100. Then, click connect. Due to an error in the modbus library, the connection status indication does not work, and you will always see CONNECTING...
  - The FILE setting is also functional. If this is your first installation and the configuration file does not exist on the disk (in the ROOT directory of node-red), it will create one. You can adjust the setting parameters as needed and save them with the SAVE CONFIG button. The DELETE CONFIG button is useful when installing a new version of FLOW to correctly load default values.
  - Variables across FLOW have been changed from separate definitions to a global structure that can be saved to disk.

![image](https://github.com/user-attachments/assets/ef521fc2-1faa-478d-a702-8a99e7f5978a)


```javascript
// Definice timezone and Country code
// LIST CODES: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
const sCounty_Code ="cs-CZ";
const sTZidentifier = "Europe/Prague";
```


### Nainstalované NODE:

 - node-red - 4.0.3
 - node-red-contrib-modbus - 5.42.0
 - node-red-dashboard - 3.6.5