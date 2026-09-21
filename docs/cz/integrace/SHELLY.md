# Shelly — zapojení do LINEA

Modul pro Dashboard 2.0 monitoruje a ovládá Shelly Gen2+ přes MQTT. Obsahuje přepínací kanály, kouřová čidla a notifikace ntfy. Úplný návod a export jsou v [node-red-shelly](https://github.com/hacesoft/node-red-shelly).

## MQTT broker se nastavuje ručně ve flow

IP adresu MQTT brokeru **nelze v tomto modulu změnit z Dashboardu LINEA**. Na rozdíl od Modbus konfigurace zůstává MQTT spojení v konfiguračním uzlu Node-RED. Pokusy o přepínání brokeru z UI vedly v tomto projektu k nestabilitě.

1. V editoru Node-RED najděte skupinu `SHELLY :: MQTT` nebo přes hledání uzel `MQTT Switch events`.
2. Otevřete MQTT vstup a u položky **Server** klikněte na tužku.
3. V dodaném exportu je broker pojmenovaný `NAS_docker_mqtt`. Upravte **Server/host** na IP nebo DNS svého brokeru a **Port** (běžně 1883 pro připojení bez TLS).
4. Podle brokeru nastavte přihlašovací údaje v **Security** a případně TLS. Název uzlu je pouze popisek, jeho změna adresu nepřepíše.
5. Zkontrolujte, že výstup `MQTT Switch RPC` používá stejný broker. Změna sdíleného config uzlu ovlivní všechny jeho uživatele.
6. Potvrďte **Update/Done → Deploy**. U MQTT uzlů ověřte připojení a příjem zpráv.
7. Stejný broker nastavte také v jednotlivých Shelly zařízeních. IP zařízení v seznamu Shelly není IP brokeru.

Seznam zařízení používá `global.shellyDevices`. Při importu zachovejte napojení ukládání konfigurace a inicializace. Veřejný API stav vzniká v `global.lineaApiShellyState` a `global.lineaApiShellySmokeState` a je dostupný v `shelly` na `/api/v1/status`.

API předává uložený stav. `available` není potvrzení aktuálního síťového spojení; stáří kouřového čidla vyhodnocujte také z `lastSeen`. Podrobnosti zařízení, MQTT topics a samostatného provozu patří do návodu modulu.

[English](../../en/integrations/SHELLY.md) · [← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
