# Управление циклами

## Введение

В bash есть несколько команд для управления выполнением циклов. В этом уроке мы рассмотрим команды break и continue, а также способы обработки вывода в циклах.

## Команда break

### Базовое использование

```bash
# Выход из цикла при определенном условии
for i in {1..10}
do
    if [ $i -eq 5 ]; then
        break
    fi
    echo "Число: $i"
done
```

### Выход из вложенных циклов

```bash
# Выход из внутреннего цикла
for i in {1..3}
do
    for j in {1..3}
    do
        if [ $j -eq 2 ]; then
            break
        fi
        echo "i=$i, j=$j"
    done
done

# Выход из всех циклов
for i in {1..3}
do
    for j in {1..3}
    do
        if [ $j -eq 2 ]; then
            break 2
        fi
        echo "i=$i, j=$j"
    done
done
```

## Команда continue

### Пропуск итерации

```bash
# Пропуск определенных значений
for i in {1..5}
do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Число: $i"
done
```

### Пропуск с условием

```bash
# Пропуск четных чисел
for i in {1..10}
do
    if [ $((i % 2)) -eq 0 ]; then
        continue
    fi
    echo "Нечетное число: $i"
done
```

## Обработка вывода в цикле

### Перенаправление вывода

```bash
# Сохранение вывода в файл
for file in *.txt
do
    echo "Обработка файла: $file" >> log.txt
done

# Фильтрация вывода
for process in $(ps aux)
do
    if [[ $process == *"bash"* ]]; then
        echo "$process" | grep "bash"
    fi
done
```

### Обработка ошибок

```bash
# Проверка успешности выполнения
for file in *.txt
do
    if ! grep "pattern" "$file" > /dev/null 2>&1; then
        echo "Ошибка при обработке $file"
        continue
    fi
    echo "Успешно обработан $file"
done
```

## Пример: поиск исполняемых файлов

### Базовый поиск

```bash
#!/bin/bash

# Поиск исполняемых файлов
for file in $(find . -type f)
do
    if [ -x "$file" ]; then
        echo "Исполняемый файл: $file"
    fi
done
```

### Расширенный поиск

```bash
#!/bin/bash

# Поиск с дополнительными проверками
for file in $(find . -type f)
do
    # Пропускаем скрытые файлы
    if [[ "$file" == .* ]]; then
        continue
    fi
    
    # Проверяем права на выполнение
    if [ -x "$file" ]; then
        # Получаем информацию о файле
        size=$(stat -f%z "$file")
        owner=$(stat -f%Su "$file")
        
        echo "=== Исполняемый файл ==="
        echo "Путь: $file"
        echo "Размер: $size байт"
        echo "Владелец: $owner"
        echo "-------------------"
    fi
done
```

## Практическое задание

Создайте скрипт `loop_control_demo.sh`:

```bash
#!/bin/bash

# 1. Демонстрация break
echo "=== Демонстрация break ==="
for i in {1..5}
do
    if [ $i -eq 3 ]; then
        echo "Выход из цикла при i=3"
        break
    fi
    echo "i=$i"
done

# 2. Демонстрация continue
echo -e "\n=== Демонстрация continue ==="
for i in {1..5}
do
    if [ $i -eq 3 ]; then
        echo "Пропуск i=3"
        continue
    fi
    echo "i=$i"
done

# 3. Обработка вывода
echo -e "\n=== Обработка вывода ==="
for file in *.sh
do
    if [ -f "$file" ]; then
        echo "Обработка файла: $file" >> output.log
        if [ -x "$file" ]; then
            echo "Файл исполняемый" >> output.log
        else
            echo "Файл не исполняемый" >> output.log
        fi
    fi
done

# 4. Поиск исполняемых файлов
echo -e "\n=== Поиск исполняемых файлов ==="
for file in $(find . -type f)
do
    if [ -x "$file" ]; then
        echo "Найден исполняемый файл: $file"
    fi
done
```

## Важные моменты

- Используйте break для преждевременного выхода из цикла
- Используйте continue для пропуска текущей итерации
- Обрабатывайте ошибки в циклах
- Следите за перенаправлением вывода
- Проверяйте существование файлов перед обработкой

## Дополнительные материалы

- [Bash Break Command](https://www.gnu.org/software/bash/manual/bash.html#index-break)
- [Bash Continue Command](https://www.gnu.org/software/bash/manual/bash.html#index-continue)

## Вопросы для самопроверки

1. В чем разница между break и continue?
2. Как выйти из вложенных циклов?
3. Как обработать ошибки в цикле?
4. Как перенаправить вывод цикла в файл? 