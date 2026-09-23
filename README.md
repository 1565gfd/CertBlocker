# CertBlocker

Утилита для Windows, помечающая выбранный сертификат недоверенным: добавляет его
в системное хранилище **Untrusted Certificates** (`LocalMachine\Disallowed`). После
этого сертификату перестают доверять Windows и приложения, использующие системное
хранилище — Microsoft Edge, Google Chrome, Internet Explorer, .NET. Firefox имеет
собственное хранилище и на эту настройку не влияет.

![platform](https://img.shields.io/badge/platform-Windows%207–11-0078D6?logo=windows&logoColor=white)
![framework](https://img.shields.io/badge/.NET%20Framework-4.x-512BD4)
![admin](https://img.shields.io/badge/admin-required-E67E22)
![license](https://img.shields.io/badge/license-MIT-2ECC71)

![Скриншот](docs/screenshot.png)

## Возможности

- Блокировка сертификата из файла (`.cer`, `.crt`, `.der`, `.pem`).
- Поиск установленных корневых сертификатов и блокировка выбранного.
- Просмотр и разблокировка ранее заблокированных сертификатов.
- Экспорт сертификата в `.cer`.

## Требования

- Windows 7–11.
- Права администратора (запись в хранилище `LocalMachine`).
- .NET Framework 4.x (входит в состав Windows).

## Сборка

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

Результат — `build\CertBlocker.exe`. Отдельный SDK не требуется: используется
компилятор `csc.exe` из состава .NET Framework.

## Проверка целостности

```powershell
Get-FileHash -Algorithm SHA256 .\CertBlocker.exe
```

SHA-256 релиза **v1.0.3**:

```
434DCEFD112677ABF40F56250D59C90AAE62E0E38883E8EAA651B5C9090FC8DC
```

## Примечания

- Приложение не подписано сертификатом разработчика — при первом запуске возможно
  предупреждение SmartScreen.
- Единственное изменение в системе — запись в хранилище `Disallowed`, обратимая
  кнопкой «Разблокировать».
- Приложение не использует сеть и не запускает внешних процессов. Защита от подмены
  DLL; при загрузке файла используется только публичная часть сертификата.

## Разблокировка вручную

`certmgr.msc` → «Сертификаты, к которым нет доверия» → удалить запись.

## Версия без прав администратора

[CertBlocker-NoAdmin](https://github.com/1565gfd/CertBlocker-NoAdmin) — блокировка
для текущего пользователя, без UAC.

## Лицензия

[MIT](LICENSE)
