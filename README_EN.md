Linea
Control of photovoltaic power plant using Node-RED” for Node-RED v4.0.2

I would appreciate any feedback and corrections of errors and improvements.

Description of FLOW LINEA for Node-RED
FLOW is an extension for Node-RED that provides a comprehensive set of functions for controlling and monitoring photovoltaic power plants (PV VICTRON) and battery storage.

Key Details:
Hosting and Setup: FLOW runs within Node-RED, which must be installed on a hosting device such as a NAS, laptop, or PC that is always on.
FAQ:
I do not have permission for surpluses:

In this case, any control is unnecessary as there is nothing to control.
I have permission for surpluses:

If I have permission for surpluses from the distributor and want to sell surpluses, this FLOW solution is ideal.
Or you can look for another solution (e.g., Delte Green, control directly in VRM).
Surpluses are already controlled by another solution:

Only one surplus control can be used; otherwise, different controls will conflict.
I have limited permission for surpluses:

No problem, set the maximum allowed surpluses in Cerbu, minus some reserve, e.g., 500 W.
How will the PV be controlled?

The first priority is supply to consumption, which cannot be influenced.
The remainder can be influenced as to what happens to it.
It either goes to the battery or for sale.
Sales can be set up in various ways:
Option to sell to the GRID.
Option to sell energy in the battery during morning or evening peak to the set SOC, or both.
Sell surplus on SPOT up to the set value.
There are plenty of options.
With reasonable settings, FLOW works quite reasonably and automatically.
How does the PV behave without this FLOW?

First, consumption from the panel is satisfied.
The remainder goes to the battery, and once the battery is charged, the remainder goes for sale if sales are permitted (basic setting in Cerbu).
Without FLOW permission, it never sells anything.
You can also set a limit on the maximum current sent to the GRID.
What happens if I do not have any PV control and sell energy?

This depends on the type of buyback agreement you have:
For fixed prices, control is completely unnecessary.
For spot prices:
If you have a negative SPOT, you pay for the electricity supply (applies if you have no PV control).
If the "Limit Price" is set lower than the current SPOT, it sells.
If the "Limit Price" is set higher than the current SPOT, nothing is sold.
Prerequisites:
Activation of Node-RED: Ensure Node-RED is activated either in CERBO (control unit for Victron PV) or hosted on another device.
Access to PV: You must have login credentials for accessing the PV.
Network Configuration: Node-RED should be on the same network as the PV. If not, appropriate firewall/router rules must be set, or a permanent bridge (e.g., OpenVPN) must be created for their connection.
It is recommended to install the latest version of FLOW.
Functionality:
Monitoring: Real-time monitoring of energy production, consumption, and storage level.
Control: Remote control of parameters and system settings.
Installation and Configuration:
Installation of Node-RED: Follow the official Node-RED installation guide for your specific operating system.
Install Dependent Libraries for running FLOW (list at the end of this page). You may need to restart Node-Red; follow the on-screen instructions. Here is the guide on how to proceed: Node-RED Palette Manager
Install FLOW via Import:
Import the ALL_flow_xxx.json file into Node-RED (where xxx is replaced by the version number). Here is the guide on how to proceed: Node-RED Import/Export
Or: Click anywhere on an empty spot in FLOW with the left mouse button and select:
In Import nodes, select the CLIPBOARD tab, click the select and file import button, choose your FLOW file, and confirm everything.
Configuration of FLOW: Configuration is done from the FLOW UI, which means: http://IP:PORT/ui/ (example: http://192.168.8.10:1881/ui/)
IP is the address of your Node-Red installation
PORT is the value you specified during the Node-Red installation
Navigate to the CONFIG tab. Enter the IP address (without http) of your PV and click the CONNECT button; in the VRM settings, connect FLOW with VRM. If you do not do this, FLOW will continue to function, but this data will not be displayed:
After setup, save the configuration. Everything is described below in this text.
FLOW is a comprehensive tool for controlling and monitoring PV and battery storage, constantly bringing new features and improvements. For more information and updates, follow the development logs and documentation.
Features and Controls:
Automatic Control Based on SPOT: Monitors the current spot price and controls surpluses.
Surpluses: Turns surpluses on or off.
Battery Charging Shift: Sets the time period for battery charging.
Battery Sale: Function for selling the battery during morning and evening peaks.
Option to configure directly from the UI without modifying the FLOW itself.
Saving and loading default configuration.
Connection status and error indicators.
I recommend always installing the latest version of FLOW. Installation is done by importing FLOW into Node-RED, but first, do not forget to install the dependent libraries (defined at the end of this page, including versions on which FLOW is built and tested).

Description of Individual Versions:
Version: SELECTION_flows_20022025.json

Minor typos fixed. Removed graphs that never worked correctly. The code for graphs is part of FLOW but is only commented out. Removed FLOW for controlling the cooling of inverters and FW regulators, which are controlled via the Shelly plugin. From now on, only relevant cards will be exported, using the add-on: node-red-contrib-flow-manager.
Version: K_ALL_flows_09022025.json:

Revised version of token handling. The token can be generated directly in VRM and manually entered into the file, or generated using FLOW. This is an option if, for some reason, you cannot generate a token. This can happen if you do not have sufficient rights in VRM for integrating this FLOW. We recommend having ADMIN rights, as rights such as Technician or User may not be sufficient. In all other versions of FLOW, the more secure BearerToken is used. This version (K_ALL_flows_09022025) will probably not be further developed.
Version 07022025:

Minor code tweaks.
Icons added.
Added calculations for storing electrical energy to and from the battery.
Version 02112024:

Added an indicator on the FVE::Real Data card - runtime, which indicates how long a particular FLOW has been running. The counter resets with any change.
Flow can determine where it is running and set the path to storage accordingly.
Version 14102014:

Revised logic for battery sale functions during morning and afternoon peaks.
Version 28082024:

On the config card, the CONNECT button is indicated by an indicator showing whether the device is connected to the given IP address. There is still an error in the node-red-contrib-modbus library itself. The whole issue is that the flow tries to read the SN of the installation. If it fails or no information about the SN is received for about 4 seconds, the indicator changes color to gray. Upon connection, the indicator changes color to green. The node connection component does not send any messages, so I introduced a timestamp into the flow, and if it is valid, the device is not available at the given IP address.
Added default configuration. After starting the flow on the config card, you will see the DEFAULT LOAD and DEFAULT SAVE buttons. Set everything as you need and save. This is the basic setting so that you do not have to change anything in the global structure and change everything again when updating to a new version. Everything is done only in the UI. After this save, the buttons will change names to CONFIG LOAD and CONFIG SAVE, where you set what you need. The whole point is that upon restarting node-red, the default configuration is loaded. So, I recommend setting IP addresses and login to VRM there, including basic switch settings.
Version 15092024:

Added GRID CHARGING function for charging the battery from the GRID. The charging current is set on the CONFIG card, item MAX GRID POINT, and it does not matter whether the number is positive or negative. The respective functions will adjust it according to their requirements. You set to what SOC it should charge, and after reaching it, it automatically turns off and does not continue. This is not a function to maintain the battery at a given SOC but is rather intended for emergency charging of the battery in anticipation of an imminent power outage. Only the set SOC value is saved in the configuration.
Added diagnostic functions regarding the failure to load data from VRM. It may report file save errors.
Fixed the error of losing the token for VRM. The fix is done as follows: In case of token loss, the configuration is loaded, and only the token value is taken; the rest of the settings are ignored.
Version 25082024:

Minor fixes in the tooltip.
Changed the path for saving the configuration file to root/mode_modules.
The token is saved in the configuration file. When Node-Red runs in a container, it often restarts, and then the connection to VRM is lost.
In the Inmout box for entering the Token, there is a primitive test for token validation.
Version 14082024:

Fixed minor bugs in the CSS profile.
Fixed minor bugs in FLOW.
Option to configure the constant: nBalancingReserve. The value in watts is used to add value to the difference between the total load and the current production from the PV panel. The difference is sent to the GRID to prevent battery charging/discharging fluctuations. This constant has a value of 230W for me. It may need to be adjusted for another system.
Added the current time and date to the “Installation Data:” LABEL.
Version 13082024:

Finally, the function for evening battery sale is completed.
On the CONFIG card, it is now possible to set the maximum discharge current in W for both morning and evening peaks combined.
Minor fixes, mainly the interpretation of FALSE and TRUE values, using double negations in succession (example: !!fGetConfigProperty()).
On the config card, set up access to VRM. Here, enter your login credentials (only the email is saved, not the password). Also, enter the name of your installation, to which a random string is added. Additionally, the number of your installation is required, which can be found in the URL of your VRM installation. After connecting, a token is generated, which is not stored anywhere but exists in the global variable as long as you do not restart Cergo or the container where Node-RED is running. Then, a new token request must be made. The generated token can then be viewed in your installation in VRM under settings: “Preferences/Integrations/Access Tokens”. On the FVE card in the Real Data column, you have information about your installation.
The RealTime Power card is still under development; although it shows something, it cannot be relied upon yet.
Version 03082024:

Fixed a critical SPOT failure error after removing the node-red-contrib-config library; the flow required significant modification.
Version 31072024:

Removed dependency on the library: node-red-contrib-config.
Removed dependency on the library: node-red-contrib-victron-modbus. I never used this library and eventually discarded it completely.
FIX: Some control elements did not react correctly to the current settings when loading the configuration.
Note: The “Surpluses: On / Off” switch is not and will not be saved in the global structure and therefore will not be saved with the configuration to prevent accidental enabling of surpluses. This is controlled by the automatic spot and limit price (trigger).
Version 29072024:

Added the Config card - from the card, the TCP settings currently work, where you enter the IP address of your PV, port, and ID, which you usually do not change. The port is default 502, and the ID is default 100. Then, click connect. Since there is an error in the modbus library, the connection status signalization does not work, and you will always see CONNECTING…
The FILE settings also work, where if you have the first installation, if a configuration file does not exist on the disk (in the ROOT directory of node-red), it will create one. You can adjust the settings parameters as you like and save them with the SAVE CONFIG button. The DELETE CONFIG button is useful when installing a new version of FLOW to load the correct default values.
Across FLOW, variables have been changed from standalone definitions to a global structure that can then be saved to disk.
Version 20072024:

FIX CCS profile - Minor detail, when closing cards, the columns would move.
FIX flow for cooling FVE - Added a 3s delay node to shift the packet.
Version 19072024:

FIX function CopyOnChange_2707 - Now, only changes are written to and sent to register 2707, not the same value.
FIX function GLOBAL FUNCTION - Functions sExtractTime and mExtractTime have the same time base using the function fSetFixedDate and cannot have different values.
The flow for cooling FVE has been mostly reworked:
Where indicators show whether the fans are running, this is a response from the SHELLY PLUGIN confirming receipt of the command. Monitoring of power consumption is not implemented.
The fan turns on immediately upon reaching the trigger value but turns off only after receiving the STOP command 20 times in a row to avoid any flickering.
Version 15072024:

FIX function convert signet to unsigned on flow battery control.
FIX function convert unsigned to signet on flow battery control.
The Spot Excess Control flow has been partially reworked. Part of the node has been moved to the function node: Logical write register 2707.
Version 14072024:

Added flow for direct control of the fan using the Shelly plugin. For your purposes, you may need to adjust or completely remove it. It is not entirely complete, especially the GUI is incomplete and impractical.
The morning peak sale function is now working; currently, the morning peak is defined for a 2-hour period.
Added test outputs; just turn on DEBUG to true in the respective node and optionally adjust the desired variable output.
Added global functions - so repeating functions are written only once and loaded in other function nodes.
Fixed time handling across the flow. From this version onwards, global variables are set on the flow GUI County_Code and TZidentifier.
Copy
// Definition of timezone and Country code
// LIST CODES: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
const sCounty_Code ="cs-CZ";
const sTZidentifier = "Europe/Prague";
Installed NODES:
node-red - 4.0.3
node-red-contrib-modbus - 5.42.0
node-red-dashboard - 3.6.5
Measure twice, cut once!
Disclaimer
The author of this project provides no warranties, express or implied, regarding the accuracy, reliability, functionality, or suitability for any purpose of this software, code, schematics, instructions, technical solutions, products, and any other materials provided. The use of this software, code, schematics, instructions, technical solutions, products, and any other materials provided is entirely at the user's own risk.

The author assumes no liability for any damages, losses, financial costs, direct or indirect damages resulting from the use of these materials, including but not limited to data loss, device damage, system failures, electrical or other installation faults, fires, loss of income, or other unforeseen consequences.

The user acknowledges that any modifications, assembly, installation, wiring, or implementation based on the provided information is done solely at their own risk. The author provides no warranties of functionality, safety, or compliance with applicable laws and regulations.

The user agrees not to pursue any legal actions against the author in connection with any damages or other claims arising from the use of this software, products, schematics, or instructions. Any legal claims against the author are hereby expressly waived and unenforceable, including through legal action.

By using these materials, the user confirms their agreement with the above conditions. If you do not agree with these terms, do not use this software, schematics, instructions, or any other provided materials.
