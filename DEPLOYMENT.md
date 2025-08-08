# Инструкции по развертыванию

## Требования к системе

### Минимальные требования
- Python 3.7 или выше
- 512 MB RAM
- 100 MB свободного места на диске
- Стабильное интернет-соединение

### Рекомендуемые требования
- Python 3.9+
- 1 GB RAM
- 500 MB свободного места на диске
- Высокоскоростное интернет-соединение

## Установка зависимостей

### 1. Создание виртуального окружения
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/MacOS
python3 -m venv venv
source venv/bin/activate
```

### 2. Установка пакетов
```bash
pip install pyTelegramBotAPI
pip install sqlite3
```

### 3. Проверка установки
```bash
python -c "import telebot; print('Telegram Bot API установлен успешно')"
```

## Настройка бота

### 1. Получение токена бота
1. Откройте Telegram и найдите @BotFather
2. Отправьте команду `/newbot`
3. Следуйте инструкциям для создания бота
4. Скопируйте полученный токен

### 2. Настройка конфигурации
Откройте файл `projeckk4.py` и измените следующие параметры:

```python
# Строка 11: Замените на ваш токен
bot = telebot.TeleBot("YOUR_BOT_TOKEN")

# Строки 14-19: Настройте администраторов
MAIN_ADMIN_TELEGRAM_ID = 'YOUR_ADMIN_ID'
MAIN_ADMIN_PASSWORD = 'YOUR_ADMIN_PASSWORD'
JUNIOR_ADMIN_TELEGRAM_ID = 'YOUR_JUNIOR_ADMIN_ID'
JUNIOR_ADMIN_PASSWORD = 'YOUR_JUNIOR_ADMIN_PASSWORD'
```

### 3. Получение Telegram ID
Для получения Telegram ID:
1. Отправьте сообщение боту @userinfobot
2. Скопируйте ваш ID из ответа

## Запуск в различных средах

### Локальная разработка
```bash
# Активация виртуального окружения
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/MacOS

# Запуск бота
python projeckk4.py
```

### Запуск в фоновом режиме (Linux/MacOS)
```bash
# Использование nohup
nohup python projeckk4.py > bot.log 2>&1 &

# Использование screen
screen -S telegram_bot
python projeckk4.py
# Ctrl+A, затем D для отключения от сессии
```

### Запуск как служба (systemd)
Создайте файл `/etc/systemd/system/telegram-bot.service`:

```ini
[Unit]
Description=Telegram Bot Service
After=network.target

[Service]
Type=simple
User=your_username
WorkingDirectory=/path/to/your/bot
Environment=PATH=/path/to/your/venv/bin
ExecStart=/path/to/your/venv/bin/python projeckk4.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Затем выполните:
```bash
sudo systemctl daemon-reload
sudo systemctl enable telegram-bot
sudo systemctl start telegram-bot
```

### Docker развертывание
Создайте `Dockerfile`:

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "projeckk4.py"]
```

Создайте `requirements.txt`:
```
pyTelegramBotAPI==4.12.0
```

Соберите и запустите контейнер:
```bash
docker build -t telegram-bot .
docker run -d --name telegram-bot telegram-bot
```

## Мониторинг и логирование

### Настройка логирования
Добавьте в начало файла `projeckk4.py`:

```python
import logging

# Настройка логирования
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('bot.log'),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)
```

### Мониторинг состояния
Создайте скрипт `monitor.py`:

```python
import requests
import time
import smtplib
from email.mime.text import MIMEText

def check_bot_status():
    try:
        # Проверка доступности бота
        response = requests.get(f"https://api.telegram.org/bot{YOUR_BOT_TOKEN}/getMe")
        return response.status_code == 200
    except:
        return False

def send_alert(message):
    # Настройка отправки уведомлений
    pass

while True:
    if not check_bot_status():
        send_alert("Бот недоступен!")
    time.sleep(300)  # Проверка каждые 5 минут
```

## Резервное копирование

### Автоматическое резервное копирование
Создайте скрипт `backup.py`:

```python
import sqlite3
import shutil
import os
from datetime import datetime

def backup_database():
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    backup_path = f"backup/MegaBASE2_{timestamp}.db"
    
    os.makedirs("backup", exist_ok=True)
    shutil.copy2("MegaBASE2.db", backup_path)
    
    print(f"Резервная копия создана: {backup_path}")

if __name__ == "__main__":
    backup_database()
```

### Настройка cron для автоматического резервного копирования
```bash
# Добавьте в crontab
0 2 * * * /path/to/your/venv/bin/python /path/to/backup.py
```

## Обновление

### Процедура обновления
1. Остановите бота
2. Создайте резервную копию базы данных
3. Обновите код
4. Проверьте конфигурацию
5. Запустите бота

```bash
# Остановка
sudo systemctl stop telegram-bot  # если используется systemd
# или
pkill -f projeckk4.py

# Резервное копирование
python backup.py

# Обновление кода
git pull origin main

# Запуск
sudo systemctl start telegram-bot
```

## Устранение неполадок

### Частые проблемы

#### 1. Ошибка подключения к Telegram API
```
telebot.apihelper.ApiException: A request to the Telegram API was unsuccessful
```
**Решение**: Проверьте токен бота и интернет-соединение

#### 2. Ошибки базы данных
```
sqlite3.OperationalError: database is locked
```
**Решение**: Убедитесь, что нет других процессов, использующих базу данных

#### 3. Проблемы с правами доступа
```
PermissionError: [Errno 13] Permission denied
```
**Решение**: Проверьте права доступа к файлам и папкам

### Логи для диагностики
```bash
# Просмотр логов systemd
sudo journalctl -u telegram-bot -f

# Просмотр файла логов
tail -f bot.log

# Проверка процессов
ps aux | grep projeckk4.py
```

## Безопасность

### Рекомендации по безопасности
1. Используйте сильные пароли для администраторов
2. Ограничьте доступ к файлам конфигурации
3. Регулярно обновляйте зависимости
4. Мониторьте логи на подозрительную активность
5. Используйте HTTPS для внешних соединений

### Настройка файрвола
```bash
# Разрешить только необходимые порты
sudo ufw allow 22/tcp  # SSH
sudo ufw enable
```

## Производительность

### Оптимизация для высоких нагрузок
1. Увеличьте размер пула соединений с базой данных
2. Настройте кэширование
3. Используйте асинхронную обработку
4. Мониторьте использование ресурсов

### Мониторинг производительности
```bash
# Мониторинг CPU и памяти
htop

# Мониторинг дискового пространства
df -h

# Мониторинг сетевых соединений
netstat -tulpn
```
