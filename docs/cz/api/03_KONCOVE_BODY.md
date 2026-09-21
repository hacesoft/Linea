# Koncové body

## `GET /api/v1/health`

Kontroluje existenci hlavního stavu, nikoli zdraví všech zařízení. Vrací HTTP **200**, pokud existuje pravdivý `global.lineaDecisionState`, jinak **503**. `status: "ok"` může být současně se `stale: true`.

```json
{
  "api": {
    "name": "LINEA API",
    "version": "1.0.0",
    "schema": 1,
    "readOnly": true
  },
  "status": "ok",
  "sourceAgeMs": 1000,
  "stale": false,
  "timestamp": "2026-09-21T12:00:00.000Z"
}
```

`sourceAgeMs` je stáří zdrojové časové značky. `stale` je `true`, pokud stáří nelze určit nebo je **větší než 5 000 ms**. Tělo 503 má stejná pole, `status: "initializing"`, `sourceAgeMs: null` a `stale: true`.

## `GET /api/v1/status`

Vrací HTTP **200** s bloky `api`, `system`, `units`, `conventions`, `energy`, `ess`, `spot`, `forecast`, `solar`, `weather`, `vrm`, `temperatures`, `shelly`, `ups`, `climate`. API má **version 1.0.0, schema 1**.

`system.stale` je `true`, pokud stáří nelze určit nebo je **větší než 30 000 ms**. Starý snapshot stále vrací HTTP 200. API tím nepotvrzuje úspěšný zápis Modbus ani čerstvost jednotlivých senzorů.

Pokud hlavní snapshot nebo jeho `timestamp` chybí, vrací HTTP **503**:

```json
{
  "api": {
    "name": "LINEA API",
    "version": "1.0.0",
    "schema": 1,
    "readOnly": true
  },
  "status": "initializing",
  "timestamp": "2026-09-21T12:00:00.000Z"
}
```

Odpověď 503 nemá `system`, `units` ani energetické bloky. `/status` nastavuje `Cache-Control: no-store`; `/health` tento header v referenční funkci nenastavuje.

`vrm`, `temperatures`, `shelly`, `ups` a `climate` používají obálku `{available, data}`. Ostatní sekce mají vlastní strukturu. `available` samo o sobě nezaručuje čerstvost ani online zařízení. Nepřítomný volitelný modul běžně neblokuje celý status.

## Rozsah

Tyto endpointy jsou pouze pro čtení. Neexistuje zde `/set`, `/control`, `/write` ani endpoint historie. Cesta je relativní k HTTP kořeni Node-RED; prefix reverse proxy nebo `httpNodeRoot` ji může změnit.


[English](../../en/api/03_ENDPOINTS.md) · [← LINEA API](PREHLED.md) · [← LINEA](../README.md)
