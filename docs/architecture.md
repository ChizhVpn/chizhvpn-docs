# Архитектура

## Компоненты
### 1) Telegram Bot (aiogram v3)
- UI: команды / кнопки / callback
- бизнес-логика: создание/продление ключей, выдача ссылок
- поднимает локальные HTTP endpoints (если включено): например webhook YooKassa

Код: `app/tg/*`, `app/services/*`

### 2) PostgreSQL + SQLAlchemy + Alembic
- хранит пользователей, ключи, платежи (если таблица платежей уже добавлена)
- миграции в `alembic/*`

### 3) 3x-ui (Xray)
- бот логинится в панель и управляет client’ами в inbound’е
- операции: create / extend / delete

Код: `app/adapters/xui.py`

### 4) subwrap
- собирает subscription по `sub_id` (обертка над конфигом)
- даёт публичную ссылку, которую можно импортировать в клиент

Код: `app/adapters/subwrap.py`

### 5) Happ redirect page
Отдельная страница `/h/{subid}`, которая:
- показывает кнопку подключения
- формирует deep-link `happ://add/<subscription-url>`

Код: `happ_redirect.py` (aiohttp)

### 6) YooKassa
- бот создаёт платеж (API)
- после оплаты YooKassa шлёт уведомление на webhook
- бот проверяет платеж через API и выполняет действие (create/extend)

## Поток “создать/продлить” (схема)
```mermaid
sequenceDiagram
  participant U as Пользователь
  participant TG as Telegram Bot
  participant Y as YooKassa
  participant DB as PostgreSQL
  participant X as 3x-ui
  participant S as subwrap

  U->>TG: Купить/Продлить
  TG->>DB: создать payment (pending)
  TG->>Y: POST /payments (idempotence-key)
  Y-->>TG: confirmation_url
  U->>Y: Оплата
  Y-->>TG: webhook (event + payment_id)
  TG->>Y: GET /payments/{id}
  Y-->>TG: status=succeeded
  TG->>DB: отметить payment succeeded (idempotent)
  TG->>X: создать/продлить client (inbound)
  X-->>TG: ok (client_uuid / expire)
  TG->>DB: сохранить/обновить key
  TG->>S: сформировать subscription URL
  TG-->>U: ссылка /h/{subid} + инструкция
```

## Порты (по докам проекта)
- subwrap health: `127.0.0.1:8090`
- YooKassa webhook (локально): `127.0.0.1:8091`
- Happ redirect (локально): `127.0.0.1:18080`

Публичный доступ обычно делается через Cloudflare Tunnel / reverse proxy.
