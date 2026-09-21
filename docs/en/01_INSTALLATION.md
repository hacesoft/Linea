[Česky](../cz/01_INSTALACE.md) | [English](01_INSTALLATION.md)

# Installation

1. Prepare a continuously available Node-RED host with network access to the Victron GX device. Enable Modbus TCP on the GX and check routing/firewall access.
2. Provide writable persistent storage. For Docker, persist `/data`; preserve both flow files and application configuration files.
3. Set the correct server timezone, for example `TZ=Europe/Prague`, and restart Node-RED after changing it.
4. Install the [required packages](05_DEPENDENCIES.md) through **Menu → Manage palette → Install**.
5. Back up existing flows. Import the LINEA JSON through **Menu → Import → select a file**. Do not run old and new control branches in parallel.
6. Before Deploy, review imported Modbus write nodes and prevent writes until your addresses and limits are checked. Default values belong to the reference installation. The ESS/AC Grid switch chooses a target register; OFF does not disable control.
7. Configure the installation, save its settings, then follow [First start](02_FIRST_START.md).

Node-RED can run on a suitable GX installation or a separate host. If it is remote, provide reliable routed/VPN connectivity. Configure Modbus host, port and service Unit IDs for your hardware; do not assume one Unit ID applies to every service. Add VRM access and location where those functions are used.

Optional modules have their own [repositories and manuals](06_INTEGRATIONS_AND_TOOLS.md). Check whether the full LINEA export already contains them before importing another copy.

[← Documentation](README.md)
