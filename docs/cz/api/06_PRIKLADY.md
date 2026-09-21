# Příklady API

Následující soubory jsou demonstrační odpovědi vytvořené spuštěním referenčních API funkcí nad umělými vstupy. Nejsou to měření instalace.

- [health.json](../../examples/health.json)
- [status.json](../../examples/status.json)
- [health-initializing.json](../../examples/health-initializing.json)
- [status-initializing.json](../../examples/status-initializing.json)

```bash
curl -i 'http://LINEA_HOST:1880/api/v1/health'
curl -i 'http://LINEA_HOST:1880/api/v1/status'
```

Úspěšný status obsahuje `units` a schema 1. Při 503 zpracujte `status: "initializing"`; nepřistupujte k neexistujícímu `system`. I při HTTP 200 čtěte příznaky stáří. Bez vlastního `updatedAt` nelze stáří modulu odvodit z hlavního snapshotu.

[English](../../en/api/06_EXAMPLES.md) · [← LINEA API](PREHLED.md) · [← LINEA](../README.md)
