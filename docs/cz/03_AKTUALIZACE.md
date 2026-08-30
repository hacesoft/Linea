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

Konfigurace se slučuje s výchozí šablonou. Chybějící nový klíč dostane výchozí hodnotu a existující uživatelská hodnota zůstane zachována.

## Vlastní úpravy

Lokální změny Function nodů a UI se automaticky nepřenášejí. Porovnejte je s novou verzí.

## Verze

`config.oVersion` používá formát `DDMMYYYY:HHMM`. Update checker porovnává datum před dvojtečkou s release na GitHubu.

---

[← Dokumentace LINEA](README.md) · [FAQ](04_FAQ.md) · [Technická reference](11_REFERENCE_NASTAVENI.md)
