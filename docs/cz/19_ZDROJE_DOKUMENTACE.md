[🇨🇿 **Česky**](19_ZDROJE_DOKUMENTACE.md) | [🇬🇧 English](../en/README.md)

---

# Zdroje a pravidla dokumentace

## Účel

Technická dokumentace popisuje **aktuální funkci LINEA**.

Neobsahuje historii vývoje, informace o auditu ani popis toho, jak se konkrétní funkce chovala v předchozích verzích.

## Zdroj pravdy

Pro popis chování se používá:

1. aktuální Node-RED flow;
2. nápověda přímo v aktuálním Dashboardu;
3. veřejná dokumentace projektu LINEA jako doplňující vysvětlení účelu funkcí.

Pokud se text staršího README liší od aktuální implementace, dokumentace popisuje aktuální implementaci.

## Historie změn

Informace typu:

- opraveno;
- změněno;
- dříve;
- původně;
- od verze;
- předchozí chování;

patří pouze do `CHANGELOG.md`.

## Styl technické dokumentace

Každá funkce má být popsána pokud možno v pořadí:

```text
účel
→ vstupy
→ výpočet / rozhodnutí
→ výstup
→ vazby na jiné funkce
→ fail-safe
```

Dokumentace má vysvětlovat nejen název přepínače, ale také jeho skutečný vliv na tok dat a řídicí algoritmus.
