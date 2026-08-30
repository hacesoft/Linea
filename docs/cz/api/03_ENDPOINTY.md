[🇨🇿 Česky](03_ENDPOINTY.md) | [🇬🇧 English](../../en/api/03_ENDPOINTS.md)

---

# Endpointy

## `GET /api/v1/health`

Slouží k jednoduchému ověření dostupnosti konektoru a jeho kontraktu.

```json
{
  "name": "LINEA API",
  "version": "1.0-r2.3.3",
  "schema": 3,
  "readOnly": true
}
```

`version` je verze implementace konektoru. `schema` je verze veřejného datového modelu.

## `GET /api/v1/status`

Vrací jeden konsolidovaný snapshot provozního stavu LINEA. Hlavní sekce schema 3:

```text
api
system
conventions
energy
ess
spot
forecast
solar
weather
vrm
temperatures
shelly
ups
climate
```

Volitelné moduly používají jednotný princip:

```json
{"available": true, "data": {}}
```

nebo:

```json
{"available": false, "data": null}
```

Výpadek Daikin, UPS, Shelly nebo jiného volitelného zdroje tedy neznamená selhání celého `/status`.

## Co API nemá

Hlavní API nemá řídicí endpointy. Zejména neexistuje univerzální `/set`, `/control` nebo `/write`. Pokud někdy vznikne omezené ovládání několika povolených zařízení, musí být oddělené od hlavního read-only API.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
