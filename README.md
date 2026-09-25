# FloV:MP — установка сервера

Актуальная версия — **1.0.8 Beta** ([что нового](https://github.com/shizeexgod/FloV-MP-releases/releases/latest)).
Команды ниже всегда ставят последнюю версию. Нужен ключ лицензии (выдаётся
при покупке, вид `FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX`).

- [Windows](#windows)
- [Linux (Ubuntu / Debian)](#linux-ubuntu--debian)

# Windows

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

### Конкретная версия

Поставить именно определённый релиз (например, чтобы вернуться на прошлый) —
добавьте `-Tag`:

```powershell
.\get.ps1 -GitHub shizeexgod/FloV-MP-releases -Tag v1.0.8-beta -Key FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
```

## Способ 2. Архивом, в один клик

Архив `flovmp-setup.zip` лежит в корне этого репозитория и среди файлов
релиза.

1. Распакуйте **весь** архив `flovmp-setup.zip` в папку будущего сервера.
2. Двойной щелчок по `УСТАНОВИТЬ.cmd`.
3. Введите ключ лицензии.

Второй запуск того же файла — обновление, ключ и папка уже сохранены.

## Способ 3. Из полного пакета (без интернета на сервере)

Скачайте `flovmp-server-<версия>-windows.zip` из [последнего релиза](https://github.com/shizeexgod/FloV-MP-releases/releases/latest), распакуйте во временную папку и
из неё выполните:

```powershell
powershell -ExecutionPolicy Bypass -File .\install.ps1 -InstallDir "C:\FloVMP" -LicenseKey FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
```

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

---

# Linux (Ubuntu / Debian)

Нужны root-права (`sudo`). Всё остальное — .NET 10, MariaDB, служба systemd,
порты в файрволе — установщик поставит и настроит сам.

## Установка

```bash
curl -fsSLo flovmp-get.sh https://github.com/shizeexgod/FloV-MP-releases/releases/latest/download/get.sh
sudo bash flovmp-get.sh --github shizeexgod/FloV-MP-releases --key FLV-XXXXXXXX-XXXXXXXX-XXXXXXXX-XXXXXXXX
```

Сервер встанет в `/opt/flovmp` и запустится как служба `flovmp`. Основные
параметры (их можно добавить к команде установки):

| Параметр | Что делает |
|---|---|
| `--dir /srv/flovmp` | другая папка установки |
| `--name "Мой сервер"` | название сервера (только при первой установке) |
| `--owner-sc <SocialClubId>` | сразу выдать себе права основателя |
| `--port 7788` | игровой порт |
| `--no-db` | без MariaDB — всё в файлах |

Подпись релиза и SHA-256 пакета проверяются так же, как на Windows: подменить
пакет нельзя.

## Обновление

```bash
sudo bash /opt/flovmp/update.sh
```

Ключ и источник обновлений берутся из установки. Если стоит последняя
версия, ничего не скачивается. Конкретный релиз — `--tag v1.0.8-beta`.

Если сервер ставили версией **1.0.5 и раньше** (в папке ещё нет `update.sh`),
первый раз обновите командой установки без ключа — дальше хватит `update.sh`:

```bash
curl -fsSLo flovmp-get.sh https://github.com/shizeexgod/FloV-MP-releases/releases/latest/download/get.sh
sudo bash flovmp-get.sh --github shizeexgod/FloV-MP-releases
```

## Управление сервером

```bash
sudo systemctl status flovmp        # состояние
sudo systemctl restart flovmp       # перезапуск
sudo journalctl -u flovmp -f        # журнал службы
tail -f /opt/flovmp/server/server.log
```

Настройки — те же, что на Windows: `/opt/flovmp/server/server.toml` и
`/opt/flovmp/server/config/client.cfg`, свой код — `/opt/flovmp/gamemode`
(сборка: `sudo /opt/flovmp/gamemode/build.sh --install-sdk --restart`).

## Второй сервер на той же машине

Укажите свою папку, службу, порт и базу, чтобы серверы не мешали друг другу:

```bash
sudo bash flovmp-get.sh --github shizeexgod/FloV-MP-releases --key FLV-... \
  --dir /opt/flovmp2 --service flovmp2 --port 7800 --db-name flovmp_server2 --db-user flovmp2
```

## Если сервер не стартует

- `Failed to create host` — занят порт или в ядре выключен IPv6
  (`ipv6.disable=1` в `/etc/default/grub`). Установщик предупреждает об этом.
- `[License]` в журнале — проверьте ключ в `/opt/flovmp/config/flovmp.env`.

---

## Поддержка

Вопросы и продление лицензии — продавцу лицензии.
