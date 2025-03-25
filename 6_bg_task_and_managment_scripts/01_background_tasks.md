# Фоновые задачи и управление скриптами

## Введение

В Linux/Unix системах существует несколько способов запуска скриптов в фоновом режиме и управления их выполнением. В этом уроке мы рассмотрим основные методы и инструменты.

## Выполнение сценариев командной строки в фоновом режиме

### Базовый запуск в фоне

```bash
#!/bin/bash

# Запуск команды в фоновом режиме
command &

# Запуск скрипта в фоновом режиме
./script.sh &

# Запуск с перенаправлением вывода
command > output.log 2>&1 &
```

### Запуск с nohup

```bash
#!/bin/bash

# Запуск с nohup (игнорирование SIGHUP)
nohup ./script.sh &

# Запуск с перенаправлением вывода
nohup ./script.sh > output.log 2>&1 &
```

## Выполнение скриптов, не завершающих работу при закрытии терминала

### Использование setsid

```bash
#!/bin/bash

# Запуск в новой сессии
setsid ./script.sh

# Запуск с перенаправлением вывода
setsid ./script.sh > output.log 2>&1
```

### Использование screen

```bash
#!/bin/bash

# Создание новой screen сессии
screen -S myscript ./script.sh

# Создание именованной сессии
screen -dmS myscript ./script.sh
```

## Просмотр заданий

### Команда jobs

```bash
#!/bin/bash

# Просмотр всех заданий
jobs

# Просмотр с подробной информацией
jobs -l

# Просмотр только процессов
jobs -p
```

### Команда ps

```bash
#!/bin/bash

# Просмотр процессов текущего пользователя
ps aux | grep script.sh

# Просмотр процессов с деревом
ps -ef --forest
```

## Перезапуск приостановленных заданий

### Возобновление в фоне

```bash
#!/bin/bash

# Возобновление последнего приостановленного задания
bg

# Возобновление конкретного задания
bg %1

# Возобновление в фоне с выводом на экран
bg %1 > /dev/tty
```

### Возобновление на переднем плане

```bash
#!/bin/bash

# Возобновление последнего приостановленного задания
fg

# Возобновление конкретного задания
fg %1
```

## Планирование запуска скриптов

### Команда at

```bash
#!/bin/bash

# Запуск через 5 минут
at now + 5 minutes << EOF
./script.sh
EOF

# Запуск в определенное время
at 15:30 << EOF
./script.sh
EOF
```

### Команда batch

```bash
#!/bin/bash

# Запуск при низкой нагрузке системы
batch << EOF
./script.sh
EOF
```

## Удаление заданий, ожидающих выполнения

### Удаление заданий at

```bash
#!/bin/bash

# Просмотр заданий
atq

# Удаление задания
atrm 1

# Удаление всех заданий
atq | awk '{print $1}' | xargs atrm
```

### Удаление фоновых процессов

```bash
#!/bin/bash

# Удаление по PID
kill 1234

# Удаление по имени
pkill script.sh

# Удаление всех процессов пользователя
killall -u username
```

## Запуск скриптов по расписанию

### Настройка cron

```bash
#!/bin/bash

# Открытие редактора crontab
crontab -e

# Примеры расписания
# Каждую минуту
* * * * * /path/to/script.sh

# Каждый час
0 * * * * /path/to/script.sh

# Каждый день в полночь
0 0 * * * /path/to/script.sh

# Каждую неделю
0 0 * * 0 /path/to/script.sh
```

### Проверка и управление cron

```bash
#!/bin/bash

# Просмотр текущих задач
crontab -l

# Удаление всех задач
crontab -r

# Проверка логов cron
grep CRON /var/log/syslog
```

## Запуск скриптов при входе в систему и при запуске оболочки

### Автозапуск при входе в систему

```bash
#!/bin/bash

# Добавление в /etc/rc.local
echo "/path/to/script.sh" >> /etc/rc.local

# Создание systemd сервиса
cat > /etc/systemd/system/myscript.service << EOF
[Unit]
Description=My Script Service
After=network.target

[Service]
Type=simple
ExecStart=/path/to/script.sh
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

### Автозапуск при запуске оболочки

```bash
#!/bin/bash

# Добавление в ~/.bashrc
echo "/path/to/script.sh &" >> ~/.bashrc

# Добавление в ~/.profile
echo "/path/to/script.sh &" >> ~/.profile
```

## Практическое задание

Создайте скрипт `background_demo.sh`:

```bash
#!/bin/bash

# Функция очистки
cleanup() {
    echo "Очистка..."
    rm -f temp_file
    exit 0
}

# Регистрация очистки
trap cleanup EXIT

# Создание временного файла
touch temp_file

# Основной цикл
while true; do
    echo "Работа скрипта..."
    echo "PID: $$"
    echo "Время: $(date)"
    echo "---"
    sleep 5
done
```

## Важные моменты

- Всегда проверяйте права доступа для автозапуска
- Используйте логирование для фоновых задач
- Правильно обрабатывайте сигналы завершения
- Проверяйте зависимости перед автозапуском
- Используйте screen или tmux для долгих задач

## Дополнительные материалы

- [Bash Job Control](https://www.gnu.org/software/bash/manual/bash.html#Job-Control)
- [Cron Documentation](https://man7.org/linux/man-pages/man8/cron.8.html)
- [Systemd Service](https://www.freedesktop.org/software/systemd/man/systemd.service.html)

## Вопросы для самопроверки

1. Как запустить скрипт в фоновом режиме?
2. Как сделать скрипт устойчивым к закрытию терминала?
3. Как просмотреть список фоновых задач?
4. Как настроить автозапуск скрипта при старте системы?
5. Как удалить задание из очереди at? 