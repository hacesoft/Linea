[🇨🇿 **Česky**](13_REGISTRY_A_REZIMY.md) | [🇬🇧 English](../en/13_REGISTERS_AND_MODES.md)

---

# Registry a režimy řízení

| Registr | Význam | Typ použití v LINEA |
|---:|---|---|
| 2700 | ESS Control Loop Setpoint | starší Grid Point režim |
| 2706 | Maximum System Grid Feed-In | limit exportu |
| 2707 | DC Grid Feed-In Enable | povolení DC přetoků |
| 2708 | AC Grid Feed-In Enable | AC feed-in; standardně se neřídí |
| 2716/2717 | AC Grid Set Point | aktivní externí Grid Point řízení |

## Volba 2700 / 2716–2717

Přepínač:

```text
Control Mode: ESS / AC Grid
```

Config:

```javascript
nControl_Mode_ESS_AC_Grid
```

```text
OFF → 2700
ON  → 2716/2717
```

## 2700

- INT16;
- nevolatilní uložení;
- zápis při změně;
- bez periodického heartbeat.

## 2716/2717

- INT32;
- dvě 16bitová slova;
- provozní setpoint v RAM;
- periodický heartbeat;
- bezpečný návrat setpointu při ztrátě externího řízení.

## 2706

Limit maximálního exportu do distribuční sítě. Řídicí algoritmus nesmí požadovat vyšší feed-in, než dovoluje tento limit.

## 2707

Povoluje DC feed-in. Vztahuje se na DC zdroje, například FV přes MPPT.

## 2708

Povoluje AC feed-in. Je relevantní pro zdroje na AC straně a není součástí standardního automatického řízení LINEA.

---

[← Dokumentace LINEA](README.md)
