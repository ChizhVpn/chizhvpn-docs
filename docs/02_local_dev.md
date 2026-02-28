# Local development (safe)

## Requirements
- Python 3.10
- PostgreSQL (local)
- A personal Telegram test bot token (create via BotFather)

## Local run (concept)
1) Create local env file (NOT committed):
   - BOT_TOKEN=...
   - DATABASE_URL=postgresql+asyncpg://...
2) Install deps:
   - pip install -r requirements.txt
3) Apply migrations:
   - alembic upgrade head
4) Run bot in polling mode:
   - python -m app.main

## Notes
- Each developer should have their own BotFather test bot token.
- Never commit env files or tokens.
