# Разработка

## Полезные команды
### Запуск
```bash
python main.py
```

### Миграции Alembic
```bash
alembic current
alembic history
alembic revision -m "описание" --autogenerate
alembic upgrade head
```

## Как не сломать себе жизнь
- `.env*` и любые локальные env-файлы — только локально. В git их не должно быть.
- если случайно засветил `BOT_TOKEN`/ключи — делай ротацию сразу (BotFather `/revoke`, ЮKassa — смена ключей).

## Как добавить новую таблицу/поле
1) Правишь модели в `app/db/models.py`
2) Делаешь миграцию:
   ```bash
   alembic revision -m "add payments table" --autogenerate
   ```
3) Проверяешь миграцию глазами (Alembic иногда делает странности)
4) Применяешь:
   ```bash
   alembic upgrade head
   ```
