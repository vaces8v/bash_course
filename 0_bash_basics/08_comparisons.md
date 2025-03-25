# Сравнения

## Введение

В bash есть различные операторы для сравнения чисел и строк. В этом уроке мы рассмотрим все типы сравнений и их особенности.

## Сравнение чисел

### Операторы сравнения чисел

```bash
# Равно
if [ $a -eq $b ]; then
    echo "a равно b"
fi

# Не равно
if [ $a -ne $b ]; then
    echo "a не равно b"
fi

# Больше
if [ $a -gt $b ]; then
    echo "a больше b"
fi

# Меньше
if [ $a -lt $b ]; then
    echo "a меньше b"
fi

# Больше или равно
if [ $a -ge $b ]; then
    echo "a больше или равно b"
fi

# Меньше или равно
if [ $a -le $b ]; then
    echo "a меньше или равно b"
fi
```

## Сравнение строк

### Операторы сравнения строк

```bash
# Равно
if [ "$str1" = "$str2" ]; then
    echo "Строки равны"
fi

# Не равно
if [ "$str1" != "$str2" ]; then
    echo "Строки не равны"
fi

# Строка не пустая
if [ -n "$str" ]; then
    echo "Строка не пустая"
fi

# Строка пустая
if [ -z "$str" ]; then
    echo "Строка пустая"
fi

# Строка начинается с подстроки
if [[ "$str" == "prefix"* ]]; then
    echo "Строка начинается с 'prefix'"
fi

# Строка содержит подстроку
if [[ "$str" == *"substring"* ]]; then
    echo "Строка содержит 'substring'"
fi
```

## Практическое задание

Создайте скрипт `comparisons_demo.sh`:

```bash
#!/bin/bash

# Сравнение чисел
echo "=== Сравнение чисел ==="
a=10
b=5

echo "a = $a, b = $b"
if [ $a -gt $b ]; then
    echo "$a больше $b"
fi
if [ $a -ne $b ]; then
    echo "$a не равно $b"
fi

# Сравнение строк
echo -e "\n=== Сравнение строк ==="
str1="Hello"
str2="World"
str3="Hello World"

echo "str1 = '$str1'"
echo "str2 = '$str2'"
echo "str3 = '$str3'"

if [ "$str1" = "Hello" ]; then
    echo "str1 равно 'Hello'"
fi

if [ -n "$str2" ]; then
    echo "str2 не пустая"
fi

if [[ "$str3" == "Hello"* ]]; then
    echo "str3 начинается с 'Hello'"
fi

if [[ "$str3" == *"World" ]]; then
    echo "str3 заканчивается на 'World'"
fi

# Сравнение с регулярными выражениями
echo -e "\n=== Сравнение с регулярными выражениями ==="
email="user@example.com"
if [[ "$email" =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "Валидный email адрес"
else
    echo "Невалидный email адрес"
fi
```

## Важные моменты

- Всегда используйте кавычки при сравнении строк
- Используйте `[[ ]]` для расширенных сравнений строк
- Проверяйте переменные на пустоту перед сравнением
- Используйте `=~` для сравнения с регулярными выражениями
- Следите за пробелами в условиях

## Дополнительные материалы

- [Bash Conditional Expressions](https://www.gnu.org/software/bash/manual/bash.html#Bash-Conditional-Expressions)
- [String Comparison in Bash](https://stackoverflow.com/questions/12791077/bash-string-comparison)

## Вопросы для самопроверки

1. В чем разница между `=` и `==` при сравнении строк?
2. Как проверить, что строка пустая?
3. Как использовать регулярные выражения в сравнениях?
4. Почему важно использовать кавычки при сравнении строк? 