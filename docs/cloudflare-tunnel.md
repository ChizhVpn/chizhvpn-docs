# Cloudflare Tunnel (схема)

Цель: дать публичные URL к локальным сервисам без прямого открытия портов.

Типичная раскладка для ChizhVPN:
- `sub.<domain>` → subwrap (например локально `127.0.0.1:8090`)
- `pay.<domain>` → webhook YooKassa (локально `127.0.0.1:8091`)
- `pay.<domain>/h/*` → Happ redirect (локально `127.0.0.1:18080`) или отдельный домен

## Проверка cloudflared
```bash
sudo systemctl status cloudflared --no-pager -l
sudo journalctl -u cloudflared -n 200 --no-pager -o cat
```

## Проверка маршрутизации (локально)
```bash
ss -lntp | egrep "8090|8091|18080" || true
curl -i http://127.0.0.1:8090/ || true
```

Важно: конфиг tunnel (`/etc/cloudflared/config.yml`) часто содержит токены.
В public репозиторий его не кладём. Максимум — пример `config.example.yml` без токенов.
