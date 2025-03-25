 # Функции в bash

## Введение

Функции в bash позволяют создавать переиспользуемые блоки кода, что делает скрипты более организованными и поддерживаемыми. В этом уроке мы рассмотрим различные аспекты работы с функциями.

## Объявление функций

### Базовый синтаксис

```bash
#!/bin/bash

# Объявление функции
function my_function() {
    echo "Это моя функция"
}

# Альтернативный синтаксис
my_other_function() {
    echo "Это другая функция"
}
```

### Функции с описанием

```bash
#!/bin/bash

# Функция с описанием
function print_info() {
    # Описание функции
    echo "Функция для вывода информации"
    echo "Аргументы: $@"
}
```

## Использование функций

### Вызов функции

```bash
#!/bin/bash

# Определение функции
greet() {
    echo "Привет, $1!"
}

# Вызов функции
greet "Пользователь"

# Вызов с несколькими аргументами
greet "Пользователь" "Добро пожаловать"
```

### Вложенные функции

```bash
#!/bin/bash

# Внешняя функция
outer_function() {
    echo "Внешняя функция"
    
    # Внутренняя функция
    inner_function() {
        echo "Внутренняя функция"
    }
    
    # Вызов внутренней функции
    inner_function
}
```

## Использование команды return

### Возврат статуса

```bash
#!/bin/bash

# Функция с проверкой статуса
check_file() {
    if [ -f "$1" ]; then
        return 0
    else
        return 1
    fi
}

# Использование статуса
if check_file "test.txt"; then
    echo "Файл существует"
else
    echo "Файл не существует"
fi
```

### Возврат значения

```bash
#!/bin/bash

# Функция с возвратом значения
get_sum() {
    local sum=$(( $1 + $2 ))
    return $sum
}

# Получение результата
get_sum 5 3
echo "Сумма: $?"
```

## Запись вывода функции в переменную

### Использование command substitution

```bash
#!/bin/bash

# Функция с выводом
get_date() {
    date "+%Y-%m-%d %H:%M:%S"
}

# Запись вывода в переменную
current_date=$(get_date)
echo "Текущая дата: $current_date"
```

### Использование глобальных переменных

```bash
#!/bin/bash

# Глобальная переменная
result=""

# Функция с записью в глобальную переменную
calculate() {
    result=$(( $1 * $2 ))
}

# Использование
calculate 5 3
echo "Результат: $result"
```

## Аргументы функций

### Работа с аргументами

```bash
#!/bin/bash

# Функция с аргументами
print_args() {
    echo "Количество аргументов: $#"
    echo "Все аргументы: $@"
    echo "Первый аргумент: $1"
    echo "Второй аргумент: $2"
}

# Вызов с аргументами
print_args "arg1" "arg2" "arg3"
```

### Аргументы по умолчанию

```bash
#!/bin/bash

# Функция с аргументами по умолчанию
greet() {
    local name=${1:-"Гость"}
    local greeting=${2:-"Добро пожаловать"}
    echo "$greeting, $name!"
}

# Вызов с разными аргументами
greet "Пользователь" "Привет"
greet "Пользователь"
greet
```

## Работа с переменными в функциях

### Глобальные переменные

```bash
#!/bin/bash

# Глобальная переменная
global_var="Глобальное значение"

# Функция с использованием глобальной переменной
use_global() {
    echo "Глобальная переменная: $global_var"
    global_var="Измененное значение"
}

# Вызов функции
use_global
echo "После функции: $global_var"
```

### Локальные переменные

```bash
#!/bin/bash

# Функция с локальными переменными
local_vars() {
    local var1="Локальное значение 1"
    local var2="Локальное значение 2"
    echo "Внутри функции: $var1, $var2"
}

# Вызов функции
local_vars
echo "Вне функции: $var1, $var2"  # Переменные не доступны
```

## Передача функциям массивов в качестве аргументов

### Работа с массивами

```bash
#!/bin/bash

# Функция для работы с массивом
print_array() {
    local arr=("$@")
    echo "Элементы массива:"
    for element in "${arr[@]}"; do
        echo "$element"
    done
}

# Создание и передача массива
my_array=("Элемент 1" "Элемент 2" "Элемент 3")
print_array "${my_array[@]}"
```

### Модификация массива

```bash
#!/bin/bash

# Функция для модификации массива
modify_array() {
    local -n arr=$1
    arr+=("Новый элемент")
}

# Создание массива
my_array=("Элемент 1" "Элемент 2")
modify_array my_array
echo "После модификации: ${my_array[@]}"
```

## Рекурсивные функции

### Простая рекурсия

```bash
#!/bin/bash

# Рекурсивная функция
countdown() {
    if [ $1 -le 0 ]; then
        echo "Время вышло!"
        return
    fi
    echo $1
    sleep 1
    countdown $(( $1 - 1 ))
}

# Вызов рекурсивной функции
countdown 5
```

### Рекурсия с условием выхода

```bash
#!/bin/bash

# Рекурсивная функция с условием
factorial() {
    if [ $1 -le 1 ]; then
        echo 1
        return
    fi
    local prev=$(factorial $(( $1 - 1 )))
    echo $(( $1 * prev ))
}

# Вызов
echo "Факториал 5: $(factorial 5)"
```

## Создание и использование библиотек

### Создание библиотеки

```bash
#!/bin/bash
# library.sh

# Функции библиотеки
function math_add() {
    echo $(( $1 + $2 ))
}

function math_subtract() {
    echo $(( $1 - $2 ))
}

function math_multiply() {
    echo $(( $1 * $2 ))
}
```

### Использование библиотеки

```bash
#!/bin/bash

# Подключение библиотеки
source library.sh

# Использование функций
result=$(math_add 5 3)
echo "5 + 3 = $result"

result=$(math_multiply 4 2)
echo "4 * 2 = $result"
```

## Вызов bash-функций из командной строки

### Экспорт функций

```bash
#!/bin/bash

# Определение функции
function greet() {
    echo "Привет, $1!"
}

# Экспорт функции
export -f greet

# Теперь функцию можно вызвать из командной строки:
# bash -c 'greet "Пользователь"'
```

### Функции в .bashrc

```bash
#!/bin/bash

# Добавление функции в .bashrc
echo 'function mycommand() {
    echo "Это моя команда"
}' >> ~/.bashrc

# Перезагрузка .bashrc
source ~/.bashrc
```

## Практическое задание

Создайте скрипт `functions_demo.sh`:

```bash
#!/bin/bash

# Функция для работы с файлами
file_operations() {
    local filename=$1
    local operation=$2
    
    case $operation in
        "create")
            touch "$filename"
            echo "Создан файл: $filename"
            ;;
        "delete")
            rm -f "$filename"
            echo "Удален файл: $filename"
            ;;
        "check")
            if [ -f "$filename" ]; then
                echo "Файл существует"
            else
                echo "Файл не существует"
            fi
            ;;
        *)
            echo "Неизвестная операция"
            return 1
            ;;
    esac
}

# Функция для работы с числами
number_operations() {
    local num1=$1
    local num2=$2
    local operation=$3
    
    case $operation in
        "add")
            echo $(( num1 + num2 ))
            ;;
        "subtract")
            echo $(( num1 - num2 ))
            ;;
        "multiply")
            echo $(( num1 * num2 ))
            ;;
        *)
            echo "Неизвестная операция"
            return 1
            ;;
    esac
}

# Демонстрация работы с файлами
echo "=== Работа с файлами ==="
file_operations "test.txt" "create"
file_operations "test.txt" "check"
file_operations "test.txt" "delete"
file_operations "test.txt" "check"

# Демонстрация работы с числами
echo -e "\n=== Работа с числами ==="
echo "5 + 3 = $(number_operations 5 3 "add")"
echo "10 - 4 = $(number_operations 10 4 "subtract")"
echo "6 * 7 = $(number_operations 6 7 "multiply")"
```

## Важные моменты

- Всегда используйте локальные переменные в функциях
- Проверяйте аргументы функций
- Используйте return для возврата статуса
- Документируйте функции
- Избегайте глобальных переменных

## Дополнительные материалы

- [Bash Functions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Functions)
- [Bash Variables](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameters)

## Вопросы для самопроверки

1. Как объявить функцию в bash?
2. Как передать аргументы в функцию?
3. Как использовать локальные переменные?
4. Как создать рекурсивную функцию?
5. Как использовать библиотеки функций?