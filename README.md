# ChizhVPN — Telegram бот VPN-сервиса

Бот на **Python 3.10 + aiogram v3**. Запускается на **Ubuntu** через **systemd**.

Что делает:
- управляет клиентами в **3x-ui (Xray)** через HTTP API
- выдаёт subscription через **subwrap** (ссылки вида `/u/<sub_id>`)
- принимает оплату через **ЮKassa** и после успешной оплаты создаёт/продлевает ключ
- отдаёт пользователю ссылку `.../h/<sub_id>` (страница с deep-link `happ://add/...` для приложения Happ)
- хранит состояние в **PostgreSQL**, миграции — **Alembic**

## Документация
- Оглавление: `docs/README.md`
- Быстрый старт (локально): `docs/quickstart.md`
- Конфиг/ENV: `docs/config.md`
- Архитектура: `docs/architecture.md`
- База данных: `docs/database.md`
- Платежи YooKassa: `docs/payments-yookassa.md`
- Деплой + rollback: `docs/deploy.md`
- Runbook: `docs/runbook.md`
- Troubleshooting: `docs/troubleshooting.md`
- Cloudflare Tunnel: `docs/cloudflare-tunnel.md`

## Серверная структура (как сейчас устроено)
- репозиторий (git): `/opt/vpnbot/repo`
- релизы:
  - staging: `/opt/vpnbot/releases/staging/<timestamp-sha>`
  - prod: `/opt/vpnbot/releases/prod/<timestamp-sha>`
- активные версии (symlink):
  - `/opt/vpnbot/current-staging`
  - `/opt/vpnbot/current-prod`
- env файлы (секреты, не в git):
  - `/opt/vpnbot/shared/staging.env`
  - `/opt/vpnbot/shared/prod.env`

## systemd
- staging: `vpnbot-staging.service`
- prod: `vpnbot.service`

## Быстрые команды на сервере
```bash
# деплой staging
sudo -u vpnbot /opt/vpnbot/deploy/deploy.sh staging

# promote staging -> prod
sudo -u vpnbot /opt/vpnbot/deploy/promote.sh

# статус и логи
sudo systemctl status vpnbot-staging --no-pager -l
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat

# какая версия активна
readlink -f /opt/vpnbot/current-staging
readlink -f /opt/vpnbot/current-prod
```
