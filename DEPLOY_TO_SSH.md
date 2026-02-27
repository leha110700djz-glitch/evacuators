# 🚀 Деплой на SSH Сервер (alexei@158.255.6.22)

## 📋 Содержание
1. [Подготовка сервера](#подготовка-сервера)
2. [Деплой Backend (FastAPI)](#деплой-backend)
3. [Деплой Landing Page](#деплой-landing)
4. [Настройка Nginx](#настройка-nginx)
5. [SSL/HTTPS (Let's Encrypt)](#ssl-сертификат)
6. [Автоматический деплой](#автоматический-деплой)

---

## 🔧 Подготовка сервера

### Шаг 1: Подключение к серверу

```bash
ssh alexei@158.255.6.22
```

### Шаг 2: Установка необходимого ПО

```bash
# Обновляем систему
sudo apt update && sudo apt upgrade -y

# Устанавливаем Python 3.11+
sudo apt install python3.11 python3.11-venv python3-pip -y

# Устанавливаем Nginx
sudo apt install nginx -y

# Устанавливаем Git
sudo apt install git -y

# Устанавливаем Supervisor (для автозапуска)
sudo apt install supervisor -y
```

### Шаг 3: Создаем структуру папок

```bash
# Создаем директорию для проекта
sudo mkdir -p /var/www/cartaxi
sudo chown -R alexei:alexei /var/www/cartaxi
cd /var/www/cartaxi
```

---

## 🐍 Деплой Backend (FastAPI)

### Шаг 1: Загрузка кода

```bash
# Клонируем репозиторий (или копируем файлы через SCP)
cd /var/www/cartaxi

# Вариант 1: Клонирование из Git
git clone https://github.com/YOUR_USERNAME/cartaxi-backend.git backend

# Вариант 2: Копирование с локального компьютера
# На вашем Windows:
# scp -r C:\Users\User\Desktop\эвакуаторы\эвакуаторы\backend alexei@158.255.6.22:/var/www/cartaxi/
```

### Шаг 2: Настройка виртуального окружения

```bash
cd /var/www/cartaxi/backend

# Создаем виртуальное окружение
python3.11 -m venv venv

# Активируем
source venv/bin/activate

# Устанавливаем зависимости
pip install --upgrade pip
pip install -r requirements.txt
```

### Шаг 3: Настройка environment variables

```bash
# Создаем .env файл
nano /var/www/cartaxi/backend/.env
```

**Содержимое `.env`:**

```env
DATABASE_URL=sqlite:///./cartaxi.db
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=False

SECRET_KEY=your-super-secret-key-change-this-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

CORS_ORIGINS=http://158.255.6.22,https://cartaxi.io,http://cartaxi.io

# ЮKassa
YOOKASSA_SHOP_ID=your_shop_id
YOOKASSA_SECRET_KEY=your_secret_key

# Google Maps
GOOGLE_MAPS_API_KEY=your_google_maps_key
```

### Шаг 4: Инициализация базы данных

```bash
cd /var/www/cartaxi/backend

# Активируем venv
source venv/bin/activate

# Запускаем seed скрипт
python run_seed.py
```

### Шаг 5: Настройка Supervisor (автозапуск)

```bash
sudo nano /etc/supervisor/conf.d/cartaxi-backend.conf
```

**Содержимое:**

```ini
[program:cartaxi-backend]
directory=/var/www/cartaxi/backend
command=/var/www/cartaxi/backend/venv/bin/uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
user=alexei
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
stderr_logfile=/var/log/cartaxi/backend.err.log
stdout_logfile=/var/log/cartaxi/backend.out.log
environment=PYTHONUNBUFFERED=1

[group:cartaxi]
programs=cartaxi-backend
```

**Создаем папку для логов:**

```bash
sudo mkdir -p /var/log/cartaxi
sudo chown alexei:alexei /var/log/cartaxi
```

**Запускаем:**

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start cartaxi-backend
sudo supervisorctl status
```

**Проверка:**

```bash
curl http://localhost:8000/health
# Ответ: {"status": "healthy"}
```

---

## 🌐 Деплой Landing Page

### Шаг 1: Копирование файлов

```bash
# На вашем Windows (PowerShell):
scp -r C:\Users\User\Desktop\эвакуаторы\landing\* alexei@158.255.6.22:/var/www/cartaxi/landing/

# Или на сервере:
mkdir -p /var/www/cartaxi/landing
# Затем копируете файлы вручную
```

### Шаг 2: Настройка прав доступа

```bash
sudo chown -R www-data:www-data /var/www/cartaxi/landing
sudo chmod -R 755 /var/www/cartaxi/landing
```

### Шаг 3: Обновление API_BASE_URL в index.html

```bash
nano /var/www/cartaxi/landing/index.html
```

**Замените:**

```javascript
// Было:
fetch('http://localhost:8000/api/orders/', {

// Стало:
fetch('http://158.255.6.22/api/orders/', {
// или
fetch('https://api.cartaxi.io/api/orders/', {
```

---

## ⚙️ Настройка Nginx

### Шаг 1: Создаем конфигурацию

```bash
sudo nano /etc/nginx/sites-available/cartaxi
```

**Содержимое:**

```nginx
# Backend API
server {
    listen 80;
    server_name api.cartaxi.io 158.255.6.22;

    # Логи
    access_log /var/log/nginx/cartaxi-api-access.log;
    error_log /var/log/nginx/cartaxi-api-error.log;

    # Ограничение размера загружаемых файлов
    client_max_body_size 10M;

    # API endpoints
    location /api/ {
        proxy_pass http://127.0.0.1:8000/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # CORS
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
        
        if ($request_method = 'OPTIONS') {
            return 204;
        }
    }

    # Swagger docs
    location /docs {
        proxy_pass http://127.0.0.1:8000/docs;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /openapi.json {
        proxy_pass http://127.0.0.1:8000/openapi.json;
        proxy_set_header Host $host;
    }

    # Health check
    location /health {
        proxy_pass http://127.0.0.1:8000/health;
    }
}

# Landing Page
server {
    listen 80;
    server_name cartaxi.io www.cartaxi.io 158.255.6.22;

    root /var/www/cartaxi/landing;
    index index.html;

    # Логи
    access_log /var/log/nginx/cartaxi-landing-access.log;
    error_log /var/log/nginx/cartaxi-landing-error.log;

    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1000;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss application/json;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Кэширование статических файлов
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    # Запрет на доступ к скрытым файлам
    location ~ /\. {
        deny all;
    }
}
```

### Шаг 2: Активируем конфигурацию

```bash
# Создаем симлинк
sudo ln -s /etc/nginx/sites-available/cartaxi /etc/nginx/sites-enabled/

# Удаляем дефолтную конфигурацию
sudo rm /etc/nginx/sites-enabled/default

# Проверяем конфигурацию
sudo nginx -t

# Перезагружаем Nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```

### Шаг 3: Настраиваем файрвол

```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```

---

## 🔒 SSL Сертификат (Let's Encrypt)

### Установка Certbot

```bash
sudo apt install certbot python3-certbot-nginx -y
```

### Получение сертификата

```bash
# Для API
sudo certbot --nginx -d api.cartaxi.io

# Для Landing
sudo certbot --nginx -d cartaxi.io -d www.cartaxi.io
```

### Автопродление

```bash
# Certbot автоматически добавляет cron job
# Проверка автопродления:
sudo certbot renew --dry-run
```

---

## 🤖 Автоматический деплой

### Создаем скрипт деплоя

```bash
nano /var/www/cartaxi/deploy.sh
```

**Содержимое:**

```bash
#!/bin/bash

echo "🚀 Начинаем деплой CarTaxi..."

# Переходим в директорию проекта
cd /var/www/cartaxi/backend

# Останавливаем backend
sudo supervisorctl stop cartaxi-backend

# Обновляем код из Git
echo "📥 Обновляем код..."
git pull origin main

# Активируем виртуальное окружение
source venv/bin/activate

# Обновляем зависимости
echo "📦 Обновляем зависимости..."
pip install -r requirements.txt --upgrade

# Применяем миграции (если есть)
# alembic upgrade head

# Запускаем backend
echo "✅ Запускаем backend..."
sudo supervisorctl start cartaxi-backend

# Проверяем статус
echo "🔍 Проверяем статус..."
sudo supervisorctl status cartaxi-backend

# Перезагружаем Nginx
echo "🔄 Перезагружаем Nginx..."
sudo systemctl reload nginx

echo "✨ Деплой завершен!"
echo "🌐 API: http://158.255.6.22/docs"
echo "🌐 Landing: http://158.255.6.22"
```

**Делаем исполняемым:**

```bash
chmod +x /var/www/cartaxi/deploy.sh
```

**Использование:**

```bash
/var/www/cartaxi/deploy.sh
```

---

## 📊 Мониторинг и логи

### Просмотр логов Backend

```bash
# Логи Supervisor
sudo tail -f /var/log/cartaxi/backend.out.log
sudo tail -f /var/log/cartaxi/backend.err.log

# Системные логи
sudo journalctl -u supervisor -f
```

### Просмотр логов Nginx

```bash
# Access logs
sudo tail -f /var/log/nginx/cartaxi-landing-access.log
sudo tail -f /var/log/nginx/cartaxi-api-access.log

# Error logs
sudo tail -f /var/log/nginx/cartaxi-landing-error.log
sudo tail -f /var/log/nginx/cartaxi-api-error.log
```

### Проверка статуса сервисов

```bash
# Supervisor
sudo supervisorctl status

# Nginx
sudo systemctl status nginx

# Система
htop
df -h
free -h
```

---

## 🎯 Быстрый старт (TL;DR)

```bash
# 1. Подключаемся к серверу
ssh alexei@158.255.6.22

# 2. Копируем проект
scp -r C:\Users\User\Desktop\эвакуаторы\эвакуаторы\backend alexei@158.255.6.22:/var/www/cartaxi/
scp -r C:\Users\User\Desktop\эвакуаторы\landing alexei@158.255.6.22:/var/www/cartaxi/

# 3. На сервере устанавливаем зависимости
cd /var/www/cartaxi/backend
python3.11 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python run_seed.py

# 4. Настраиваем Supervisor
sudo cp /path/to/supervisor/config /etc/supervisor/conf.d/cartaxi-backend.conf
sudo supervisorctl reread && sudo supervisorctl update

# 5. Настраиваем Nginx
sudo cp /path/to/nginx/config /etc/nginx/sites-available/cartaxi
sudo ln -s /etc/nginx/sites-available/cartaxi /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl restart nginx

# 6. Готово!
```

---

## ✅ Проверка работы

После деплоя проверьте:

1. **Backend API:**
   ```
   http://158.255.6.22/docs
   http://158.255.6.22/health
   ```

2. **Landing Page:**
   ```
   http://158.255.6.22
   ```

3. **Тест создания заказа:**
   - Откройте landing
   - Заполните форму
   - Проверьте в Swagger (/docs) что заказ создался

---

## 🆘 Troubleshooting

### Backend не запускается

```bash
# Проверяем логи
sudo tail -f /var/log/cartaxi/backend.err.log

# Проверяем порт
sudo netstat -tulpn | grep 8000

# Перезапускаем
sudo supervisorctl restart cartaxi-backend
```

### Nginx 502 Bad Gateway

```bash
# Проверяем что backend запущен
curl http://localhost:8000/health

# Проверяем конфигурацию Nginx
sudo nginx -t

# Проверяем логи
sudo tail -f /var/log/nginx/cartaxi-api-error.log
```

### База данных не создается

```bash
cd /var/www/cartaxi/backend
source venv/bin/activate
python run_seed.py
```

---

## 📞 Поддержка

Если возникли проблемы:
1. Проверьте логи
2. Убедитесь что все сервисы запущены
3. Проверьте конфигурацию Nginx и Supervisor

Удачного деплоя! 🚀



