# Переменные окружения

Хранятся на сервере:
- `/opt/vpnbot/shared/staging.env`
- `/opt/vpnbot/shared/prod.env`

## Общие
- `BOT_TOKEN` — токен бота Telegram
- `ADMINS` — список telegram_id админов (через запятую)
- `DATABASE_URL` — строка подключения PostgreSQL
- `XUI_BASE` — base URL панели 3x-ui
- `XUI_USERNAME`, `XUI_PASSWORD`
- `XUI_INBOUND_ID` — inbound_id, с которым работает бот

## subwrap
- `SUBWRAP_BASE` / `SUBWRAP_ROOT` / `SUBWRAP_PREFIX` — настройки ссылок подписки
- `SUBWRAP_HEALTH_URL` — проверка health

## YooKassa
- `YOOKASSA_SHOP_ID`
- `YOOKASSA_SECRET_KEY`
- `PAY_RETURN_URL` — куда вернётся пользователь после оплаты
- `YOOKASSA_WEBHOOK_TOKEN` — секрет для проверки webhook (см. docs/payments-yookassa.md)

### Тест оплаты
- `PAY_TEST_AMOUNT_RUB=1` — форсит сумму оплаты (для staging)
## Важно про безопасность

- Файлы `/opt/vpnbot/shared/*.env` **никогда не коммитятся в git**
- Секреты не хранятся в README или docs
- Для новых разработчиков создаётся `.env.example` без значений

Рекомендуется:
- Ограничить права на env-файлы:
  chmod 600 /opt/vpnbot/shared/*.env
- Владельцем должен быть пользователь `vpnbot`
