# 🌐 Bypass List for OpenWrt Podkop & Sing-box

> Централизованный репозиторий доменов обхода блокировок для роутеров OpenWrt с пакетом **Podkop** и ядром **Sing-box**.

---

## 📁 Структура

- **`custom_domains.txt`** — Полный список доменов обхода (8 128+ доменов):
  - Поисковые и облачные сервисы (Google, Cloudflare, AI Suite: Anthropic, OpenAI, Gemini, Cursor).
  - Криптобиржи, децентрализованные протоколы, CLOB и Quant API (`api.hyperliquid.xyz`, `kalshi.com`, `odds-api.io`, `betfair.com`, `polymarket.com` и др.).
  - Соцсети и мессенджеры (Instagram, Twitter/X, Discord, YouTube).

---

## ⚙️ Интеграция с роутером OpenWrt (`192.168.1.1`)

В `/etc/config/podkop` настроена прямая загрузка:
```uci
list remote_domain_lists 'https://raw.githubusercontent.com/yurac777/bypass-list/main/custom_domains.txt'
option remote_domain_list_type 'text'
```

### ⏰ Расписание обновления:
В cron на роутере задано ежедневное обновление:
```cron
13 9 * * * /usr/bin/podkop list_update
```
При выполнении `list_update` скрипт скачивает свежий `custom_domains.txt`, компилирует `/tmp/sing-box/rulesets/main-remote-domains-ruleset.json` и автоматически перезагружает Sing-box.

---

## 🚀 Как добавить новые домены

1. Добавить строку с доменом в `custom_domains.txt`.
2. Закоммитить и отправить в GitHub:
   ```bash
   git commit -am "feat: add domain"
   git push origin main
   ```
3. Применить на роутере немедленно (или дождаться 09:13):
   ```bash
   ssh router '/usr/bin/podkop list_update'
   ```

---

## 🛡️ Правило Zero-FakeIP для внутренних сервисов

**КАТЕГОРИЧЕСКИ ЗАПРЕЩЕНО** добавлять в этот список российские ресурсы и внутренние эндпоинты кластера:
- `daily-author.ru`, `vpn-rating.space`, `alm-quant.xyz`, `pay.alm-quant.xyz`
- `yandex.ru`, `vk.com`, `kinopoisk.ru`, `gosuslugi.ru`, банки РФ

Они должны резолвиться напрямую через Яндекс DNS (`77.88.8.8`) и идти через `direct-out` без попадания в FakeIP (`198.18.x.x`).
