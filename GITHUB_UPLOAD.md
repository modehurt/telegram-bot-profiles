# Инструкция по загрузке в GitHub

## Шаги для загрузки проекта в GitHub

### 1. Создание репозитория на GitHub
1. Перейдите на [GitHub.com](https://github.com)
2. Нажмите кнопку "New repository"
3. Введите название репозитория (например: `telegram-bot-profiles`)
4. Выберите "Public" или "Private"
5. НЕ инициализируйте с README (у нас уже есть)
6. Нажмите "Create repository"

### 2. Подключение локального репозитория к GitHub
После создания репозитория на GitHub, выполните следующие команды:

```bash
# Добавить удаленный репозиторий (замените YOUR_USERNAME и REPO_NAME)
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# Переименовать ветку в main (современный стандарт)
git branch -M main

# Отправить код в GitHub
git push -u origin main
```

### 3. Альтернативный способ (если репозиторий уже создан)
Если вы уже создали репозиторий с README, выполните:

```bash
# Добавить удаленный репозиторий
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# Получить изменения с GitHub
git pull origin main --allow-unrelated-histories

# Отправить код
git push -u origin main
```

### 4. Проверка загрузки
После успешной загрузки:
1. Перейдите на страницу вашего репозитория на GitHub
2. Убедитесь, что все файлы загружены:
   - `projeckk4.py` - основной файл бота
   - `README.md` - основная документация
   - `CHANGELOG.md` - история изменений
   - `API_DOCUMENTATION.md` - техническая документация
   - `DEPLOYMENT.md` - инструкции по развертыванию
   - `requirements.txt` - зависимости
   - `.gitignore` - исключения для Git

### 5. Настройка GitHub Pages (опционально)
Для создания веб-страницы с документацией:

1. Перейдите в Settings репозитория
2. Прокрутите до раздела "Pages"
3. В "Source" выберите "Deploy from a branch"
4. Выберите ветку "main" и папку "/ (root)"
5. Нажмите "Save"

### 6. Добавление описания репозитория
В настройках репозитория добавьте:
- **Description**: "Telegram Bot для управления анкетами пользователей с системой ролей и поиском"
- **Topics**: telegram-bot, python, sqlite, profiles, search-system

### 7. Создание релиза (опционально)
1. Перейдите в раздел "Releases"
2. Нажмите "Create a new release"
3. Введите тег версии: `v1.0.0`
4. Заголовок: "Initial Release"
5. Описание: скопируйте содержимое из `CHANGELOG.md`
6. Нажмите "Publish release"

## Структура загруженных файлов

```
telegram-bot-profiles/
├── projeckk4.py          # Основной файл бота (1412 строк)
├── README.md             # Основная документация
├── CHANGELOG.md          # История изменений
├── API_DOCUMENTATION.md  # Техническая документация API
├── DEPLOYMENT.md         # Инструкции по развертыванию
├── requirements.txt      # Зависимости проекта
├── .gitignore           # Исключения для Git
└── GITHUB_UPLOAD.md     # Эта инструкция
```

## Важные замечания

### Безопасность
- Файл `projeckk4.py` содержит реальные токены и пароли
- Рекомендуется создать отдельный файл конфигурации
- Обновите токены перед публикацией

### Конфиденциальность
- База данных `MegaBASE2.db` исключена из загрузки (.gitignore)
- Логи и резервные копии также исключены
- Токены и ключи защищены

### Дальнейшая разработка
После загрузки в GitHub:
1. Создайте ветки для новых функций
2. Используйте Pull Requests для слияния
3. Добавляйте тесты для новых функций
4. Обновляйте документацию при изменениях

## Полезные команды Git

```bash
# Проверить статус
git status

# Посмотреть историю коммитов
git log --oneline

# Создать новую ветку
git checkout -b feature/new-feature

# Переключиться между ветками
git checkout main

# Обновить локальный репозиторий
git pull origin main

# Отправить изменения
git push origin main
```
