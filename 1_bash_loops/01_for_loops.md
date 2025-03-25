# Циклы for

## Введение

Цикл for - это один из самых мощных инструментов в bash-скриптах. Он позволяет выполнять повторяющиеся действия с различными наборами данных. В этом уроке мы рассмотрим различные способы использования цикла for.

## Перебор простых значений

### Базовый синтаксис

```bash
for var in list
do
    команды
done
```

### Примеры использования

```bash
# Перебор списка слов
for color in red green blue
do
    echo "Цвет: $color"
done

# Перебор чисел
for i in 1 2 3 4 5
do
    echo "Число: $i"
done
```

## Перебор сложных значений

### Строки с пробелами

```bash
# Использование кавычек
for name in "John Smith" "Jane Doe" "Bob Wilson"
do
    echo "Имя: $name"
done

# Использование экранирования
for name in John\ Smith Jane\ Doe Bob\ Wilson
do
    echo "Имя: $name"
done
```

### Список значений с шагом

```bash
# Использование seq
for i in $(seq 1 2 10)
do
    echo "Число: $i"
done

# Использование brace expansion
for i in {1..10..2}
do
    echo "Число: $i"
done
```

## Инициализация цикла списком из команды

### Использование вывода команды

```bash
# Перебор файлов
for file in $(ls *.txt)
do
    echo "Файл: $file"
done

# Перебор процессов
for process in $(ps aux | grep bash)
do
    echo "Процесс: $process"
done
```

### Использование подстановки команд

```bash
# Перебор строк из файла
for line in $(cat file.txt)
do
    echo "Строка: $line"
done

# Перебор результатов поиска
for file in $(find . -name "*.sh")
do
    echo "Скрипт: $file"
done
```

## Разделители полей

### Изменение разделителя

```bash
# Установка разделителя
IFS=$'\n'
for line in $(cat file.txt)
do
    echo "Строка: $line"
done

# Восстановление разделителя
IFS=$' \t\n'
```

### Обработка CSV

```bash
# Обработка CSV файла
IFS=','
for field in $(cat data.csv)
do
    echo "Поле: $field"
done
```

## Обход файлов в директории

### Перебор всех файлов

```bash
# Перебор всех файлов
for file in *
do
    echo "Файл: $file"
done

# Перебор только определенных файлов
for file in *.txt
do
    echo "Текстовый файл: $file"
done
```

### Рекурсивный обход

```bash
# Рекурсивный обход директорий
for file in $(find . -type f)
do
    echo "Найден файл: $file"
done
```

## Циклы for в стиле C

### Базовый синтаксис

```bash
for (( i=1; i<=5; i++ ))
do
    echo "Число: $i"
done
```

### Примеры использования

```bash
# Обратный отсчет
for (( i=10; i>=1; i-- ))
do
    echo "Осталось: $i"
done

# С шагом
for (( i=1; i<=10; i+=2 ))
do
    echo "Число: $i"
done
```

## Практическое задание

Создайте скрипт `for_loops_demo.sh`:

```bash
#!/bin/bash

# 1. Перебор простых значений
echo "=== Простые значения ==="
for color in red green blue
do
    echo "Цвет: $color"
done

# 2. Перебор чисел
echo -e "\n=== Числа ==="
for i in {1..5}
do
    echo "Число: $i"
done

# 3. Перебор файлов
echo -e "\n=== Файлы ==="
for file in *.sh
do
    echo "Скрипт: $file"
done

# 4. Цикл в стиле C
echo -e "\n=== Цикл в стиле C ==="
for (( i=1; i<=5; i++ ))
do
    echo "Итерация: $i"
done

# 5. Обработка CSV
echo -e "\n=== Обработка CSV ==="
IFS=','
for field in "name,age,city"
do
    echo "Поле: $field"
done
IFS=$' \t\n'
```

## Важные моменты

- Всегда используйте кавычки при работе со строками
- Следите за разделителями полей (IFS)
- Проверяйте существование файлов перед обработкой
- Используйте подходящий тип цикла для задачи
- Обрабатывайте возможные ошибки

## Дополнительные материалы

- [Bash For Loop](https://www.gnu.org/software/bash/manual/bash.html#Looping-Constructs)
- [Bash Brace Expansion](https://www.gnu.org/software/bash/manual/bash.html#Brace-Expansion)

## Вопросы для самопроверки

1. В чем разница между простым циклом for и циклом в стиле C?
2. Как изменить разделитель полей в цикле?
3. Как перебрать все файлы в директории?
4. Как создать цикл с шагом? 