# FloV:MP — установка сервера на Windows

Версия 1.0.4 (стабильная). Нужен ключ лицензии (выдаётся при покупке, вид
`FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX`).

---

## Способ 1. Одной командой

Откройте PowerShell, перейдите в папку, где будет жить сервер, и выполните
одну команду — подставив свой ключ:

```powershell
cd C:\FloVMP
powershell -ExecutionPolicy Bypass -Command "iwr https://github.com/shizeexgod/FloV-MP-releases/releases/latest/download/get.ps1 -OutFile get.ps1; .\get.ps1 -GitHub shizeexgod/FloV-MP-releases -Key FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX"
```

Что произойдёт: скачается описание последнего релиза, проверится его подпись
ключом FloV:MP, скачается пакет сервера, сверится SHA-256, и сервер
установится **в текущую папку** (ту, в которую вы зашли через `cd`).

Обновление потом — та же команда без `-Key`:

```powershell
cd C:\FloVMP
.\get.ps1 -GitHub shizeexgod/FloV-MP-releases
```

Ключ возьмётся из установленного `config\flovmp.env`. Если установлена
последняя версия, ничего не скачивается.

### Бета-версия

Стабильная версия ставится командой выше. Чтобы поставить бету (в ней новое
меню Esc, серверный подсчёт урона, автопереподключение), добавьте `-Tag`:

```powershell
.\get.ps1 -GitHub shizeexgod/FloV-MP-releases -Tag v1.0.5-beta -Key FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
```

## Способ 2. Архивом, в один клик

Архив `flovmp-setup.zip` лежит в корне этого репозитория и среди файлов
релиза.

1. Распакуйте **весь** архив `flovmp-setup.zip` в папку будущего сервера.
2. Двойной щелчок по `УСТАНОВИТЬ.cmd`.
3. Введите ключ лицензии.

Второй запуск того же файла — обновление, ключ и папка уже сохранены.

## Способ 3. Из полного пакета (без интернета на сервере)

Скачайте `flovmp-server-1.0.4-windows.zip`, распакуйте во временную папку и
из неё выполните:

```powershell
powershell -ExecutionPolicy Bypass -File .\install.ps1 -InstallDir "C:\FloVMP" -LicenseKey FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
```

---

## После установки

1. Запустите `FloVMP-Server.exe`. При первом запуске он создаст настройки,
   активирует ключ и создаст папку `gamemode` для вашего кода.
   В окне появится строка `[License] лицензия FLV-...`.
2. Порты для игроков: **7788** (TCP+UDP) игра, **7798** (TCP+UDP) клиенты
   GTA V Legacy 1.0.3889.0 и голос, **7895** (UDP) голос.
3. Клиент игроку: папка `client-b3889` — `install-client.cmd` ставит клиент в
   GTA V, `play.cmd` заходит на сервер.
4. Настройки сервера: `server\server.toml` (имя, порт, слоты) и
   `server\config\client.cfg` (61 параметр: мир, транспорт, чат, интерфейс,
   ники, голос, клавиши). Меняются на ходу командой `reloadsettings`.
5. Свой код сервера: папка `gamemode` (C#), справочник — `gamemode\README.md`
   и `sdk\template`. Сборка: `gamemode\build.cmd`.

## Что обновление не трогает

`config\flovmp.env`, `license.flv`, `server\server.toml`, `server\config\*`
(включая `client.cfg`, `admins.json`, `admin-commands.cfg`, `maps`), папку
`gamemode`, ваши ресурсы в `server\resources`, миграции с номера 100, базу и
резервные копии. Перед заменой файлов платформы создаётся резервная копия
рядом с папкой сервера, при ошибке установка откатывается.

## Если в папке уже есть другой сервер

Установщик сравнивает имена файлов. Совпадений нет — FloV:MP встанет рядом и
чужое не тронет. Совпадения есть — установка остановится и покажет список,
ничего не изменив; продолжить, сохранив чужие файлы в резервную копию, можно
ключом `-Force`.

## Поддержка

Вопросы и продление лицензии — продавцу лицензии.
