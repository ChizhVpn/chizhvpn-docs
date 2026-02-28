# Runbook (операторские действия)

Цель: быстро понять “жив/не жив” и починить без гаданий.

## 0) Мини-чеклист
```bash
# сервис
sudo systemctl status vpnbot-staging --no-pager -l

# последние логи
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat

# порты (subwrap / webhook / happ)
ss -lntp | egrep "8090|8091|18080" || true

# какая версия активна
readlink -f /opt/vpnbot/current-staging
```

## 1) Бот “не отвечает” в Telegram
Что проверить:
1) `systemctl status vpnbot-staging`
2) логи `journalctl -u ...`
3) токен бота: не отозван ли (если отозван — обновить в `staging.env`)
4) сеть/доступ к Telegram (редко, но бывает)

Команды:
```bash
sudo systemctl restart vpnbot-staging
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat
```

## 2) Платежи не обрабатываются
Проверить:
- доступен ли публичный URL webhook
- слушает ли локальный порт `8091`
- есть ли ошибки в логах при обработке webhook
- успешна ли проверка платежа через API YooKassa

Команды:
```bash
ss -lntp | grep 8091 || true
sudo journalctl -u vpnbot-staging -n 300 --no-pager -o cat | tail -n 200
```

## 3) Не создаются/не продлеваются ключи (3x-ui)
Проверить:
- `XUI_BASE`, логин/пароль
- правильный `XUI_INBOUND_ID`
- доступен ли 3x-ui по сети с сервера

Команды:
```bash
# минимум: посмотри ошибки в логах
sudo journalctl -u vpnbot-staging -n 300 --no-pager -o cat
```

## 4) Не открывается Happ/не импортируется подписка
Проверить:
- доступен ли `SUBWRAP_BASE` снаружи
- правильный `SUBWRAP_ROOT`
- работает ли `/h/<subid>` (страница)

Команды:
```bash
ss -lntp | grep 18080 || true
```

## 5) Экстренный откат
Смотри `docs/deploy.md` → Rollback.
