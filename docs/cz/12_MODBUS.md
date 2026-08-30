[🇨🇿 **Česky**](12_MODBUS.md) | [🇬🇧 English](../en/12_MODBUS.md)

---

# Modbus komunikace

## Úloha Modbus vrstvy

Modbus TCP zajišťuje čtení provozních hodnot FVE/ESS a zápis řídicích registrů.

## Řídicí režimy

LINEA podporuje dva způsoby zápisu Grid Pointu.

### Registr 2700

```text
ESS Control Loop Setpoint
INT16
```

Používá se při:

```text
Control Mode: ESS / AC Grid = OFF
```

Hodnota registru 2700 je ukládána v nevolatilní interní paměti zařízení. Proto se stejná hodnota neposílá periodicky.

```text
hodnota se změnila → zapsat
hodnota je stejná → nezapisovat
```

Tím se omezuje zbytečné zapisování do FLASH/EEPROM.

### Registry 2716/2717

```text
AC Grid Set Point
INT32
```

Používají se při:

```text
Control Mode: ESS / AC Grid = ON
```

32bitová hodnota je přenášena ve dvou 16bitových registrech.

Tento setpoint je provozní hodnota v RAM a je určen pro aktivní externí řízení.

## Heartbeat 2716/2717

Hodnota 2716/2717 se zapisuje periodicky i tehdy, když se požadovaný Grid Point nezměnil.

```text
500 W → 500 W → 500 W → 500 W ...
```

Opakovaný zápis funguje jako heartbeat. Pokud externí controller přestane hodnotu obnovovat, firmware měniče po timeoutu vrátí setpoint do bezpečného stavu, typicky 0 W.

Na této větvi proto nesmí být použito RBE ani jiné potlačení opakovaných hodnot.

## Změna Control Mode

Při přepnutí mezi 2700 a 2716/2717 se aktuální setpoint odešle do nově zvoleného cíle bez ohledu na to, zda se jeho číselná hodnota změnila.

## Registr 2706

`Maximum System Grid Feed-In` určuje maximální povolený export. Požadovaný Grid Point je proti tomuto limitu kontrolován před zápisem.

`nMAX_Grid_Point` a registr 2706 nejsou totéž:

```text
nMAX_Grid_Point → výkon požadovaný strategií
2706            → horní limit povoleného feed-in
```

## Registr 2707

Řídí povolení DC feed-in. Týká se zejména DC FV zdrojů/MPPT.

## Registr 2708

Řídí AC feed-in. Základní LINEA řízení jej standardně nepoužívá.

## Validace zápisu

Před zápisem se kontroluje:

- platnost čísla;
- `NaN`;
- rozsah;
- znaménko;
- cílový datový typ;
- feed-in limit;
- aktivní Control Mode.

---

[← Dokumentace LINEA](README.md)
