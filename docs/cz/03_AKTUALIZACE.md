[🇨🇿 **Česky**](03_AKTUALIZACE.md) | [🇬🇧 English](../en/03_UPDATING.md)

---

# Aktualizace LINEA

## Před aktualizací

Zálohujte flow i konfiguraci, poznamenejte vlastní úpravy a přečtěte [CHANGELOG](../../CHANGELOG.md).

## Postup

1. Dočasně deaktivujte aktivní externí řízení.
2. Exportujte stávající flow jako zálohu.
3. Zajistěte, aby současně neběžela stará a nová řídicí větev.
4. Importujte novou verzi.
5. Proveďte Deploy.
6. Ověřte načtení konfigurace.
7. Zkontrolujte nové položky v CONFIG.
8. Ověřte komunikaci, limity a Control Mode.
9. Znovu aktivujte automatické strategie.

## Konfigurace mezi verzemi

Po načtení porovnejte uloženou konfiguraci s výchozí šablonou nové verze. Referenční `Parse Config` načtený objekt přímo vloží do `global.config`; automatické sloučení všech nových klíčů není zaručeno. Chybějící položky doplňte přes podporované nastavení a konfiguraci znovu uložte.

## Vlastní úpravy

Lokální změny Function nodů a UI se automaticky nepřenášejí. Porovnejte je s novou verzí.

## Verze

`config.flow_version` používá formát `DDMMYYYY_HHMM`. Inicializace jej předává do `global.linea_version_local`. Update checker porovnává datum **i čas** se suffixem názvů JSON/ZIP souborů v `release/`, například `LINEA_flows_15092026_1757.json`. Staré `oVersion` se ještě může objevit ve stavovém popisku parseru, není však zdrojem aktuálního update checkeru.

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)
