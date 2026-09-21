# Chlazení racku a MPPT — zapojení do LINEA

Flow a úplný popis nastavení jsou v [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack).

Chlazení je samostatný projekt. Referenční flow využívá výkonové hodnoty `global.pvPower` a `global.nBattery_Power`, převod znaménka bateriového registru a HTTP příkazy relé. Při použití bez LINEA je nutné dodat odpovídající vstupy a pomocné funkce se stejnými jednotkami a významem.

Před spuštěním nastavte adresy relé, kanály, výkonové prahy, časový plán a hysterézi podle své instalace. Konkrétní hodnoty a funkce ověřte v návodu daného exportu; tento modul není obecný hotový adaptér libovolných teplotních čidel.

Příkaz OFF ani zobrazený požadovaný stav nepotvrzuje skutečné vypnutí ventilátoru. V referenčním flow nelze tlačítko `FAN ALL STOP` považovat za okamžité bezpečnostní zastavení: zpráva prochází běžnou hysterézí. Vyhodnoťte skutečnou odezvu relé a ventilátoru.

Hlavní LINEA API nemá samostatný blok `cooling` ani řídicí endpoint pro ventilátory. Případně zobrazené teploty v `temperatures` nejsou potvrzením jejich stavu.

[English](../../en/integrations/COOLING.md) · [← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
