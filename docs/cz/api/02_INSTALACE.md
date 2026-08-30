[🇨🇿 Česky](02_INSTALACE.md) | [🇬🇧 English](../../en/api/02_INSTALLATION.md)

---

# Instalace a připojení

LINEA API běží uvnitř Node-RED vedle hlavního LINEA flow a čte jeho existující globals/snapshoty. Není samostatným řídicím systémem.

## Požadavky

- funkční LINEA v Node-RED;
- importovaný flow/modul LINEA API odpovídající verzi `1.0-r2.3.3`;
- dostupné zdrojové globals/snapshoty pro sekce, které má instalace publikovat;
- síťový přístup klienta k HTTP rozhraní Node-RED podle lokálního způsobu nasazení.

## Ověření po nasazení

Nejprve zavolejte:

```text
GET /api/v1/health
```

Očekávaný kontrakt:

```json
{"name":"LINEA API","version":"1.0-r2.3.3","schema":3,"readOnly":true}
```

Poté ověřte `GET /api/v1/status`, zejména `system.ageMs`, `system.stale`, energetické hodnoty a `available` u volitelných modulů.

## Důležité

Nedostupnost volitelného modulu nesmí shodit celé API. Klient musí umět přijmout `available: false, data: null`.

---

[← LINEA API](README.md) · [← Hlavní dokumentace](../README.md)
