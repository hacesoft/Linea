[🇨🇿 **Česky**](UPS.md) | [🇬🇧 English](../../en/integrations/UPS.md)

---
# UPS / NUT modul

Úplný návod, nastavení NUT, instalace a aktuální flow: [node-red-eaton-ups](https://github.com/hacesoft/node-red-eaton-ups).

## 1. Účel
Aktivní volitelný modul poskytuje read-only monitoring UPS přes Network UPS Tools (NUT). LINEA CORE na něm není závislá. Komunikuje přímo s `upsd` přes TCP příkazem `LIST VAR <UPS_NAME>`. Starší disabled UPS větev ponechaná ve vývojovém flow tato dokumentace nepovažuje za podporovanou implementaci.

## 2. Konfigurace
Konfigurace je v `global.config.upsConfig`:
```javascript
upsConfig: { host: "192.168.x.x", port: 3493, upsName: "UPS", timeoutMs: 4000, ntfyUrl: "" }
```
`upsName` je case-sensitive. Prázdné `ntfyUrl` notifikace vypíná. Aktivní modul nemá uživatelský `enabled` ani nastavitelný polling. Dashboard umí otestovat právě vyplněný host/port/UPS name ještě před uložením.

## 3. Periodické čtení a watchdog
LINEA dotazuje NUT přibližně každých **5 s**. To není frekvence fyzické aktualizace UPS: NUT driver má vlastní `pollinterval` a `pollfreq`, takže stejné hodnoty v několika odpovědích jsou normální. `NUT watchdog` sleduje poslední úspěšnou odpověď a po více než **15 s** bez úspěchu hlásí `NUT OFFLINE`. Sleduje komunikaci, nikoli změnu hodnot.

## 4. Data a události
Parser zpracovává `VAR <UPS_NAME> <key> "<value>"` a vytváří `global.ups`: výrobce/model/firmware, status, baterie, runtime, napětí baterie, load, reálný a jmenovitý výkon, vstupní/výstupní napětí, frekvence a timestamp. Známé statusy zahrnují `OL`, `OB`, `LB`, `CHRG`, `DISCHRG`, `OVER`, `RB`, `BYPASS`. Události zahrnují minimálně `POWER_LOST`, `POWER_RESTORED`, `BATTERY_LOW`.

## 5. API a budoucí historie
Pro veřejné read-only API vzniká redukovaný `global.lineaApiUpsState`. Dlouhodobá historie patří do Nextcloud databáze. U pomalu se měnícího UPS stavu je vhodné změnové ukládání: shodný stav prodlouží `valid_to`; nový řádek vznikne až při změně.

## 6. Troubleshooting
Ověřte host/port, přesný case-sensitive UPS name, dostupnost `upsd`, odpověď `LIST VAR <UPS_NAME>` a timeout. `NUT OFFLINE` znamená chybějící úspěšnou odpověď déle než 15 s, nikoli pouze nezměněné hodnoty.

---
[← Integrace](../06_INTEGRACE_A_DOPLNKY.md)
