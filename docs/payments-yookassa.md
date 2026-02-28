# YooKassa: платежи + webhook

## Идея
YooKassa присылает уведомления (webhook), но доверять “на слово” нельзя.
Нормальная схема:
1) webhook прилетает → ты вытаскиваешь `payment_id`
2) сам идёшь в YooKassa API `GET /v3/payments/{id}`
3) только если там `status=succeeded` → выполняешь create/extend

## Поток (в проекте)
1) Бот создаёт платёж через `POST /v3/payments`
2) Пользователь получает `confirmation_url`
3) После оплаты YooKassa шлёт webhook на публичный endpoint
4) Бот проверяет `payment_id` через API
5) Фиксирует платёж в БД и выполняет действие
6) Отдаёт пользователю ссылку `/h/<subid>`

## Webhook endpoint
Локально (на сервере) слушаем:
- `http://127.0.0.1:8091/yookassa/<TOKEN>`

Где `<TOKEN>` = `YOOKASSA_WEBHOOK_TOKEN`.

Снаружи обычно проксируется/туннелится на:
- `https://pay.<domain>/yookassa/<TOKEN>`

## Идемпотентность (чтобы не продлевало два раза)
Обязательные правила:
- `external_id` = `payment_id` YooKassa. Должен быть **unique** в БД.
- при обработке webhook:
  1) пытаемся вставить payment (или SELECT FOR UPDATE по external_id)
  2) если `processed=true` — выходим (мы это уже делали)
  3) если статус в YooKassa `succeeded` — выполняем действие, ставим `processed=true`

## Быстрая проверка endpoint’а
```bash
curl -i -X POST "https://pay.<domain>/yookassa/wrong" \
  -H "Content-Type: application/json" \
  -d '{}'
# ожидаемо 404 (потому что токен в пути неверный)
```

## Полезные логи на сервере
```bash
sudo journalctl -u vpnbot-staging -n 200 --no-pager -o cat
```
