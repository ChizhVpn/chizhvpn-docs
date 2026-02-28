# Конфигурация (ENV)

На сервере переменные лежат в:
- `/opt/vpnbot/shared/staging.env`
- `/opt/vpnbot/shared/prod.env`

Для локальной разработки — `.env` (не коммитится).

## Общие
- `BOT_TOKEN` — токен Telegram бота
- `ADMINS` — список telegram_id админов через запятую (например: `123,456`)
- `DATABASE_URL` — DSN PostgreSQL, формат:
  - `postgresql+asyncpg://USER:PASSWORD@HOST:5432/DBNAME`

## 3x-ui (Xray)
- `XUI_BASE` — base URL панели (например: `http://127.0.0.1:2053`)
- `XUI_USERNAME`, `XUI_PASSWORD` — логин/пароль панели
- `XUI_INBOUND_ID` — inbound_id, с которым работает бот (целое число)

## subwrap
- `SUBWRAP_BASE` — публичная база подписки (например: `https://sub.example.com/u`)
- `SUBWRAP_ROOT` — “корень” внутри ссылки (часть пути)
- `SUBWRAP_PREFIX` — префикс (если используется вашей конфигурацией)
- `SUBWRAP_HEALTH_URL` — health endpoint subwrap (по умолчанию часто `http://127.0.0.1:8090/`)

## YooKassa
- `YOOKASSA_SHOP_ID`
- `YOOKASSA_SECRET_KEY`
- `PAY_RETURN_URL` — куда вернётся пользователь после оплаты
- `YOOKASSA_WEBHOOK_TOKEN` — секрет в URL webhook (токен в пути)

### Тестовый режим (staging)
- `PAY_TEST_AMOUNT_RUB=1` — фиксированная сумма для тестов (если у вас это реализовано)

## Безопасность
- `*.env` не коммитятся.
- права на env-файлы на сервере:
  ```bash
  chmod 600 /opt/vpnbot/shared/*.env
  chown vpnbot:vpnbot /opt/vpnbot/shared/*.env
  ```
