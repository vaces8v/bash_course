# Язык обработки данных awk

## Введение

awk - это мощный язык обработки данных, который особенно эффективен для работы с текстовыми файлами и потоками данных. В этом уроке мы рассмотрим основные возможности awk.

## Особенности вызова awk

### Базовый синтаксис

```bash
#!/bin/bash

# Базовый вызов awk
awk 'команда' файл

# Пример простой команды
awk '{print $1}' input.txt

# Вызов с флагами
awk -F: '{print $1}' input.txt  # указание разделителя
awk -v var=value '{print var}' input.txt  # определение переменной
```

### Работа со стандартным вводом

```bash
#!/bin/bash

# Чтение из stdin
echo "текст" | awk '{print $1}'

# Чтение из файла
cat input.txt | awk '{print $1}'
```

## Чтение awk-скриптов из командной строки

### Простые команды

```bash
#!/bin/bash

# Вывод первого поля
awk '{print $1}' input.txt

# Вывод нескольких полей
awk '{print $1, $3}' input.txt

# Вывод с форматированием
awk '{printf "Имя: %s, Возраст: %s\n", $1, $2}' input.txt
```

### Сложные команды

```bash
#!/bin/bash

# Команда с условием
awk '$3 > 100 {print $1, $3}' input.txt

# Команда с регулярным выражением
awk '/шаблон/ {print $0}' input.txt

# Команда с вычислениями
awk '{sum += $3} END {print "Сумма: " sum}' input.txt
```

## Позиционные переменные, хранящие данные полей

### Работа с полями

```bash
#!/bin/bash

# Вывод полей
awk '{print $1, $2, $3}' input.txt

# Вывод последнего поля
awk '{print $NF}' input.txt

# Вывод предпоследнего поля
awk '{print $(NF-1)}' input.txt
```

### Специальные переменные

```bash
#!/bin/bash

# NR - номер текущей строки
awk '{print NR, $0}' input.txt

# NF - количество полей
awk '{print NF, $0}' input.txt

# FS - разделитель полей
awk -F: '{print $1, $2}' input.txt
```

## Использование нескольких команд

### Команды в одной строке

```bash
#!/bin/bash

# Несколько команд через точку с запятой
awk '{print $1; print $2}' input.txt

# Команды с условиями
awk '$1=="test" {print $2; print $3}' input.txt
```

### Блоки BEGIN и END

```bash
#!/bin/bash

# Инициализация и завершение
awk 'BEGIN {print "Начало"} {print $0} END {print "Конец"}' input.txt

# Подсчет статистики
awk 'BEGIN {sum=0} {sum+=$3} END {print "Сумма: " sum}' input.txt
```

## Чтение скрипта awk из файла

### Создание скрипта

```bash
#!/bin/bash

# Создание awk-скрипта
cat > script.awk << EOF
BEGIN { print "Начало обработки" }
{ print $1, $2 }
END { print "Конец обработки" }
EOF

# Запуск скрипта
awk -f script.awk input.txt
```

### Сложные скрипты

```bash
#!/bin/bash

# Создание скрипта с функциями
cat > complex.awk << EOF
function sum(a, b) {
    return a + b
}
{ print sum($1, $2) }
EOF

# Запуск скрипта
awk -f complex.awk input.txt
```

## Выполнение команд до начала обработки данных

### Блок BEGIN

```bash
#!/bin/bash

# Инициализация переменных
awk 'BEGIN {FS=":"; OFS=" - "} {print $1, $2}' input.txt

# Подготовка данных
awk 'BEGIN {print "=== Обработка данных ==="} {print $0}' input.txt
```

### Настройка окружения

```bash
#!/bin/bash

# Настройка разделителей
awk 'BEGIN {FS=","; RS="\n"} {print $1, $2}' input.txt

# Настройка вывода
awk 'BEGIN {OFS="\t"} {print $1, $2}' input.txt
```

## Выполнение команд после окончания обработки данных

### Блок END

```bash
#!/bin/bash

# Подсчет статистики
awk '{sum+=$3} END {print "Сумма: " sum}' input.txt

# Вывод итогов
awk '{count++} END {print "Всего строк: " count}' input.txt
```

### Форматирование результатов

```bash
#!/bin/bash

# Форматированный вывод статистики
awk 'BEGIN {printf "=== Статистика ===\n"}
     {sum+=$3; count++}
     END {printf "Сумма: %d\nСреднее: %.2f\n", sum, sum/count}' input.txt
```

## Встроенные переменные: настройка процесса обработки данных

### Основные переменные

```bash
#!/bin/bash

# FS - разделитель полей
awk -F: '{print $1}' input.txt

# RS - разделитель записей
awk 'BEGIN {RS="\n\n"} {print $0}' input.txt

# OFS - разделитель полей вывода
awk 'BEGIN {OFS=" - "} {print $1, $2}' input.txt
```

### Дополнительные переменные

```bash
#!/bin/bash

# ORS - разделитель записей вывода
awk 'BEGIN {ORS="\n---\n"} {print $0}' input.txt

# NR - номер текущей записи
awk '{print NR, $0}' input.txt

# NF - количество полей
awk '{print NF, $0}' input.txt
```

## Встроенные переменные: сведения о данных и об окружении

### Информационные переменные

```bash
#!/bin/bash

# FILENAME - имя текущего файла
awk '{print FILENAME, $0}' input.txt

# FNR - номер записи в текущем файле
awk '{print FNR, $0}' input.txt

# ARGC - количество аргументов
awk 'BEGIN {print ARGC}' input.txt
```

### Переменные окружения

```bash
#!/bin/bash

# ENVIRON - переменные окружения
awk 'BEGIN {print ENVIRON["PATH"]}'

# ARGV - массив аргументов
awk 'BEGIN {for(i=1; i<ARGC; i++) print ARGV[i]}' file1 file2
```

## Пользовательские переменные

### Определение переменных

```bash
#!/bin/bash

# Определение через -v
awk -v var=value '{print var}' input.txt

# Определение в блоке BEGIN
awk 'BEGIN {var="значение"} {print var}' input.txt
```

### Работа с массивами

```bash
#!/bin/bash

# Ассоциативные массивы
awk '{count[$1]++} END {for(key in count) print key, count[key]}' input.txt

# Числовые массивы
awk 'BEGIN {for(i=1; i<=5; i++) arr[i]=i*i} {print arr[NR]}' input.txt
```

## Условный оператор

### Простые условия

```bash
#!/bin/bash

# if-else
awk '{if($3>100) print $1; else print $2}' input.txt

# Тернарный оператор
awk '{print ($3>100 ? $1 : $2)}' input.txt
```

### Сложные условия

```bash
#!/bin/bash

# Множественные условия
awk '{if($1=="test" && $3>100) print $0}' input.txt

# Вложенные условия
awk '{if($1=="test") {if($3>100) print $1; else print $2}}' input.txt
```

## Цикл while

### Базовый цикл

```bash
#!/bin/bash

# Простой цикл
awk 'BEGIN {i=1; while(i<=5) {print i; i++}}'

# Цикл с условием
awk '{while($1>0) {print $1; $1--}}' input.txt
```

### Вложенные циклы

```bash
#!/bin/bash

# Два вложенных цикла
awk 'BEGIN {for(i=1; i<=2; i++) {for(j=1; j<=2; j++) print i,j}}'
```

## Цикл for

### Цикл по диапазону

```bash
#!/bin/bash

# Простой цикл for
awk 'BEGIN {for(i=1; i<=5; i++) print i}'

# Цикл с шагом
awk 'BEGIN {for(i=1; i<=10; i+=2) print i}'
```

### Цикл по массиву

```bash
#!/bin/bash

# Цикл по элементам массива
awk '{for(i=1; i<=NF; i++) print $i}' input.txt

# Цикл по ассоциативному массиву
awk '{count[$1]++} END {for(key in count) print key, count[key]}' input.txt
```

## Форматированный вывод данных

### Функция printf

```bash
#!/bin/bash

# Базовый формат
awk '{printf "Имя: %s, Возраст: %d\n", $1, $2}' input.txt

# Числовые форматы
awk '{printf "%.2f\n", $3}' input.txt
```

### Специальные форматы

```bash
#!/bin/bash

# Выравнивание
awk '{printf "%-20s %10d\n", $1, $2}' input.txt

# Двоичный формат
awk '{printf "%b\n", $1}' input.txt
```

## Встроенные математические функции

### Основные функции

```bash
#!/bin/bash

# Абсолютное значение
awk '{print abs($1)}' input.txt

# Квадратный корень
awk '{print sqrt($1)}' input.txt

# Случайное число
awk 'BEGIN {print rand()}'
```

### Тригонометрические функции

```bash
#!/bin/bash

# Синус
awk '{print sin($1)}' input.txt

# Косинус
awk '{print cos($1)}' input.txt

# Тангенс
awk '{print atan2($1, $2)}' input.txt
```

## Строковые функции

### Базовые функции

```bash
#!/bin/bash

# Длина строки
awk '{print length($0)}' input.txt

# Подстрока
awk '{print substr($1, 1, 3)}' input.txt

# Индекс подстроки
awk '{print index($0, "test")}' input.txt
```

### Сложные операции

```bash
#!/bin/bash

# Замена подстроки
awk '{gsub(/old/, "new"); print}' input.txt

# Разделение строки
awk '{split($0, arr, ","); print arr[1]}' input.txt
```

## Пользовательские функции

### Определение функций

```bash
#!/bin/bash

# Простая функция
awk 'function square(x) {return x*x} {print square($1)}' input.txt

# Функция с несколькими параметрами
awk 'function sum(a,b) {return a+b} {print sum($1,$2)}' input.txt
```

### Рекурсивные функции

```bash
#!/bin/bash

# Факториал
awk 'function fact(n) {return (n<=1)?1:n*fact(n-1)} BEGIN {print fact(5)}'

# Фибоначчи
awk 'function fib(n) {return (n<=1)?n:fib(n-1)+fib(n-2)} BEGIN {print fib(10)}'
```

## Практическое задание

Создайте скрипт `awk_demo.sh`:

```bash
#!/bin/bash

# Создание тестового файла
cat > data.txt << EOF
John 25 50000
Jane 30 75000
Bob 35 60000
EOF

echo "=== Исходные данные ==="
cat data.txt

echo -e "\n=== Статистика ==="
awk '
BEGIN {print "=== Анализ данных ==="}
{sum+=$3; count++}
END {
    print "Средняя зарплата: " sum/count
    print "Всего сотрудников: " count
}' data.txt

echo -e "\n=== Форматированный вывод ==="
awk '{printf "%-10s %5d %10d\n", $1, $2, $3}' data.txt

echo -e "\n=== Фильтрация ==="
awk '$3 > 60000 {print $1, "зарплата выше 60000"}' data.txt
```

## Важные моменты

- awk обрабатывает данные построчно
- Каждая строка разбивается на поля
- Можно использовать регулярные выражения
- Поддерживаются массивы и функции
- Мощные возможности форматирования

## Дополнительные материалы

- [Awk Manual](https://www.gnu.org/software/gawk/manual/gawk.html)
- [Awk One-Liners](https://github.com/onceupon/Bash-Oneliner)

## Вопросы для самопроверки

1. Как определить разделитель полей в awk?
2. Как использовать условные операторы?
3. Как работать с массивами?
4. Как создать пользовательскую функцию?
5. Как форматировать вывод данных? 