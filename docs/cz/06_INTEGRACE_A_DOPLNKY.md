# Volitelné moduly a integrace

LINEA CORE zajišťuje řízení FVE/ESS. Shelly, Daikin, UPS a chlazení jsou volitelné moduly se samostatnými repozitáři. Aktuální flow, závislosti, instalaci a úplný návod hledejte vždy v repozitáři modulu. Zde jsou popsány vazby na LINEA.

| Modul | Repozitář a úplný návod | Zapojení do LINEA |
|---|---|---|
| Shelly | [node-red-shelly](https://github.com/hacesoft/node-red-shelly) | [Zapojení](integrace/SHELLY.md) |
| Daikin ONECTA | [node-red-daikin](https://github.com/hacesoft/node-red-daikin) | [Zapojení](integrace/DAIKIN.md) |
| Eaton UPS / NUT | [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups) | [Zapojení](integrace/UPS.md) |
| Chlazení / Cooling | [Cooling_Trackers_Rack](https://github.com/hacesoft/Cooling_Trackers_Rack) | [Zapojení](integrace/COOLING.md) |

## Import a aktualizace

Zálohujte flow i konfiguraci. Nejprve ověřte, zda daný modul už není součástí vašeho LINEA exportu. Nahraďte existující modul; nespouštějte dvě kopie současně. Duplicitní MQTT odběry, NUT dotazy, cloudové požadavky nebo příkazy zařízením se mohou navzájem ovlivňovat. Před Deploy zkontrolujte sdílené Dashboard stránky/skupiny, konfigurační uzly, link uzly, globals a cesty k souborům. Samostatný provoz vyžaduje závislosti popsané v návodu modulu; samotný import JSON nedoplní chybějící pomocné funkce LINEA.

## Nextcloud monitoring — ve vývoji

Plánovaná aplikace LINEA Monitor & Analytics bude číst [LINEA API](api/PREHLED.md). Adresa jejího repozitáře ani náhledový obrázek zatím nejsou součástí tohoto balíčku dokumentace. Instalace ani sběr historie zde nejsou k dispozici. Viz [plánovaný rozsah](api/07_NEXTCLOUD_A_BUDOUCNOST.md).

## Související projekty
- [LM335](https://github.com/hacesoft/LM335)
- [Clever Boiler](https://github.com/hacesoft/Clever_boiler)
- [Modbus Scanner](https://github.com/hacesoft/Scanner_ModBus)
- [7-inch display for Victron](https://github.com/hacesoft/7_inch_display_for_victron)

[← LINEA](README.md)
