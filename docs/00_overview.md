# Overview

## Purpose
ChizhVPN provides VPN subscriptions to users via a Telegram bot.

## Core responsibilities (high-level)
- Onboarding / basic bot UI flows
- Tariff selection
- Payment initiation (provider integration exists internally)
- Subscription activation & expiry tracking
- Support/admin operations (internally)

## Environments (concept)
- dev: local developer testing (each developer uses their own test bot + local DB)
- staging: shared pre-production testing
- production: live bot

> This public repo intentionally does not describe production infrastructure, secrets, or deployment steps.
