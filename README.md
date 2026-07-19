# AHS Screens — установка и обновление

Этот репозиторий содержит только готовые установочные пакеты AHS Screens.
Исходный код и приложение выпуска лицензий здесь не публикуются.

## Сервер: Debian 12 или Debian 13

Графическая оболочка серверу не нужна. Выполните команды от `root`:

```bash
apt-get update
apt-get install -y curl ca-certificates
curl -fLO https://github.com/akinin/ahs-screens-releases/releases/latest/download/install-server-release.sh
chmod +x install-server-release.sh
ADMIN_USER_VALUE=admin ADMIN_PASSWORD_VALUE='ЗАМЕНИТЕ_НА_НАДЕЖНЫЙ_ПАРОЛЬ' ./install-server-release.sh
```

После установки панель доступна по адресу `http://IP-СЕРВЕРА:8000`. Для
публикации через Nginx Proxy Manager направьте домен на этот адрес и включите
WebSocket Support. После проверки HTTPS установите `COOKIE_SECURE=1` в
`/etc/digitalsign/server.env` и перезапустите `digitalsign-server`.

## Клиент: Debian 12 или Debian 13 без рабочего стола

Скрипт установит минимальное графическое окружение, Chromium, плеер и службу
автозапуска:

```bash
apt-get update
apt-get install -y curl ca-certificates
curl -fLO https://github.com/akinin/ahs-screens-releases/releases/latest/download/install-client-release.sh
chmod +x install-client-release.sh
SERVER_URL='https://screens.example.com' ./install-client-release.sh
```

После запуска на экране появится код привязки. Подтвердите устройство в панели
администратора.

## Клиент: macOS

Запускайте от обычного пользователя, без `sudo`:

```bash
curl -fLO https://github.com/akinin/ahs-screens-releases/releases/latest/download/bootstrap-macos.sh
chmod +x bootstrap-macos.sh
SERVER_URL='https://screens.example.com' ./bootstrap-macos.sh
```

Тестовый режим macOS работает в окне. Настройки клиента позволяют проверить
сервер, перепривязать устройство и включить автоматический запуск.

## Клиент: Windows

Откройте PowerShell от имени администратора:

```powershell
Set-ExecutionPolicy -Scope Process Bypass -Force
Invoke-WebRequest https://github.com/akinin/ahs-screens-releases/releases/latest/download/bootstrap-windows.ps1 -OutFile bootstrap-windows.ps1
.\bootstrap-windows.ps1 -ServerUrl 'https://screens.example.com'
```

## Клиент: Android

Скачайте APK из раздела **Releases → Latest**, разрешите установку из выбранного
браузера или файлового менеджера и установите приложение. Для полноценного
киоск-режима используйте сценарий Device Owner из настроек клиента.

## Обновление

- сервер: **Настройки → Проверить обновления → Обновить сервер**;
- клиенты: кнопка **Обновить** у устройства или групповая команда;
- без интернета: загрузите серверный `.deb` через раздел **Локальные обновления**;
- установочные пакеты проверяются по `SHA256SUMS`.

## Диагностика

```bash
systemctl status digitalsign-server --no-pager
journalctl -u digitalsign-server -n 100 --no-pager
systemctl status digitalsign-client --no-pager
journalctl -u digitalsign-client -n 100 --no-pager
```

Перед обращением в поддержку укажите версию сервера, версию клиента, ОС
устройства и последние сообщения соответствующей службы.
