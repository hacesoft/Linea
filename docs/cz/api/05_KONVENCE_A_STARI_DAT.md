# Konvence, stáří a dostupnost dat

## Znaménka a jednotky

Síť: kladný výkon = import, záporný = export. Baterie: kladný výkon = nabíjení, záporný = vybíjení. Výkony jsou ve W, energie v kWh. Objekt `units` mapuje číselné cesty na jednotky; nejde o validátor ani úplný popis typu pole.

## Stáří hlavního stavu

`system.timestamp` je vytvoření odpovědi, `system.sourceTimestamp` vytvoření snapshotu `lineaDecisionState` v AC LOAD. `system.ageMs` je jejich časový odstup vůči serverovým hodinám, záporný rozdíl je oříznut na nulu. `/status` označuje stáří **> 30 000 ms**, `/health` stáří **> 5 000 ms**. Neplatný čas vede na `null` stáří a `stale: true`.

Čerstvý AC LOAD může obsahovat dříve uložené náhradní hodnoty. Hlavní timestamp tedy nepotvrzuje poslední fyzické měření každého registru. HTTP 200 není potvrzení čerstvosti ani zdraví zařízení.

## Moduly a `available`

Obálka volitelného modulu hodnotí přítomnost uloženého objektu. `available: true` neznamená online ani čerstvá data. Sledujte vlastní `updatedAt` nebo `lastSeen`; chybí-li, stáří je neznámé. Nejnovější `shelly.data.updatedAt` může patřit jen jedné části Shelly, ne všem zařízením. Uložené `ageSec` kouřového čidla se nemusí přepočítat při každém HTTP požadavku; pro průběžné stáří použijte `lastSeen`.

`energy.available` je u úspěšné odpovědi vždy `true`. `forecast.available` testuje `success === true`, `spot.available` přítomnost ceny nebo cenového pole. Ani jeden příznak sám nekontroluje stáří či úplnost dat. Jednotná per-module freshness smlouva zde není.

## Nula a chybějící hodnota

Klient musí zachovat rozdíl mezi nulou a `null`. Implementace však má omezení: `finiteOrNull` používá `Number(v)`, takže `null`, prázdný řetězec a `false` mohou skončit jako **0**. AC LOAD navíc u některých vstupů dosazuje dřívější hodnotu nebo nulu. Nelze proto tvrdit, že každá nula je ověřené fyzické měření.

## Časové řady

SPOT pole je mapováno indexem na `hour`, s `intervalMinutes: 60` a datem ze systémového lokálního času. API negarantuje 24 položek ani neřeší zvlášť 23/25hodinový den. Zkontrolujte datum, délku a skutečný zdrojový interval před výpočtem nákladů. Forecast deklaruje 15 minut, převádí Wh na kWh a nevkládá chybějící body.

## Kompatibilita

Kontrolujte `api.schema === 1`, tolerujte nová pole a chybějící volitelné hodnoty. Změna významu nebo struktury může vyžadovat nové schema. Název `vrm.data.today` nemění fakt, že bateriové čítače jsou kumulativní od resetu, nikoli denní.


[English](../../en/api/05_CONVENTIONS_AND_FRESHNESS.md) · [← LINEA API](PREHLED.md) · [← LINEA](../README.md)
