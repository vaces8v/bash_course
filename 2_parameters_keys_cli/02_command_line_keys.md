# Ключи командной строки

## Введение

Ключи командной строки - это специальные параметры, которые начинаются с дефиса (-) или двойного дефиса (--). В этом уроке мы рассмотрим различные способы обработки ключей в bash-скриптах.

## Различение ключей и параметров

### Базовый синтаксис

```bash
#!/bin/bash

# Обработка ключей
while [[ $# -gt 0 ]]
do
    case $1 in
        -h|--help)
            echo "Использование: $0 [-h|--help] [-v|--version]"
            exit 0
            ;;
        -v|--version)
            echo "Версия 1.0"
            exit 0
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done
```

### Примеры использования

```bash
#!/bin/bash

# Инициализация переменных
verbose=false
output=""

# Обработка ключей
while [[ $# -gt 0 ]]
do
    case $1 in
        -v|--verbose)
            verbose=true
            ;;
        -o|--output)
            output="$2"
            shift
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done

# Использование параметров
if [ "$verbose" = true ]; then
    echo "Режим подробного вывода"
fi

if [ -n "$output" ]; then
    echo "Вывод будет сохранен в: $output"
fi
```

## Обработка ключей со значениями

### Ключи с обязательными значениями

```bash
#!/bin/bash

# Обработка ключей со значениями
while [[ $# -gt 0 ]]
do
    case $1 in
        -f|--file)
            if [ -z "$2" ]; then
                echo "Ошибка: требуется значение для ключа $1"
                exit 1
            fi
            file="$2"
            shift
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done

# Проверка наличия обязательного значения
if [ -z "$file" ]; then
    echo "Ошибка: не указан файл"
    exit 1
fi
```

### Ключи с опциональными значениями

```bash
#!/bin/bash

# Обработка ключей с опциональными значениями
while [[ $# -gt 0 ]]
do
    case $1 in
        -l|--level)
            if [[ "$2" =~ ^[0-9]+$ ]]; then
                level="$2"
                shift
            else
                level=1
            fi
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done

# Использование значения по умолчанию
level=${level:-1}
```

## Использование стандартных ключей

### Общие ключи

```bash
#!/bin/bash

# Обработка стандартных ключей
while [[ $# -gt 0 ]]
do
    case $1 in
        -h|--help)
            echo "Использование: $0 [опции]"
            echo "Опции:"
            echo "  -h, --help     Показать эту справку"
            echo "  -v, --version  Показать версию"
            echo "  -d, --debug    Включить режим отладки"
            echo "  -q, --quiet    Тихий режим"
            exit 0
            ;;
        -v|--version)
            echo "Версия 1.0"
            exit 0
            ;;
        -d|--debug)
            set -x
            ;;
        -q|--quiet)
            quiet=true
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done
```

### Группировка коротких ключей

```bash
#!/bin/bash

# Обработка сгруппированных ключей
while [[ $# -gt 0 ]]
do
    case $1 in
        -[vqd])
            for (( i=1; i<${#1}; i++ ))
            do
                case ${1:$i:1} in
                    v) verbose=true ;;
                    q) quiet=true ;;
                    d) debug=true ;;
                esac
            done
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done
```

## Практическое задание

Создайте скрипт `keys_demo.sh`:

```bash
#!/bin/bash

# Инициализация переменных
verbose=false
quiet=false
output=""
level=1

# Обработка ключей
while [[ $# -gt 0 ]]
do
    case $1 in
        -h|--help)
            echo "Использование: $0 [опции]"
            echo "Опции:"
            echo "  -h, --help     Показать эту справку"
            echo "  -v, --verbose  Включить подробный вывод"
            echo "  -q, --quiet    Тихий режим"
            echo "  -o, --output   Файл для вывода"
            echo "  -l, --level    Уровень (1-3)"
            exit 0
            ;;
        -v|--verbose)
            verbose=true
            ;;
        -q|--quiet)
            quiet=true
            ;;
        -o|--output)
            if [ -z "$2" ]; then
                echo "Ошибка: требуется значение для ключа $1"
                exit 1
            fi
            output="$2"
            shift
            ;;
        -l|--level)
            if [[ "$2" =~ ^[1-3]$ ]]; then
                level="$2"
                shift
            else
                echo "Ошибка: уровень должен быть от 1 до 3"
                exit 1
            fi
            ;;
        *)
            echo "Неизвестный ключ: $1"
            exit 1
            ;;
    esac
    shift
done

# Проверка конфликтующих опций
if [ "$verbose" = true ] && [ "$quiet" = true ]; then
    echo "Ошибка: нельзя использовать -v и -q одновременно"
    exit 1
fi

# Вывод настроек
if [ "$verbose" = true ]; then
    echo "Режим подробного вывода включен"
fi

if [ "$quiet" = true ]; then
    echo "Тихий режим включен"
fi

if [ -n "$output" ]; then
    echo "Вывод будет сохранен в: $output"
fi

echo "Уровень: $level"
```

## Важные моменты

- Всегда проверяйте наличие значений для ключей
- Обрабатывайте конфликтующие опции
- Используйте стандартные ключи (--help, --version)
- Документируйте все доступные ключи
- Обрабатывайте ошибки

## Дополнительные материалы

- [Bash Command Line Arguments](https://www.gnu.org/software/bash/manual/bash.html#Command-Line-Editing)
- [Bash Case Statement](https://www.gnu.org/software/bash/manual/bash.html#Conditional-Constructs)

## Вопросы для самопроверки

1. Как различать ключи и обычные параметры?
2. Как обработать ключи со значениями?
3. Как реализовать группировку коротких ключей?
4. Как обработать конфликтующие опции? 