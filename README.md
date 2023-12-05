# Tutorial Systemd

Systemd - подсистема инициализации и управления службами в Linux, фактически вытеснившая в 2010-е годы традиционную подсистему init.

## Шаг 1: Создание файла службы для вашего бота

Откройте терминал на вашем удаленном сервере.

Создайте файл службы "name".service для вашего бота:

```bash 
sudo nano /etc/systemd/system/"name".service
```

Вставьте следующую конфигурацию для юнита "name".service:
```bash
[Unit]
Description="name"
After=network.target

[Service]
User=root
WorkingDirectory=/root/game/"name"
ExecStart=/usr/bin/python3 /root/game/"name"/"name".py
Restart=always

[Install]
WantedBy=multi-user.target
```

Здесь:

- Description - описание вашей службы.
- User - пользователь, от имени которого будет запущен бот (в данном случае, root).
- WorkingDirectory - каталог, где находится ваш файл "name".py.
- ExecStart - полный путь к вашему файлу "name".py.





## Шаг 2: Управление юнитом systemd

Обновите конфигурацию systemd:
```bash 
sudo systemctl daemon-reload
```

Запустите службу бота:

```bash 
sudo systemctl start "name"
```

Проверьте статус службы, чтобы убедиться, что она запущена без ошибок:

```bash 
sudo systemctl status "name"
```

Если все в порядке, включите службу, чтобы она автоматически запускалась при загрузке сервера:
```bash 
sudo systemctl enable "name"
```



---
---

Расшифровка конфигурации для юнита "name".service

```bash
[Unit]
Description="name"
After=network.target

[Service]
User=root
WorkingDirectory=/root/game/"name"
ExecStart=/usr/bin/python3 /root/game/"name"/"name".py
Restart=always

[Install]
WantedBy=multi-user.target
```
- Description: Здесь вы описываете вашу службу. В данном случае, это описание вашего бота.
- After=network.target: Это указывает на то, что ваша служба будет запускаться после того, как будет загружена сеть (network.target). Это гарантирует, что ваш бот будет запускаться после установки сетевых подключений.


- User=root: Указывает, от имени какого пользователя будет запускаться служба. В данном случае, ваш бот будет запущен от имени пользователя root.
- WorkingDirectory: Это каталог, где будет работать ваша служба. В данном случае, это /root/game/"name", где находится ваш файл "name".py.
- ExecStart: Это команда, которая будет выполнена для запуска вашего бота. В вашем случае, это /usr/bin/python3 /root/game/"name"/"name".py, что означает использование Python 3 для запуска вашего файла "name".py.
- Restart=always: Этот параметр определяет, как система будет перезапускать вашу службу в случае её остановки. always говорит systemd о перезапуске службы при любой её остановке, даже если это было из-за ошибки.


- WantedBy=multi-user.target: Это указывает systemd, что служба должна быть запущена при достижении multi-user.target. Это обычный уровень запуска, где система работает в многопользовательском режиме.

Итак, этот файл описывает, как systemd будет запускать вашего бота "name".py. Он указывает, какой пользователь будет использоваться для запуска, где находится файл бота, как его запускать и условия перезапуска в случае остановки.


---
---

# journalctl

journalctl - это утилита журнала системы, которая позволяет просматривать журналы событий системы и выводить записи из системного журнала на большинстве дистрибутивов Linux, использующих systemd.

- Опция -u в команде journalctl позволяет фильтровать вывод журнала по определенному юниту (unit). Юниты в systemd - это модули, которые управляют различными службами, процессами или устройствами в системе. Когда вы используете -u, вы указываете journalctl вывести записи журнала только для конкретного юнита.

```bash
journalctl -u "name"
```