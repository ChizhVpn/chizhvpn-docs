# База данных

## Миграции
Alembic хранится в `alembic/`, текущая схема поднимается:
```bash
alembic upgrade head
```

## Таблицы

### users
Назначение: привязка Telegram пользователя к внутреннему id.

Поля (по миграции `6d034f8943f5`):
- `id` (PK)
- `telegram_id` (unique)
- `created_at`

### keys
Назначение: ключ/клиент в 3x-ui для конкретного пользователя.

Поля (по миграции `6d034f8943f5`):
- `id` (PK)
- `user_id` (FK -> users.id, on delete cascade)
- `key_id` (int) — внутренний номер ключа (логика проекта)
- `inbound_id` (int) — inbound в 3x-ui
- `client_uuid` (unique) — UUID клиента в 3x-ui
- `sub_id` (unique) — id для subscription (используется в ссылках)
- `expires_at` (tz)
- `created_at`, `updated_at`

Ограничения:
- `uq_client_uuid`
- `uq_sub_id`
- `uq_user_keyid` (user_id + key_id)

### payments (важно)
В документации проекта таблица `payments` уже описана, но в текущих tracked миграциях её нет.
Рекомендация: добавить миграцию, чтобы схема “сервера” совпадала с репозиторием.

Минимально полезные поля:
- `id` (PK)
- `user_id` (FK)
- `mode` (`create`/`extend`)
- `key_id` (nullable; если продление конкретного ключа)
- `months` (int)
- `amount_rub` (int)
- `external_id` (unique; id платежа YooKassa)
- `status` (string/enum)
- `processed` (bool; защита от повторных webhook)
- `created_at`, `paid_at`

## Индексы
- `users.telegram_id` — unique index
- `keys.user_id` — обычный индекс + уникальность `user_id,key_id`
- `payments.external_id` — unique (обязательно)
