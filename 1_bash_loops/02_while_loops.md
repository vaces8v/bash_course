# Циклы while

## Введение

Цикл while позволяет выполнять команды, пока условие истинно. В этом уроке мы рассмотрим различные способы использования цикла while и его применение для обработки данных.

## Базовый синтаксис

### Простой цикл while

```bash
while [ условие ]
do
    команды
done
```

### Примеры использования

```bash
# Счетчик
count=1
while [ $count -le 5 ]
do
    echo "Счет: $count"
    count=$((count + 1))
done

# Проверка значения
value="yes"
while [ "$value" = "yes" ]
do
    echo "Продолжить? (yes/no)"
    read value
done
```

## Примеры использования

### Обработка пользовательского ввода

```bash
# Ввод до определенного значения
while true
do
    echo "Введите число (0 для выхода):"
    read number
    if [ "$number" -eq 0 ]; then
        break
    fi
    echo "Вы ввели: $number"
done
```

### Ожидание условия

```bash
# Ожидание создания файла
while [ ! -f "file.txt" ]
do
    echo "Ожидание файла..."
    sleep 1
done

# Ожидание завершения процесса
while pgrep -f "process_name" > /dev/null
do
    echo "Процесс все еще работает..."
    sleep 2
done
```

## Вложенные циклы

### Комбинация while и for

```bash
# Внешний цикл while
outer=1
while [ $outer -le 3 ]
do
    echo "Внешний цикл: $outer"
    
    # Внутренний цикл for
    for inner in 1 2 3
    do
        echo "  Внутренний цикл: $inner"
    done
    
    outer=$((outer + 1))
done
```

### Множественные условия

```bash
# Проверка нескольких условий
while [ "$answer" != "exit" ] && [ "$count" -lt 10 ]
do
    echo "Счет: $count"
    echo "Введите 'exit' для выхода"
    read answer
    count=$((count + 1))
done
```

## Обработка содержимого файла

### Построчное чтение

```bash
# Чтение файла построчно
while IFS= read -r line
do
    echo "Строка: $line"
done < "file.txt"
```

### Обработка с разделителями

```bash
# Чтение CSV файла
while IFS=, read -r name age city
do
    echo "Имя: $name, Возраст: $age, Город: $city"
done < "data.csv"
```

## Практическое задание

Создайте скрипт `while_loops_demo.sh`:

```bash
#!/bin/bash

# 1. Простой счетчик
echo "=== Счетчик ==="
count=1
while [ $count -le 5 ]
do
    echo "Счет: $count"
    count=$((count + 1))
done

# 2. Обработка пользовательского ввода
echo -e "\n=== Пользовательский ввод ==="
while true
do
    echo "Выберите опцию:"
    echo "1) Показать текущую директорию"
    echo "2) Показать содержимое директории"
    echo "3) Выйти"
    read choice
    
    case $choice in
        1) pwd ;;
        2) ls ;;
        3) break ;;
        *) echo "Неверный выбор" ;;
    esac
done

# 3. Обработка файла
echo -e "\n=== Обработка файла ==="
if [ -f "test.txt" ]; then
    while IFS= read -r line
    do
        echo "Строка: $line"
    done < "test.txt"
else
    echo "Файл test.txt не существует"
fi

# 4. Ожидание условия
echo -e "\n=== Ожидание условия ==="
echo "Создаю файл test.txt..."
touch test.txt
echo "Файл создан!"
```

## Важные моменты

- Всегда проверяйте условие выхода из цикла
- Используйте sleep для предотвращения перегрузки CPU
- Обрабатывайте возможные ошибки
- Следите за бесконечными циклами
- Используйте break для выхода из цикла

## Дополнительные материалы

- [Bash While Loop](https://www.gnu.org/software/bash/manual/bash.html#Looping-Constructs)
- [Bash Read Command](https://www.gnu.org/software/bash/manual/bash.html#index-read)

## Вопросы для самопроверки

1. Как создать бесконечный цикл while?
2. Как правильно читать файл построчно?
3. Как использовать вложенные циклы while?
4. Как обработать пользовательский ввод в цикле while? 