[🇨🇿 **Česky**](05_KONVENCE_A_FRESHNESS.md) \| [🇬🇧
English](../../en/api/05_CONVENTIONS_AND_FRESHNESS.md)

# Konvence, stáří a dostupnost dat

## Výkon sítě

`gridPower` používá konvenci: **kladná hodnota = odběr ze sítě**,
**záporná hodnota = dodávka do sítě**. Jednotkou jsou watty. Stejná
konvence platí pro jednotlivé fáze v `energy.grid.phases`.

## Výkon baterie

`batteryPower` používá konvenci: **kladná hodnota = nabíjení baterie**,
**záporná hodnota = vybíjení baterie**. Jednotkou jsou watty.

## Časové údaje

`system.timestamp` je čas vytvoření odpovědi API.
`system.sourceTimestamp` je čas zdrojového snímku LINEA. `system.ageMs`
udává jeho stáří v milisekundách. `system.stale` upozorňuje, že zdrojová
data překročila povolené stáří.

Jednotlivé moduly mohou mít vlastní `updatedAt`. Proto může být hlavní
energetický stav čerstvý, zatímco například Daikin, Shelly nebo VRM byly
aktualizovány dříve. Klient má při zobrazení stáří modulu používat jeho
vlastní čas aktualizace, pokud je k dispozici.

## `available`

`available: true` znamená, že LINEA má pro daný blok použitelná data.
`available: false` znamená, že klient nemá obsah bloku považovat za
aktuálně dostupný. Neznamená to automaticky chybu celého API.

## `null` a nula

`null` znamená, že hodnota není k dispozici nebo ji zdroj neposkytl.
Nula (`0`) je platná číselná hodnota. Klient je nesmí zaměňovat.

## Volitelná pole

Externí klient má být odolný vůči chybějícím volitelným polím a novým
polím přidaným v rámci kompatibilního rozšíření. Zásadní změna významu
nebo struktury musí být vyjádřena změnou `api.schema`.

[← Datový model](04_DATOVY_MODEL.md) · [Příklady →](06_PRIKLADY.md)
