# 🚀 Быстрый старт CarTaxi

## Запуск проекта за 5 минут

---

## 📦 Требования

### Для Backend:
- Python 3.10+
- pip

### Для Frontend (Landing):
- Любой веб-браузер
- Live Server (опционально)

### Для Mobile App:
- Flutter 3.27+
- Android Studio / VS Code
- Android SDK / Xcode (для iOS)

---

## 🔧 Установка и запуск

### 1️⃣ Backend (FastAPI)

#### Windows:

```powershell
# Переходим в папку backend
cd C:\Users\User\Desktop\эвакуаторы\эвакуаторы\backend

# Создаем виртуальное окружение (если не создано)
python -m venv venv

# Активируем виртуальное окружение
.\venv\Scripts\activate

# Устанавливаем зависимости
pip install -r requirements.txt

# Запускаем сервер
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Backend будет доступен по адресу: **http://localhost:8000**  
API документация: **http://localhost:8000/docs**

---

### 2️⃣ Landing (Web)

#### Способ 1: Просто открыть файл

```powershell
# Открываем landing в браузере
start C:\Users\User\Desktop\эвакуаторы\landing\index.html
```

#### Способ 2: С помощью Live Server (рекомендуется)

1. Установите **VS Code** с расширением **Live Server**
2. Откройте папку `C:\Users\User\Desktop\эвакуаторы\landing`
3. Нажмите правой кнопкой на `index.html` → **Open with Live Server**

Landing будет доступен: **http://localhost:5500**

---

### 3️⃣ Mobile App (Flutter)

```powershell
# Переходим в папку приложения
cd C:\Users\User\Desktop\эвакуаторы\эвакуаторы

# Устанавливаем зависимости
flutter pub get

# Проверяем подключенные устройства
flutter devices

# Запускаем приложение (Android эмулятор / физическое устройство)
flutter run

# Для конкретной платформы:
flutter run -d android
flutter run -d chrome  # Для веб-версии
```

---

## 📝 Настройка API для мобильного приложения

Откройте файл `lib/logic/api/api_client.dart` и измените URL:

```dart
// Для Android эмулятора:
const String apiBaseUrl = 'http://10.0.2.2:8000';

// Для реального устройства (замените на IP вашего компьютера):
const String apiBaseUrl = 'http://192.168.1.X:8000';

// Для iOS симулятора:
const String apiBaseUrl = 'http://localhost:8000';
```

Чтобы узнать IP адрес вашего компьютера:

```powershell
# Windows
ipconfig

# Ищите "IPv4 Address" в разделе вашей активной сети
```

---

## 🧪 Тестовые данные

### Создание тестового водителя:

```bash
# С помощью curl (Windows PowerShell)
Invoke-RestMethod -Uri "http://localhost:8000/api/drivers/register" `
  -Method POST `
  -ContentType "application/json" `
  -Body '{
    "name": "Иван Иванов",
    "phone_number": "+79991234567",
    "email": "driver@test.ru",
    "license_number": "1234567890",
    "passport_number": "1234 123456"
  }'
```

Или через API документацию: **http://localhost:8000/docs**

---

## 🎯 Основные endpoints API

### Для клиентов:

```
GET  /api/services/          - Список услуг
POST /api/orders/            - Создать заказ
GET  /api/orders/{order_id}  - Получить заказ
GET  /api/orders/user/{phone} - История заказов
```

### Для водителей:

```
POST /api/drivers/register               - Регистрация водителя
GET  /api/drivers/{driver_id}            - Профиль водителя
POST /api/drivers/{driver_id}/status     - Изменить статус (онлайн/оффлайн)
GET  /api/drivers/{driver_id}/available-orders - Доступные заказы
POST /api/drivers/{driver_id}/accept-order/{order_id} - Принять заказ
GET  /api/drivers/{driver_id}/stats      - Статистика водителя
POST /api/drivers/{driver_id}/withdraw   - Вывод средств
```

### Для администраторов:

```
GET  /api/drivers/              - Список всех водителей
GET  /api/orders/               - Список всех заказов
```

---

## 📱 Тестирование функционала

### 1. Тест создания заказа:

1. Откройте landing: http://localhost:5500
2. Нажмите "Рассчитать стоимость"
3. Выберите тип транспорта и расстояние
4. Нажмите "Заказать эвакуатор"

### 2. Тест для водителя:

1. Зарегистрируйте водителя через API
2. Запустите мобильное приложение
3. Войдите как водитель (используя ID из ответа регистрации)
4. Переключите статус в "Онлайн"
5. Примите доступный заказ

### 3. Тест геолокации:

1. В мобильном приложении включите геолокацию
2. API автоматически обновит позицию водителя
3. Система найдет ближайшие заказы

---

## 🔍 Проверка работы

### Backend работает правильно, если:
- ✅ http://localhost:8000 возвращает `{"status": "CarTaxi Evacuator API is running!"}`
- ✅ http://localhost:8000/docs открывается Swagger UI
- ✅ http://localhost:8000/health возвращает `{"status": "healthy"}`

### Landing работает правильно, если:
- ✅ Страница открывается без ошибок
- ✅ Калькулятор рассчитывает стоимость
- ✅ Формы отправки работают

### Mobile App работает правильно, если:
- ✅ Приложение запускается без ошибок
- ✅ Карта отображается
- ✅ API запросы выполняются успешно

---

## 🐛 Решение проблем

### Backend не запускается:

```powershell
# Проверьте версию Python
python --version  # Должна быть 3.10+

# Переустановите зависимости
pip install --upgrade -r requirements.txt

# Проверьте, свободен ли порт 8000
netstat -ano | findstr :8000
```

### Ошибка БД:

```powershell
# Удалите и пересоздайте БД
cd C:\Users\User\Desktop\эвакуаторы\эвакуаторы\backend
Remove-Item groceries.db -ErrorAction SilentlyContinue

# Перезапустите backend - БД создастся автоматически
```

### Flutter не находит устройства:

```powershell
# Проверьте подключенные устройства
flutter devices

# Для Android эмулятора - запустите его через Android Studio
# Для Chrome:
flutter run -d chrome
```

### Приложение не подключается к API:

1. Проверьте, что backend запущен
2. Проверьте IP адрес в `api_client.dart`
3. Для Android эмулятора используйте `10.0.2.2` вместо `localhost`
4. Убедитесь, что firewall не блокирует порт 8000

---

## 📊 Мониторинг

### Логи Backend:

Backend выводит логи в консоль. Уровень логирования: DEBUG.

### Логи Flutter:

```powershell
# Просмотр логов приложения
flutter logs
```

### База данных:

```powershell
# Просмотр БД (SQLite)
# Установите DB Browser for SQLite: https://sqlitebrowser.org/

# Откройте файл:
# C:\Users\User\Desktop\эвакуаторы\эвакуаторы\backend\groceries.db
```

---

## 🚀 Запуск в продакшен

### Backend:

```powershell
# Установите Gunicorn (production ASGI server)
pip install gunicorn

# Запустите с несколькими workers
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker
```

### Landing:

Разместите на:
- **GitHub Pages** (бесплатно)
- **Netlify** (бесплатно)
- **Vercel** (бесплатно)
- Свой VPS/хостинг

### Mobile App:

```powershell
# Android APK
flutter build apk --release

# iOS IPA (только на macOS)
flutter build ios --release
```

---

## 📞 Поддержка

Если возникли проблемы:

1. Проверьте файл `DEVELOPMENT_PLAN.md`
2. Проверьте логи в консоли
3. Откройте Issue в репозитории
4. Напишите в Telegram: @cartaxi

---

## ✅ Checklist первого запуска

- [ ] Python 3.10+ установлен
- [ ] Backend запущен на порту 8000
- [ ] API документация открывается (http://localhost:8000/docs)
- [ ] Landing открывается в браузере
- [ ] Flutter SDK установлен
- [ ] Мобильное приложение запускается
- [ ] Создан тестовый водитель через API
- [ ] Тестовый заказ создается успешно

---

**Готово!** 🎉 Теперь у вас работает полноценная система CarTaxi!

Следующие шаги:
1. Изучите `DEVELOPMENT_PLAN.md` для плана развития
2. Настройте платежные системы
3. Добавьте push-уведомления
4. Запустите тестирование с реальными пользователями






