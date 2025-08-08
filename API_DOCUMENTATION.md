# API Документация - Telegram Bot

## Обзор

Данная документация описывает API и функции Telegram-бота для управления анкетами.

## Основные компоненты

### 1. Инициализация и конфигурация

#### Инициализация бота
```python
bot = telebot.TeleBot("YOUR_BOT_TOKEN")
```

#### Константы администраторов
```python
MAIN_ADMIN_TELEGRAM_ID = '6664891663'
MAIN_ADMIN_PASSWORD = 'rЮ!a9*к№уD:b'
JUNIOR_ADMIN_TELEGRAM_ID = '8068244167'
JUNIOR_ADMIN_PASSWORD = '!pW&ix^;1^|Q'
```

### 2. База данных

#### Подключение к БД
```python
def get_db_connection():
    db_path = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'MegaBASE2.db')
    conn = sqlite3.connect(db_path, check_same_thread=False)
    return conn
```

#### Структура таблиц

**Таблица `users`:**
- `id` (INTEGER PRIMARY KEY)
- `telegram_id` (TEXT UNIQUE)
- `phone` (TEXT)
- `role` (TEXT)
- `status` (TEXT)
- `password` (TEXT)
- `search_count` (INTEGER DEFAULT 0)
- `date_added` (TEXT)

**Таблица `profiles`:**
- `id` (INTEGER PRIMARY KEY)
- `user_id` (INTEGER, FOREIGN KEY)
- `user_info` (TEXT)
- `type` (TEXT)
- `characteristic` (TEXT)
- `comment` (TEXT)
- `date_added` (TEXT)

**Таблица `search_history`:**
- `id` (INTEGER PRIMARY KEY)
- `user_id` (INTEGER, FOREIGN KEY)
- `search_text` (TEXT)
- `date_searched` (TEXT)

**Таблица `requests`:**
- `request_id` (INTEGER PRIMARY KEY)
- `user_id` (INTEGER UNIQUE)
- `username` (TEXT)
- `is_approved` (INTEGER DEFAULT 0)
- `role` (TEXT DEFAULT 'user')
- `created_at` (DATETIME DEFAULT CURRENT_TIMESTAMP)

### 3. Сессионное управление

#### Структура сессии
```python
session_data = {
    "characteristics": set(),      # Характеристики анкеты
    "profile_type": None,         # Тип анкеты
    "user_info": {                # Информация о пользователе
        "phone": None,            # Номер телефона
        "telegram_id": None       # Telegram ID
    },
    "comment": None               # Комментарий к анкете
}
```

#### Функции управления сессией
```python
def initialize_session(chat_id):
    """Инициализация данных сессии для указанного chat_id"""

def clear_session(chat_id):
    """Очистка данных сессии"""
```

### 4. Основные функции

#### Аутентификация
```python
@bot.message_handler(commands=['start'])
def start(message):
    """Обработчик команды /start"""

def verify_password(message):
    """Проверка пароля пользователя"""
```

#### Главное меню
```python
def main_menu(message):
    """Отображение главного меню с статистикой"""
```

#### Добавление анкет
```python
def add_information(message):
    """Начало процесса добавления анкеты"""

def process_user_info(message):
    """Обработка ввода данных пользователя"""

def process_profile_type_selection(call):
    """Обработка выбора типа анкеты"""

def process_characteristic_selection(call):
    """Обработка выбора характеристик"""

def save_profile(chat_id, user_info, profile_type, characteristics, comment):
    """Сохранение анкеты в базу данных"""
```

#### Поиск
```python
def search_request(message):
    """Запрос для поиска"""

def perform_search(message):
    """Выполнение поиска пользователя"""

def display_search_results(message, page, edit_message=True):
    """Отображение результатов поиска с пагинацией"""
```

#### Управление заявками
```python
@bot.callback_query_handler(func=lambda call: call.data == "просмотр_заявок")
def show_requests_callback(call):
    """Просмотр заявок на регистрацию"""

def process_request(call):
    """Обработка заявок (одобрение/отклонение)"""
```

### 5. Валидация и нормализация

#### Нормализация телефона
```python
def normalize_phone(phone):
    """Приводит номер телефона к формату +7XXXXXXXXXX"""
```

#### Валидация ввода
```python
def validate_input(input_text):
    """Валидирует и нормализует ввод Telegram ID и номера телефона"""
```

### 6. Безопасность

#### Хеширование паролей
```python
def hash_password(password):
    """Хеширование пароля с использованием SHA-256"""
    return hashlib.sha256(password.encode()).hexdigest()
```

### 7. Обработчики событий

#### Callback-обработчики
```python
@bot.callback_query_handler(func=lambda call: call.data in ["поиск", "Добавить", "Главное меню", "Выход"])
def handle_main_menu_buttons(call):
    """Обработка кнопок главного меню"""

@bot.callback_query_handler(func=lambda call: call.data.startswith("type_"))
def process_profile_type_selection(call):
    """Обработка выбора типа анкеты"""

@bot.callback_query_handler(func=lambda call: call.data.startswith("character_"))
def process_characteristic_selection(call):
    """Обработка выбора характеристик"""

@bot.callback_query_handler(func=lambda call: call.data.startswith("page_"))
def handle_page_navigation(call):
    """Навигация по страницам результатов"""
```

### 8. Типы анкет и характеристики

#### Типы анкет
- `Подходящий🟩`: Положительные характеристики
- `Проблемный🟥`: Отрицательные характеристики
- `Не был⬛️`: Отсутствие информации
- `Вирт🟦`: Виртуальное взаимодействие

#### Характеристики для подходящих анкет
- `Вежливый😊`
- `Чистоплотный💧`
- `Красивый✨`
- `Большой>15`
- `Маленький<15`
- `Толстый🍔`
- `Худой🧍‍♂️`
- `Известный🔥`

#### Характеристики для проблемных анкет
- `Хам😠`
- `Нищий💰`
- `Дроч🤦`
- `Наркоман💉`
- `Вымогатель💀`
- `Мент👮`

### 9. Обработка ошибок

#### Основные типы ошибок
- `sqlite3.Error`: Ошибки базы данных
- `ValueError`: Ошибки валидации
- `ConnectionError`: Ошибки подключения
- `telebot.apihelper.ApiException`: Ошибки Telegram API

#### Логирование
```python
print(f"Ошибка: {e}")  # Базовое логирование ошибок
```

### 10. Конфигурация

#### Параметры пагинации
```python
PAGE_SIZE = 3  # Количество анкет на странице
```

#### Настройки безопасности
- Хеширование паролей (SHA-256)
- Валидация всех входных данных
- Проверка прав доступа
- Защита от SQL-инъекций

## Примеры использования

### Добавление новой анкеты
1. Пользователь отправляет команду `/start`
2. Выбирает "Добавить" в главном меню
3. Вводит Telegram ID и/или номер телефона
4. Выбирает тип анкеты
5. Выбирает характеристики (если применимо)
6. Вводит комментарий
7. Анкета сохраняется в базу данных

### Поиск анкет
1. Пользователь выбирает "Поиск" в главном меню
2. Вводит Telegram ID или номер телефона
3. Просматривает результаты с пагинацией
4. Может переходить между страницами

### Управление заявками (для администраторов)
1. Администратор выбирает "Просмотр заявок"
2. Просматривает список заявок
3. Одобряет или отклоняет заявки
4. Назначает роли пользователям
