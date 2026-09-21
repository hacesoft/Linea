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

Větev 2700 je ve flow určena pro zápis při změně, nikoli pro periodický heartbeat.

```text
hodnota se změnila → zapsat
hodnota je stejná → nezapisovat
```

Tím se omezují opakované zápisy stejného nastavení.

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

Tato větev je určena pro průběžně obnovované externí řízení.

## Heartbeat 2716/2717

Hodnota 2716/2717 se zapisuje periodicky i tehdy, když se požadovaný Grid Point nezměnil.

```text
500 W → 500 W → 500 W → 500 W ...
```

Opakovaný zápis funguje jako heartbeat. Při ztrátě obnovování ověřte skutečný timeout a návratový setpoint pro konkrétní GX/firmware. Předpokládaný návrat na 0 W není nezávislá bezpečnostní záruka.

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

`sendValueWithinLimits` načte hodnoty přes `parseFloat(...) || 0`, omezí záporný setpoint proti `-Math.abs(2706)` a vybere výstup. Pro 2716/2717 rozděluje hodnotu na vyšší a nižší slovo. Tato konverze není úplná validace konečnosti čísla ani INT16/INT32 rozsahu. Zvláštní význam neomezeného feed-in (`-1`) není v této funkci samostatně ošetřen. Používejte ověřený konečný limit; odečtený registr a skutečný tok kontrolujte nezávisle.

---

[← Dokumentace LINEA](README.md)
