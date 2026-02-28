# Troubleshooting

## Бот не отвечает
```bash
sudo systemctl status vpnbot-staging --no-pager -l
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat
```

Частые причины:
- упал процесс (исключение на старте)
- неверный `BOT_TOKEN`
- нет доступа к базе (`DATABASE_URL`)
- упал subwrap/3x-ui и бот падает на healthcheck/старте

---

## Alembic / миграции
```bash
cd /opt/vpnbot/current-staging
./.venv/bin/alembic current
./.venv/bin/alembic history
./.venv/bin/alembic upgrade head
```

Если миграции “разъехались”:
- проверь, что ты в правильной папке (`current-staging`)
- проверь `DATABASE_URL` в env

---

## Webhook не приходит
Быстрая проверка “роутинг жив”:
```bash
curl -i -X POST https://pay.<domain>/yookassa/wrong \
  -H "Content-Type: application/json" \
  -d '{}'
# ожидаемо: 404
```

Если не 404:
- неправильный роутинг/туннель
- не тот домен/ingress
- не то приложение слушает

---

## Проверка порта (webhook)
```bash
ss -lntp | grep 8091
```

---

## subwrap не отвечает
```bash
curl -i http://127.0.0.1:8090/
```

---

## 3x-ui недоступен
Проверь доступность `XUI_BASE` с сервера (зависит от схемы сети).
Минимум — смотри ошибки в логах бота:
```bash
sudo journalctl -u vpnbot-staging -n 300 --no-pager -o cat
```
