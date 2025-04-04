# Подстановка команд

## Введение

Подстановка команд позволяет сохранить вывод команды в переменную или использовать его как часть другой команды. В этом уроке мы изучим различные способы подстановки команд в bash.

## Синтаксис подстановки команд

Есть два способа выполнить подстановку команд:

1. **С помощью обратных кавычек (\`\`)**:
```bash
current_date=`date`
```

2. **С помощью $()** (рекомендуемый способ):
```bash
current_date=$(date)
```

## Примеры использования

### Сохранение вывода в переменную

```bash
# Получение текущей даты
current_date=$(date)

# Получение списка файлов
files=$(ls)

# Получение имени пользователя
user=$(whoami)
```

### Использование в командах

```bash
# Подсчет количества файлов
file_count=$(ls | wc -l)

# Проверка размера файла
file_size=$(stat -f%z "file.txt")

# Получение IP-адреса
ip_address=$(curl -s ifconfig.me)
```

## Практическое задание

Создайте скрипт `command_substitution_demo.sh`:

```bash
#!/bin/bash

# Получение системной информации
echo "=== Системная информация ==="
echo "Текущая дата: $(date)"
echo "Имя пользователя: $(whoami)"
echo "Текущая директория: $(pwd)"

# Работа с файлами
echo -e "\n=== Информация о файлах ==="
echo "Количество файлов в текущей директории: $(ls | wc -l)"
echo "Размер текущей директории: $(du -sh . | cut -f1)"

# Сетевые операции
echo -e "\n=== Сетевая информация ==="
echo "Внешний IP: $(curl -s ifconfig.me)"
echo "Локальный IP: $(hostname -I | awk '{print $1}')"

# Системные ресурсы
echo -e "\n=== Системные ресурсы ==="
echo "Использование CPU: $(top -bn1 | grep "Cpu(s)" | awk '{print $2}')%"
echo "Свободная память: $(free -m | grep Mem | awk '{print $4}')MB"
```

## Важные моменты

- Используйте `$()` вместо обратных кавычек
- Проверяйте успешность выполнения команд
- Обрабатывайте возможные ошибки
- Используйте подстановку команд для динамических значений

## Обработка ошибок

```bash
# Проверка успешности выполнения команды
if ! command_output=$(command); then
    echo "Ошибка выполнения команды"
    exit 1
fi

# Использование значения по умолчанию
backup_dir=${BACKUP_DIR:-$(pwd)/backups}
```

## Дополнительные материалы

- [Command Substitution](https://www.gnu.org/software/bash/manual/bash.html#Command-Substitution)
- [Bash Error Handling](https://wiki.bash-hackers.org/scripting/obsolete)

## Вопросы для самопроверки

1. В чем разница между `$()` и обратными кавычками?
2. Как обработать ошибку при подстановке команды?
3. Как использовать значение по умолчанию при подстановке команды?
4. Какие преимущества у подстановки команд перед прямым использованием вывода? 