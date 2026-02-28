# Architecture (high-level, safe)

## Main components
1) Telegram Bot (Python, aiogram)
2) Database (PostgreSQL) for persistent data
3) Payment provider integration (details internal)
4) Provisioning / access issuance integration (details internal)

## Typical flow
User → Bot UI → Tariff → Payment → Confirmation → Subscription activated → User gets connection instructions.

## Reliability principles
- Idempotent payment events (no double activation)
- Clear subscription state model
- Traceable logs (user_id, payment_id, request_id)
