# Быстрый старт (локально)

Ниже — минимальный сценарий, чтобы разработчик запустил проект без сервера.

## Требования
- Python **3.10**
- PostgreSQL (локально или в Docker)
- Git

## 1) Клонирование
```bash
git clone <REPO_URL>
cd chizhvpn
```

## 2) Виртуальное окружение + зависимости
### Windows (PowerShell)
```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\python -m pip install -U pip
.\.venv\Scripts\pip install -r requirements.txt
```

### Linux/macOS
```bash
python3.10 -m venv .venv
./.venv/bin/python -m pip install -U pip
./.venv/bin/pip install -r requirements.txt
```

## 3) ENV
Создай файл `.env` (он не должен коммититься). Можно начать с `.env.example`.

Минимальный набор, без которого проект не поднимется:
- `BOT_TOKEN`
- `DATABASE_URL`
- `XUI_BASE`, `XUI_USERNAME`, `XUI_PASSWORD`, `XUI_INBOUND_ID`
- `SUBWRAP_BASE`, `SUBWRAP_ROOT` (и при необходимости `SUBWRAP_PREFIX`)
- для платежей: `YOOKASSA_SHOP_ID`, `YOOKASSA_SECRET_KEY`, `YOOKASSA_WEBHOOK_TOKEN`, `PAY_RETURN_URL`

Подробно: `config.md`.

## 4) Миграции
```bash
# если alembic в venv
alembic upgrade head
```

## 5) Запуск бота (polling)
Проект содержит entrypoint `main.py` (в корне) и `app/main.py`.
Обычно запускают корневой:
```bash
python main.py
```

## 6) Страница Happ (локально)
Для теста страницы подключения Happ:
```bash
python happ_redirect.py
# по умолчанию: http://127.0.0.1:18080/h/<subid>
```

## 7) Быстрая проверка
- бот отвечает в Telegram
- миграции применились (таблицы `users`, `keys` есть)
- при открытии `http://127.0.0.1:18080/h/test` появляется кнопка “Подключиться”
