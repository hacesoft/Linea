# Daikin ONECTA — LINEA integration

Use [node-red-daikin](https://github.com/hacesoft/node-red-daikin) for registration, application creation, initial OAuth authorization, tokens, installation and troubleshooting. The optional Dashboard 2.0 module reads device state, temperatures and available energy data from the ONECTA Cloud API.

Normal settings use `global.config.daikinConfig`: `clientId`, `clientSecret`, `pollMinutes`. Set polling to **8 minutes**; the interval is configurable with an 8-minute minimum. Tokens have a separate lifecycle and must not be published in flow exports or the API.

Preserve initialization, token-manager and persistence connections. Check `daikin_config.json`, the token file, permissions and restart recovery using the module manual. Do not assume the generic LINEA save action writes every standalone module file.

`global.lineaApiClimateState` supplies `climate.data`. Check its `updatedAt` independently of the main energy snapshot. A reported operation mode does not prove that the compressor is running. For authorization failures check Client ID/Secret, identical Redirect URIs and the latest rotating refresh token.

[Česky](../../cz/integrace/DAIKIN.md) · [← Integrations](../06_INTEGRATIONS_AND_TOOLS.md)
