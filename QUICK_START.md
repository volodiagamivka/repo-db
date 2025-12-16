# Швидкий старт - Як запустити додаток

## Локальний запуск (для розробки)

### 1. Встановіть залежності:

```bash
# Створіть віртуальне середовище
python3 -m venv venv

# Активуйте його
source venv/bin/activate  # На macOS/Linux
# або
venv\Scripts\activate  # На Windows

# Встановіть залежності
pip install -r requirements.txt
```

### 2. Налаштуйте змінні середовища:

Створіть файл `.env` в корені проєкту:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=hospitalss
DB_PORT=3306
DB_SSL_DISABLED=true

FLASK_HOST=0.0.0.0
FLASK_PORT=5000
FLASK_DEBUG=true

SECRET_KEY=your-secret-key-here
```

### 3. Запустіть додаток:

```bash
# З віртуальним середовищем
source venv/bin/activate
python app.py

# Або через gunicorn (production-like)
gunicorn --bind 0.0.0.0:5000 --workers 4 app:app
```

### 4. Відкрийте в браузері:

- **API**: http://localhost:5000/api/v1/
- **Swagger**: http://localhost:5000/swagger/

---

## Запуск на Azure VM (через deploy_azure.sh)

### 1. Підключіться до VM:

```bash
ssh -i ~/.ssh/id_rsa azureuser@YOUR_VM_IP
```

### 2. Скопіюйте файли на VM:

```bash
# З локальної машини
scp -r . azureuser@YOUR_VM_IP:~/repo-db
```

### 3. Запустіть скрипт деплою:

```bash
# На VM
cd ~/repo-db
chmod +x deploy_azure.sh
./deploy_azure.sh
```

### 4. Перевірте:

- **API**: http://YOUR_VM_IP:5000/api/v1/
- **Swagger**: http://YOUR_VM_IP:5000/swagger/

---

## Автоматичний деплой через GitHub Actions

### 1. Налаштуйте GitHub Secrets:

GitHub → Settings → Secrets → Actions → New repository secret

Додайте:
- `AZURE_HOST` - IP адреса VM
- `AZURE_USERNAME` - ім'я користувача (azureuser)
- `AZURE_SSH_KEY` - приватний SSH ключ

### 2. Зробіть commit і push:

```bash
git add .
git commit -m "Update deployment configuration"
git push origin main
```

### 3. GitHub Actions автоматично:

- Оновить код на VM
- Оновить залежності
- Перезапустить сервіс

### 4. Перевірте статус:

GitHub → Actions → Перегляньте логи виконання

---

## Керування сервісом на VM

```bash
# Статус
sudo systemctl status hospital-api

# Перезапуск
sudo systemctl restart hospital-api

# Зупинка
sudo systemctl stop hospital-api

# Старт
sudo systemctl start hospital-api

# Логи
sudo journalctl -u hospital-api -f
```

---

## Troubleshooting

### Помилка: "ModuleNotFoundError"
```bash
source venv/bin/activate
pip install -r requirements.txt
```

### Помилка: "Can't connect to MySQL"
- Перевірте `.env` файл
- Перевірте, чи MySQL запущений
- Перевірте права доступу

### Помилка: "Port 5000 already in use"
```bash
# Знайдіть процес
lsof -i :5000

# Зупиніть його
kill -9 PID
```

### Помилка: "Permission denied"
```bash
# Надайте права
chmod +x deploy_azure.sh
```

---

## Корисні команди

```bash
# Перевірка версії Python
python3 --version

# Перевірка встановлених пакетів
pip list

# Тест API
curl http://localhost:5000/api/v1/patients/

# Перевірка Swagger
curl http://localhost:5000/swagger/
```

