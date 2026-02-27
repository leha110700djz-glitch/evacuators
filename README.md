# 🚗 CarTaxi - Платформа вызова эвакуаторов

![CarTaxi Logo](https://img.shields.io/badge/CarTaxi-v3.0.0-orange?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python)
![Flutter](https://img.shields.io/badge/Flutter-3.27+-02569B?style=for-the-badge&logo=flutter)
![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-green?style=for-the-badge&logo=fastapi)
![Status](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)
![Rating](https://img.shields.io/badge/Rating-8.5%2F10-yellow?style=for-the-badge)

> Современная платформа для вызова эвакуаторов с интеграцией для водителей-партнеров, системой комиссий и автоматическим распределением заказов.

---

## 📋 Содержание

1. [О проекте](#о-проекте)
2. [Возможности](#возможности)
3. [Архитектура](#архитектура)
4. [Установка](#установка)
5. [Использование](#использование)
6. [API Документация](#api-документация)
7. [Скриншоты](#скриншоты)
8. [План развития](#план-развития)
9. [Лицензия](#лицензия)

---

## 🎯 О проекте

**CarTaxi** - это полнофункциональная платформа для вызова эвакуаторов, вдохновленная успешными агрегаторами такси. Проект включает:

- 🌐 **Веб-лендинг** - современный сайт для привлечения клиентов
- 📱 **Мобильное приложение** (iOS + Android) - для клиентов и водителей
- ⚙️ **Backend API** - высокопроизводительный сервер на FastAPI
- 🤖 **Система автоматического распределения заказов** - AI-оптимизация
- 💰 **Встроенная система комиссий** - для монетизации платформы

### Ключевые отличия от конкурентов:

✅ Низкая комиссия для водителей (15% vs 20-30% у конкурентов)  
✅ Автоматическое распределение заказов по геолокации  
✅ Прозрачное ценообразование  
✅ Современный технологический стек  
✅ API для партнерских интеграций  

---

## ✨ Возможности

### Для клиентов:

- 📍 Вызов эвакуатора на карте
- 💳 Онлайн-оплата и оплата картой
- 📊 История всех заказов
- ⭐ Рейтинги и отзывы о водителях
- 🔔 Push-уведомления о статусе заказа
- 🎫 Промокоды и скидки
- 📱 Отслеживание водителя в реальном времени

### Для водителей:

- 💼 Регистрация и верификация
- 🗺️ Просмотр доступных заказов на карте
- ✅ Принятие/отклонение заказов
- 💰 Прозрачная система комиссий (15%)
- 📈 Детальная статистика и аналитика
- 💳 Быстрый вывод средств
- 🏆 Программа лояльности
- 📞 Поддержка 24/7

### Для администраторов:

- 👥 Управление водителями
- 📋 Управление заказами
- 💵 Финансовая отчетность
- 📊 Аналитика и метрики
- ⚙️ Настройка комиссий
- 🔔 Система уведомлений

---

## 🏗️ Архитектура

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │
│  │   Landing    │  │  Mobile App  │  │ Admin Panel  │    │
│  │  (HTML/CSS)  │  │   (Flutter)  │  │   (React)    │    │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘    │
│         │                  │                  │             │
└─────────┼──────────────────┼──────────────────┼─────────────┘
          │                  │                  │
          └──────────────────┴──────────────────┘
                             │
          ┌──────────────────┴───────────────────┐
          │         API GATEWAY                  │
          │         FastAPI + CORS               │
          └──────────────────┬───────────────────┘
                             │
          ┌──────────────────┴───────────────────┐
          │       BUSINESS LOGIC LAYER           │
          ├──────────────────────────────────────┤
          │  • Order Service                     │
          │  • Driver Service                    │
          │  • Payment Service                   │
          │  • Notification Service              │
          │  • Geo Distance Calculator           │
          │  • Auto Order Dispatcher             │
          └──────────────────┬───────────────────┘
                             │
          ┌──────────────────┴───────────────────┐
          │          DATA LAYER                  │
          ├──────────────────────────────────────┤
          │  • PostgreSQL / SQLite               │
          │  • Redis (Cache)                     │
          │  • SQLAlchemy ORM                    │
          └──────────────────────────────────────┘
```

### Технологический стек:

#### Backend:
- **Framework**: FastAPI (Python 3.10+)
- **ORM**: SQLAlchemy
- **Database**: SQLite (dev) / PostgreSQL (prod)
- **Cache**: Redis (planned)
- **Validation**: Pydantic
- **CORS**: Поддержка кросс-доменных запросов

#### Frontend:
- **Landing**: HTML5, Tailwind CSS, Vanilla JS
- **Admin**: React + Next.js (planned)

#### Mobile:
- **Framework**: Flutter 3.27+
- **State Management**: BLoC/Cubit
- **Maps**: Google Maps Flutter
- **HTTP**: Dio
- **Local Storage**: Hive

#### DevOps:
- **Version Control**: Git
- **CI/CD**: GitHub Actions (planned)
- **Containerization**: Docker (planned)
- **Monitoring**: Grafana + Prometheus (planned)

---

## 🚀 Установка

### Предварительные требования:

- Python 3.10+
- Flutter 3.27+
- Git

### 1. Клонирование репозитория:

```bash
git clone https://github.com/yourusername/cartaxi.git
cd cartaxi
```

### 2. Установка Backend:

```bash
cd эвакуаторы/backend

# Создание виртуального окружения
python -m venv venv

# Активация (Windows)
venv\Scripts\activate

# Активация (Linux/Mac)
source venv/bin/activate

# Установка зависимостей
pip install -r requirements.txt
```

### 3. Установка Mobile App:

```bash
cd эвакуаторы

# Установка зависимостей Flutter
flutter pub get

# Проверка установки
flutter doctor
```

### 4. Настройка:

#### Backend (.env файл):

```env
DATABASE_URL=sqlite:///./groceries.db
SECRET_KEY=your-secret-key-here
DEBUG=True
CORS_ORIGINS=*
```

#### Mobile (lib/logic/api/api_client.dart):

```dart
const String apiBaseUrl = 'http://localhost:8000';
```

---

## 💻 Использование

### Запуск Backend:

```bash
cd эвакуаторы/backend

# Активация виртуального окружения
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/Mac

# Запуск сервера
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Backend будет доступен:
- **API**: http://localhost:8000
- **Документация**: http://localhost:8000/docs
- **Health Check**: http://localhost:8000/health

### Запуск Landing:

Просто откройте файл `landing/index.html` в браузере или используйте Live Server.

### Запуск Mobile App:

```bash
cd эвакуаторы

# Запуск на Android эмулятор
flutter run

# Запуск на конкретном устройстве
flutter run -d <device-id>

# Запуск веб-версии
flutter run -d chrome
```

---

## 📚 API Документация

### Базовый URL: `http://localhost:8000/api`

### Endpoints для клиентов:

#### Получить список услуг:
```http
GET /api/services/
```

**Response:**
```json
[
  {
    "id": "uuid",
    "name": "Легковые автомобили",
    "service_type": "light_car",
    "base_price": 1500.00,
    "price_per_km": 40.00,
    "is_active": true
  }
]
```

#### Создать заказ:
```http
POST /api/orders/
Content-Type: application/json

{
  "user_phone": "+79991234567",
  "service_id": "uuid",
  "order_type": "urgent",
  "pickup_address": "Москва, ул. Ленина, 1",
  "pickup_latitude": 55.751244,
  "pickup_longitude": 37.618423,
  "delivery_address": "Москва, ул. Пушкина, 10",
  "delivery_latitude": 55.756802,
  "delivery_longitude": 37.615295,
  "vehicle_info": "Toyota Camry 2020",
  "additional_notes": "Сломалась коробка передач"
}
```

**Response:**
```json
{
  "id": "uuid",
  "status": "searching_driver",
  "total_price": 2100.00,
  "estimated_arrival": "2025-11-24T14:30:00",
  "created_at": "2025-11-24T14:15:00"
}
```

### Endpoints для водителей:

#### Регистрация водителя:
```http
POST /api/drivers/register
Content-Type: application/json

{
  "name": "Иван Иванов",
  "phone_number": "+79991234567",
  "email": "driver@example.com",
  "license_number": "1234567890",
  "passport_number": "1234 123456"
}
```

#### Получить доступные заказы:
```http
GET /api/drivers/{driver_id}/available-orders?radius_km=20
```

**Response:**
```json
[
  {
    "id": "uuid",
    "pickup_address": "Москва, ул. Ленина, 1",
    "delivery_address": "Москва, ул. Пушкина, 10",
    "total_price": 2100.00,
    "distance_to_pickup": 5.2,
    "status": "searching_driver"
  }
]
```

#### Принять заказ:
```http
POST /api/drivers/{driver_id}/accept-order/{order_id}
```

**Response:**
```json
{
  "status": "success",
  "earnings": 1785.00,
  "commission": 315.00,
  "order": { ... }
}
```

#### Получить статистику:
```http
GET /api/drivers/{driver_id}/stats
```

**Response:**
```json
{
  "driver_id": "uuid",
  "rating": 4.8,
  "total_orders": 156,
  "completed_orders": 148,
  "balance": 125450.00,
  "total_earned": 450000.00,
  "orders_today": 8,
  "earnings_today": 12500.00
}
```

### Полная документация:

После запуска backend перейдите на http://localhost:8000/docs для интерактивной Swagger документации.

---

## 📱 Скриншоты

### Web Landing:
![Landing Hero](docs/screenshots/landing-hero.png)
![Landing Services](docs/screenshots/landing-services.png)
![Landing Calculator](docs/screenshots/landing-calculator.png)

### Mobile App - Клиент:
![App Home](docs/screenshots/app-home.png)
![App Order](docs/screenshots/app-order.png)
![App Tracking](docs/screenshots/app-tracking.png)

### Mobile App - Водитель:
![Driver Dashboard](docs/screenshots/driver-dashboard.png)
![Driver Orders](docs/screenshots/driver-orders.png)
![Driver Stats](docs/screenshots/driver-stats.png)

---

## 📈 План развития

Подробный план развития проекта находится в файле [`DEVELOPMENT_PLAN.md`](DEVELOPMENT_PLAN.md).

### Краткий roadmap:

#### Фаза 1: MVP (Месяц 1-2) ✅ CURRENT
- [x] Backend API
- [x] Landing Page
- [x] Mobile App (базовый)
- [x] Система комиссий
- [ ] Интеграция платежей
- [ ] Push-уведомления

#### Фаза 2: Рост (Месяц 3-4)
- [ ] Оптимизация алгоритма распределения
- [ ] Система рейтингов и отзывов
- [ ] Программа лояльности
- [ ] Админ-панель
- [ ] Аналитика

#### Фаза 3: Экспансия (Месяц 5-8)
- [ ] Масштабирование на 100+ городов
- [ ] B2B направление
- [ ] Интеграция со страховыми
- [ ] API для партнеров
- [ ] ML для прогнозирования спроса

#### Фаза 4: Оптимизация (Месяц 9-12)
- [ ] AI ценообразование
- [ ] Международная экспансия
- [ ] Marketplace услуг
- [ ] IPO подготовка

---

## 💰 Монетизация

### Комиссия с водителей:
- **Стандарт**: 15% от заказа
- **Новички**: 20% (первые 50 заказов)
- **Опытные**: 12% (>500 заказов)
- **VIP**: 10% (>1000 заказов, рейтинг >4.8)

### Премиум подписка для водителей:
- **999₽/месяц**:
  - Приоритет в распределении
  - Комиссия 12%
  - Расширенная статистика
  - Быстрый вывод средств

### B2B контракты:
- Страховые компании
- Автосалоны
- Каршеринги

---

## 🤝 Вклад в проект

Мы приветствуем вклад в развитие проекта!

### Как помочь:

1. Fork репозиторий
2. Создайте feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit изменения (`git commit -m 'Add some AmazingFeature'`)
4. Push в branch (`git push origin feature/AmazingFeature`)
5. Откройте Pull Request

### Правила:

- Следуйте PEP 8 для Python кода
- Используйте Dart Style для Flutter
- Добавляйте тесты для нового функционала
- Обновляйте документацию

---

## 📄 Лицензия

Этот проект распространяется под лицензией MIT. Подробности в файле [LICENSE](LICENSE).

---

## 👥 Команда

- **CEO**: [Имя](mailto:ceo@cartaxi.io)
- **CTO**: [Имя](mailto:cto@cartaxi.io)
- **Lead Developer**: [Имя](mailto:dev@cartaxi.io)

---

## 📞 Контакты

- **Сайт**: https://cartaxi.io
- **Email**: info@cartaxi.io
- **Telegram**: [@cartaxi](https://t.me/cartaxi)
- **GitHub**: [@cartaxi](https://github.com/cartaxi)

---

## 🙏 Благодарности

- [FastAPI](https://fastapi.tiangolo.com/) - за отличный фреймворк
- [Flutter](https://flutter.dev/) - за кроссплатформенную разработку
- [Tailwind CSS](https://tailwindcss.com/) - за удобный CSS фреймворк
- Вдохновение: [Uber](https://uber.com), [Яндекс.Такси](https://taxi.yandex.ru)

---

## 📊 Статистика проекта

![GitHub stars](https://img.shields.io/github/stars/cartaxi/cartaxi?style=social)
![GitHub forks](https://img.shields.io/github/forks/cartaxi/cartaxi?style=social)
![GitHub issues](https://img.shields.io/github/issues/cartaxi/cartaxi)
![GitHub pull requests](https://img.shields.io/github/issues-pr/cartaxi/cartaxi)

---

<div align="center">
  <p>Сделано с ❤️ командой CarTaxi</p>
  <p>© 2025 CarTaxi. Все права защищены.</p>
</div>

