[Česky](../cz/03_AKTUALIZACE.md) | [English](03_UPDATING.md)

# Updating LINEA

1. Back up the current flow, configuration and module files; read the [changelog](../../CHANGELOG.md).
2. Stop active external writes and ensure the old and new controller cannot run concurrently.
3. Import the new version and review addresses, shared configuration nodes, file paths and module connections.
4. Deploy, verify loaded settings, then compare them with the new defaults.
5. Check communication, limits, mode and a small test setpoint before re-enabling strategies.

The reference `Parse Config` assigns the loaded object directly to `global.config`; a complete merge with new defaults is not guaranteed. Add missing settings through supported configuration and save again. Local Function/UI edits do not automatically transfer.

`config.flow_version` uses `DDMMYYYY_HHMM`. Initialization copies it to `global.linea_version_local`. The checker compares both date and time against the suffix of JSON/ZIP filenames in `release/`. The old `oVersion` may still appear in a parser status label but is not the active checker source.

Update [optional modules](06_INTEGRATIONS_AND_TOOLS.md) from their repositories without duplicating existing branches.

[← Documentation](README.md)
