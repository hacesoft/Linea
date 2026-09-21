# Using LINEA API

## Availability and access

The API is included in the supplied LINEA flow; there is no separate installation package. Find the HTTP In nodes `GET /api/v1/health` and `GET /api/v1/status` and verify their Function → HTTP Response connections. Check the imported contents rather than inferring availability from the export filename date.

Clients need access to Node-RED's HTTP interface. Replace the example `LINEA_HOST:1880` and add any configured prefix. For Nextcloud, the URL must be reachable from the Nextcloud server/container, not only from a desktop browser.

```bash
curl -i 'http://LINEA_HOST:1880/api/v1/health'
curl -i 'http://LINEA_HOST:1880/api/v1/status'
```

Check HTTP status first, then `api.schema`, freshness and individual section availability. Initialization HTTP 503 has a different JSON shape from a successful status. Display unavailability and retry with a delay; do not substitute zero for missing measurements.

## Access protection

The two exported branches have no built-in API-key or authentication node. Protect HTTP endpoints through Node-RED settings, a reverse proxy, or trusted network/VPN access. Editor login alone does not establish that these endpoints are protected. The export does not include complete server settings; verify the deployed protection.

Use HTTPS for remote access. Do not expose an unprotected Node-RED port. Do not assume CORS is configured. API clients do not need the VRM token, Daikin Client Secret or MQTT password.

## Collection

Each request returns a snapshot. There is no WebSocket/SSE stream or built-in archive. Choose polling according to display needs and load; faster requests do not refresh cloud data faster. Store history on the client side and record collection gaps.


[Česky](../../cz/api/02_ZPROVOZNENI_A_POUZITI.md) · [← LINEA API](README.md) · [← LINEA](../README.md)
