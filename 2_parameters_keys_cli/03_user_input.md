# Получение данных от пользователя

## Введение

В bash-скриптах есть несколько способов получения данных от пользователя. В этом уроке мы рассмотрим различные методы ввода данных и их обработку.

## Ввод паролей

### Безопасный ввод пароля

```bash
#!/bin/bash

# Ввод пароля без отображения символов
read -s -p "Введите пароль: " password
echo

# Проверка пароля
if [ "$password" = "secret" ]; then
    echo "Пароль верный"
else
    echo "Неверный пароль"
fi
```

### Ввод с таймаутом

```bash
#!/bin/bash

# Ввод пароля с таймаутом
read -t 10 -s -p "Введите пароль (10 секунд): " password
echo

if [ -z "$password" ]; then
    echo "Время ввода истекло"
    exit 1
fi
```

## Чтение данных из файла

### Построчное чтение

```bash
#!/bin/bash

# Чтение файла построчно
while IFS= read -r line
do
    echo "Строка: $line"
done < "file.txt"
```

### Чтение с разделителями

```bash
#!/bin/bash

# Чтение CSV файла
while IFS=, read -r name age city
do
    echo "Имя: $name"
    echo "Возраст: $age"
    echo "Город: $city"
    echo "---"
done < "data.csv"
```

### Обработка ошибок

```bash
#!/bin/bash

# Проверка существования файла
if [ ! -f "file.txt" ]; then
    echo "Ошибка: файл не найден"
    exit 1
fi

# Чтение с обработкой ошибок
while IFS= read -r line || [ -n "$line" ]
do
    echo "Строка: $line"
done < "file.txt"
```

## Практическое задание

Создайте скрипт `user_input_demo.sh`:

```bash
#!/bin/bash

# 1. Ввод пароля
echo "=== Ввод пароля ==="
read -s -p "Введите пароль: " password
echo

if [ "$password" = "secret" ]; then
    echo "Пароль верный"
else
    echo "Неверный пароль"
fi

# 2. Ввод с таймаутом
echo -e "\n=== Ввод с таймаутом ==="
read -t 5 -p "Введите что-нибудь (5 секунд): " input
if [ -z "$input" ]; then
    echo "Время ввода истекло"
else
    echo "Вы ввели: $input"
fi

# 3. Чтение файла
echo -e "\n=== Чтение файла ==="
if [ -f "test.txt" ]; then
    while IFS= read -r line
    do
        echo "Строка: $line"
    done < "test.txt"
else
    echo "Файл test.txt не существует"
    # Создаем тестовый файл
    echo "Создаю тестовый файл..."
    echo "Строка 1" > test.txt
    echo "Строка 2" >> test.txt
    echo "Строка 3" >> test.txt
    echo "Файл создан!"
fi

# 4. Чтение CSV
echo -e "\n=== Чтение CSV ==="
if [ -f "data.csv" ]; then
    while IFS=, read -r name age city
    do
        echo "=== Запись ==="
        echo "Имя: $name"
        echo "Возраст: $age"
        echo "Город: $city"
    done < "data.csv"
else
    echo "Файл data.csv не существует"
    # Создаем тестовый CSV файл
    echo "Создаю тестовый CSV файл..."
    echo "John,25,New York" > data.csv
    echo "Jane,30,London" >> data.csv
    echo "Bob,35,Paris" >> data.csv
    echo "Файл создан!"
fi
```

## Важные моменты

- Всегда проверяйте существование файлов
- Обрабатывайте ошибки ввода
- Используйте таймауты для интерактивных скриптов
- Следите за разделителями при чтении файлов
- Проверяйте корректность введенных данных

## Дополнительные материалы

- [Bash Read Command](https://www.gnu.org/software/bash/manual/bash.html#index-read)
- [Bash Input/Output](https://www.gnu.org/software/bash/manual/bash.html#Redirections)

## Вопросы для самопроверки

1. Как безопасно ввести пароль?
2. Как реализовать таймаут при вводе?
3. Как читать файл построчно?
4. Как обработать ошибки при чтении файла? 