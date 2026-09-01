[🇨🇿 Česky](../../cz/integrace/DAIKIN.md) | [🇬🇧 **English**](DAIKIN.md)

---
# Daikin Onecta module

The optional module reads HVAC state from Daikin Onecta Cloud API using OAuth 2.0 Authorization Code flow. It is not required by LINEA CORE. The working flow is still evolving, so this page documents the current operating principle rather than final internal wiring.

Current endpoints are `https://idp.onecta.daikineurope.com/v1/oidc/authorize` for authorization, `https://idp.onecta.daikineurope.com/v1/oidc/token` for token exchange/refresh, and `https://api.onecta.daikineurope.com/v1/gateway-devices` for devices. Older endpoint examples do not match the current implementation.

For initial authorization, create a Daikin developer application, register the exact Redirect URI, save Client ID/Secret, authorize with scope `openid onecta:basic.integration`, copy the returned `code`, and exchange it at the token endpoint using `grant_type=authorization_code`. Store the returned refresh token securely. Refresh tokens rotate: a newly returned refresh token must replace the previous one. `global.daikinRefreshInFlight` prevents overlapping refreshes.

Normal settings are intended to live in `global.config.daikinConfig`; tokens are operational secrets with a separate lifecycle. The intended polling minimum is **8 minutes**. A reduced `global.lineaApiClimateState` exposes monitoring data only and never credentials or tokens.

Troubleshooting: `waiting for Client ID/Secret` means the active global config lacks credentials; a stored refresh token plus expired access token still requires valid Client ID/Secret for renewal.

---
[← Integrations](../06_INTEGRATIONS_AND_TOOLS.md)
