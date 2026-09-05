[🇨🇿 Česky](07_NEXTCLOUD_A_BUDOUCNOST.md) | [🇬🇧 English](../../en/api/07_NEXTCLOUD_AND_FUTURE.md)

---

# Nextcloud, historie a budoucí rozšíření

## Nextcloud LINEA Monitor & Analytics

Plánovaná aplikace je read-only monitorovací a analytická vrstva se třemi základními oblastmi:

```text
LIVE      → aktuální /api/v1/status
HISTORY   → background collector + databáze
ANALYTICS → grafy, statistiky, ekonomika a korelace
```

Browser nemá být odpovědný za dlouhodobý sběr. Snapshoty má pravidelně získávat background job Nextcloudu a ukládat je do vlastní databáze.

## Historie a agregace

Pracovní koncept: krátká historie ve vysokém rozlišení, delší historie v 15min agregacích, po přibližně týdnu hodinové agregace a dlouhodobě denní agregace. Pro analogové hodnoty se předpokládá `avg/min/max/sample count`, pro výkon integrace v čase na kWh a pro stavy změny ON/OFF, doba ve stavu a události. Finální retention policy se určí při implementaci databáze.

## Budoucí obnova bateriových čítačů

`nBatteryALL_input_Wh` a `nBatteryALL_output_Wh` se po restartu Node-RED vynulují. Budoucí databázová služba Nextcloud má umožnit jednorázovou obnovu posledních uložených hodnot po čerstvém startu LINEA. Restore musí být chráněn interním příznakem a nesmí se opakovat během běžného provozu.

Tato obnova **není součástí API 1.0.0**.

## Co nyní neměnit

Bez konkrétní potřeby klienta se nemá vytvářet další revize jen kvůli kosmetice. Odložené jsou zejména dlouhodobá historie, databázové agregace, restore bateriových čítačů, obecný per-module freshness, ekonomické statistiky, event timeline a korelace.

## Případné budoucí ovládání Shelly

Hlavní LINEA API zůstává read-only. Pokud bude někdy realizováno velmi omezené ovládání vybraných Shelly zařízení, má vzniknout **oddělené Control API** s explicitním allowlistem. Nemá existovat univerzální ovládání libovolného Shelly. ESS, HDO, baterie, měniče a bezpečnostní/řídicí prvky se tímto rozhraním zpřístupňovat nemají. Výsledný stav se má po příkazu ověřovat přes read-only `/api/v1/status`.

---

[← LINEA API](PREHLED.md) · [← Hlavní dokumentace](PREHLED.md)

## Změnové ukládání pomalu se měnících stavů
Pro UPS a podobné stavové bloky není účelné ukládat každých několik sekund identickou kopii. Při nezměněném stavu může Nextcloud prodloužit `valid_to` posledního záznamu a nový řádek vytvořit až při změně. Historický stav se rekonstruuje podle `valid_from`–`valid_to`; události a rychlé energetické veličiny mohou mít jinou retenční politiku.
