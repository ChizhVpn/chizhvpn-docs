# Деплой

## Ветки
- `main` — prod
- `staging` — тест
- `feat/*` — разработка
- (опционально) `dev` — если вы так решили внутри команды

## Идея деплоя на сервере
На сервере хранится:
- репозиторий: `/opt/vpnbot/repo`
- релизы: `/opt/vpnbot/releases/<env>/<timestamp-sha>`
- активная версия: `/opt/vpnbot/current-<env>` (symlink)
- env: `/opt/vpnbot/shared/<env>.env`

`deploy.sh` обычно делает:
1) подтягивает свежий код в `/opt/vpnbot/repo`
2) собирает новый релиз в `/opt/vpnbot/releases/<env>/...`
3) создаёт/обновляет venv и ставит зависимости
4) применяет Alembic миграции
5) переключает symlink `current-<env>` на новый релиз
6) рестартит systemd сервис

## Деплой staging
```bash
sudo -u vpnbot /opt/vpnbot/deploy/deploy.sh staging
```

## Promote staging -> prod
```bash
sudo -u vpnbot /opt/vpnbot/deploy/promote.sh
```

## Проверка состояния
```bash
sudo systemctl status vpnbot-staging --no-pager -l
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat
```

## Какая версия активна
```bash
readlink -f /opt/vpnbot/current-staging
readlink -f /opt/vpnbot/current-prod
```

## Rollback (ручной, если всё плохо)
1) Найди прошлый релиз:
```bash
ls -la /opt/vpnbot/releases/staging | tail -n 20
```
2) Переключи symlink на прошлую папку:
```bash
sudo ln -sfn /opt/vpnbot/releases/staging/<PAST_RELEASE> /opt/vpnbot/current-staging
sudo systemctl restart vpnbot-staging
```
3) Проверь логи.

Важно: если ты уже применил миграции “вперёд”, rollback к старому коду может не работать.
В идеале миграции должны быть backward-compatible или иметь downgrade-план.
