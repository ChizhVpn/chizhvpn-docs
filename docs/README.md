# Документация ChizhVPN

## С чего начать
1) Локальный запуск: `quickstart.md`
2) Конфиг переменных окружения: `config.md`
3) Архитектура и потоки: `architecture.md`
4) Платежи: `payments-yookassa.md`
5) Деплой/роллбек: `deploy.md`
6) Если что-то сломалось: `runbook.md` → `troubleshooting.md`

## Файлы проекта (по репозиторию)
Ключевые места в коде:
- `app/tg/*` — Telegram (aiogram): хэндлеры, клавиатуры, тексты
- `app/services/*` — бизнес-логика (ключи/статус)
- `app/adapters/xui.py` — 3x-ui API
- `app/adapters/subwrap.py` — генерация subscription-ссылок
- `app/db/*` — модели + репозитории + движок SQLAlchemy
- `happ_redirect.py` — страница `/h/{subid}` с кнопкой “Подключиться” в Happ
- `alembic/*` — миграции

## Окружения
- `staging` — тестовый бот/окружение
- `prod` — боевой бот/окружение

Отличия обычно только в:
- `BOT_TOKEN`
- доменах (`SUBWRAP_BASE`, публичные URL)
- ключах ЮKassa (test/prod)
- базе данных
