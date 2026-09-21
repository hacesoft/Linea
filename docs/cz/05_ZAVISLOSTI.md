[🇨🇿 **Česky**](05_ZAVISLOSTI.md) | [🇬🇧 English](../en/05_DEPENDENCIES.md)

---

# Závislosti Node-RED

## Základ

Aktuální flow používá Node-RED, FlowFuse Dashboard 2.0 a Modbus nodes.

V exportu se vyskytují mimo jiné tyto typy:

```text
modbus-client
modbus-flex-connector
modbus-flex-getter
modbus-queue-info
modbus-read
modbus-response
modbus-write
mqtt in
mqtt out
mqtt-broker
ui-base
ui-control
ui-group
ui-notification
ui-page
ui-template
ui-theme
```

## Instalace

```text
Node-RED → Menu → Manage palette → Install
```

Pro Modbus se používá balík `node-red-contrib-modbus`.

## Dashboard

Nová dokumentace je určena pro Dashboard 2.0. Starý `node-red-dashboard` není cílovou UI platformou nové verze.

## Unknown node po importu

Doinstalujte balíček odpovídající chybějícímu typu nodu a podle potřeby restartujte Node-RED.

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)

## Balíčky v referenčním exportu

- `@flowfuse/node-red-dashboard`: 1.30.2
- `node-red-contrib-modbus`: 5.45.2

Jde o verze uvedené v exportu, nikoli o tvrzení o nejnovější verzi nebo minimální kompatibilitě. MQTT, HTTP, TCP, Function a File uzly jsou součástí Node-RED. Závislosti samostatných modulů kontrolujte v jejich repozitářích.
