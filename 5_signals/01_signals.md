# Сигналы в bash

## Введение

Сигналы - это механизм межпроцессного взаимодействия в Linux/Unix системах. Они позволяют процессам обмениваться уведомлениями о различных событиях.

## Сигналы Linux

### Основные сигналы

```bash
#!/bin/bash

# Список основных сигналов
echo "Основные сигналы Linux:"
echo "SIGHUP  (1)  - Завершение терминала"
echo "SIGINT  (2)  - Прерывание (Ctrl+C)"
echo "SIGQUIT (3)  - Выход (Ctrl+\)"
echo "SIGTERM (15) - Завершение процесса"
echo "SIGKILL (9)  - Принудительное завершение"
echo "SIGSTOP (19) - Остановка процесса"
echo "SIGTSTP (20) - Остановка с терминала (Ctrl+Z)"
echo "SIGCONT (18) - Продолжение выполнения"
```

## Отправка сигналов скриптам

### Команда kill

```bash
#!/bin/bash

# Отправка сигнала по PID
kill -SIGTERM 1234

# Отправка сигнала по имени процесса
killall -SIGTERM process_name

# Отправка сигнала группе процессов
kill -SIGTERM -1234  # Отрицательный PID означает группу
```

### Команда pkill

```bash
#!/bin/bash

# Завершение процесса по имени
pkill process_name

# Завершение процесса с указанием сигнала
pkill -SIGTERM process_name

# Завершение процесса с учетом регистра
pkill -i process_name
```

## Завершение работы процесса

### Корректное завершение

```bash
#!/bin/bash

# Функция очистки
cleanup() {
    echo "Очистка временных файлов..."
    rm -f temp_file
    echo "Завершение работы..."
    exit 0
}

# Регистрация функции очистки
trap cleanup EXIT

# Основной код
echo "Создание временного файла..."
touch temp_file
sleep 5
```

### Принудительное завершение

```bash
#!/bin/bash

# Завершение процесса по PID
kill -9 1234

# Завершение всех процессов с определенным именем
pkill -9 process_name
```

## Временная остановка процесса

### Остановка и возобновление

```bash
#!/bin/bash

# Остановка процесса
kill -SIGSTOP 1234

# Возобновление процесса
kill -SIGCONT 1234

# Остановка с терминала (Ctrl+Z)
# Возобновление с терминала (fg или bg)
```

## Перехват сигналов

### Базовый перехват

```bash
#!/bin/bash

# Функция обработки сигнала
handle_signal() {
    echo "Получен сигнал $1"
    exit 0
}

# Регистрация обработчиков
trap 'handle_signal SIGTERM' SIGTERM
trap 'handle_signal SIGINT' SIGINT

# Основной код
while true; do
    echo "Работа скрипта..."
    sleep 1
done
```

### Перехват нескольких сигналов

```bash
#!/bin/bash

# Общий обработчик для нескольких сигналов
handle_signals() {
    echo "Получен сигнал $1"
    case $1 in
        SIGTERM)
            echo "Завершение работы..."
            exit 0
            ;;
        SIGINT)
            echo "Прерывание работы..."
            exit 1
            ;;
        *)
            echo "Неизвестный сигнал"
            ;;
    esac
}

# Регистрация обработчиков
trap 'handle_signals SIGTERM' SIGTERM
trap 'handle_signals SIGINT' SIGINT
trap 'handle_signals SIGQUIT' SIGQUIT
```

## Перехват сигнала выхода из скрипта

```bash
#!/bin/bash

# Функция очистки при выходе
cleanup() {
    echo "Выполняется очистка..."
    rm -f temp_file
    echo "Скрипт завершает работу"
}

# Регистрация функции очистки
trap cleanup EXIT

# Основной код
echo "Создание временного файла..."
touch temp_file
echo "Ожидание 5 секунд..."
sleep 5
```

## Модификация перехваченных сигналов и отмена перехвата

### Модификация обработчика

```bash
#!/bin/bash

# Функция обработки сигнала
handle_signal() {
    echo "Обработка сигнала $1"
    # Изменение поведения обработчика
    case $1 in
        SIGINT)
            echo "Игнорирование прерывания..."
            ;;
        SIGTERM)
            echo "Завершение работы..."
            exit 0
            ;;
    esac
}

# Регистрация обработчика
trap 'handle_signal SIGINT' SIGINT
trap 'handle_signal SIGTERM' SIGTERM
```

### Отмена перехвата

```bash
#!/bin/bash

# Функция обработки сигнала
handle_signal() {
    echo "Получен сигнал $1"
    # Отмена перехвата сигнала
    trap - SIGINT
    echo "Перехват сигнала отменен"
}

# Регистрация обработчика
trap 'handle_signal SIGINT' SIGINT
```

## Практическое задание

Создайте скрипт `signals_demo.sh`:

```bash
#!/bin/bash

# Функция очистки
cleanup() {
    echo "Выполняется очистка..."
    rm -f temp_file
    echo "Скрипт завершает работу"
    exit 0
}

# Функция обработки сигналов
handle_signal() {
    echo "Получен сигнал $1"
    case $1 in
        SIGTERM)
            echo "Завершение работы..."
            cleanup
            ;;
        SIGINT)
            echo "Прерывание работы..."
            cleanup
            ;;
        SIGQUIT)
            echo "Выход..."
            cleanup
            ;;
        *)
            echo "Неизвестный сигнал"
            ;;
    esac
}

# Регистрация обработчиков
trap cleanup EXIT
trap 'handle_signal SIGTERM' SIGTERM
trap 'handle_signal SIGINT' SIGINT
trap 'handle_signal SIGQUIT' SIGQUIT

# Основной код
echo "Создание временного файла..."
touch temp_file

echo "Скрипт запущен. PID: $$"
echo "Используйте Ctrl+C для прерывания"
echo "Используйте kill -TERM $$ для завершения"
echo "Используйте kill -QUIT $$ для выхода"

# Бесконечный цикл
while true; do
    echo "Работа скрипта..."
    sleep 1
done
```

## Важные моменты

- Всегда обрабатывайте сигналы в скриптах
- Используйте функции очистки при выходе
- Проверяйте права доступа при отправке сигналов
- Помните о приоритетах сигналов
- SIGKILL нельзя перехватить

## Дополнительные материалы

- [Bash Signals](https://www.gnu.org/software/bash/manual/bash.html#Signals)
- [Linux Signals](https://man7.org/linux/man-pages/man7/signal.7.html)

## Вопросы для самопроверки

1. Какие основные сигналы существуют в Linux?
2. Как перехватить сигнал в bash-скрипте?
3. Как отменить перехват сигнала?
4. Как реализовать очистку при выходе из скрипта?
5. Почему нельзя перехватить SIGKILL? 