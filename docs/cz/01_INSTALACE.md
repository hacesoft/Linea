[🇨🇿 **Česky**](01_INSTALACE.md) | [🇬🇧 English](../en/01_INSTALLATION.md)

---

# Instalace

## Předpoklady

- funkční Node-RED;
- síťová dostupnost GX/FVE z Node-RED;
- povolený Modbus TCP;
- správné časové pásmo;
- persistentní úložiště Node-RED;
- nainstalované závislé nodes;
- případně VRM API přístup.

## 1. Node-RED

Node-RED může běžet na podporovaném GX zařízení nebo na jiném trvale dostupném zařízení. Pokud není ve stejné síti jako FVE, musí být zajištěna stabilní routovaná nebo VPN komunikace.

## 2. Persistentní úložiště

V Dockeru musí být `/data` persistentní a zapisovatelné.

```text
hostitelská složka → /data
read/write
```

## 3. Časové pásmo

Pro českou instalaci:

```text
TZ=Europe/Prague
```

Po změně Node-RED restartujte.

## 4. Závislé nodes

Nainstalujte balíčky z [přehledu závislostí](05_ZAVISLOSTI.md), nejlépe přes `Manage palette`.

## 5. Import flow

```text
Node-RED → Menu → Import → select a file → JSON flow → Import → Deploy
```

Po Deploy nesmí editor hlásit chybějící typy nodů.

## 6. Základní CONFIG

Nastavte minimálně Modbus TCP adresu, port, Unit ID, limity FVE a podle používaných funkcí VRM a polohu. Viz [Konfigurace](15_KONFIGURACE.md).

## 7. První uložení

Při první instalaci konfiguraci uložte, aby vznikl persistentní uživatelský stav.

## 8. Nezapínejte řízení bez kontroly

Nejprve pokračujte podle [Prvního spuštění](02_PRVNI_SPUSTENI.md).

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)
