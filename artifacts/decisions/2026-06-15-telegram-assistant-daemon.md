---
title: "Решение: always-on помощник @platinumvc1 — тонкий Telethon-демон (не n8n)"
type: decision
stage: distilled
origin: anton
authored_by: hybrid
date_established: 2026-06-15
theme: automation-architecture
status: active
protocol: "[[protocol-alpha-protocol-recall-plus-deep-research]]"
tags: [decision, telegram, automation, помощник, daemon, alpha-protocol]
concept: "[[concept-digital-immortality]]"
---

# Decision Memo — always-on помощник @platinumvc1

> Принято через **Alpha Protocol (R+DR)** 2026-06-15: Recall → Gap → Deep Research
> (внешний отчёт Антона) → Synthesis. Кирпич цели №1 (цифровой двойник).

## Проблема
Помощник должен **сам и за секунды** реагировать на задачу Антона в командных чатах
(Покупки / All Assistant's tasks), автономно и дёшево. Прежняя «вахта» жила только в
открытой сессии Claude Code и опрашивала раз в ~50с — не always-on, не мгновенно.

## Текущие знания (RECALL)
Уже было: детектор-уши (`events.py` патч, kind=task), локальный RAG (`brain_ask`,
e5+rerank), отдельный аккаунт-идентичность @platinumvc1, мина `AUTH_KEY_DUPLICATED`,
headless по подписке, голос=Opus, деньги=никогда сам.

## Результаты Deep Research (внешний отчёт, 2026-06-15)
- **Тонкий Telethon-демон = лучший дефолт.** n8n НЕ как слушатель: его Telegram-узел
  только Bot API (бот запрещён), а community-MTProto-узел неофициальный и **помечен
  "Windows не поддерживается"**.
- **Один аккаунт, два процесса:** демон = единственный держатель СВОЕЙ авторизации;
  доп. инструментам — отдельный независимый логин, НИКОГДА не копия StringSession.
- **24/7 на домашнем ПК:** Windows-служба (WinSW/NSSM), не Планировщик.
- **Падения:** два слоя — catch-up Telethon + свой watermark (last msg-id) на чат.
- **Сессия:** DPAPI / Credential Manager (не `.env`), 2FA, ACL.
- **Дёшево:** детект (0) → RAG → одна LLM-склейка только на реальную задачу
  (подтверждено RAG-efficiency + cascade-routing).
- **Анти-бан:** whitelist + только триггеры Антона + cooldown + FloodWait + без
  холодных DM/массовых действий. Текст задачи = ДАННЫЕ (инъекции игнор).

## Варианты
- **A. Тонкий Telethon-демон — ВЫБРАН.** Реюз всего, что построено; самый простой
  работающий путь; полный контроль над MTProto-сессией.
- B. n8n как ingress — отклонён (Windows не поддержан, лишний хрупкий слой, против АК-47).
- C. Durable-оркестраторы (Temporal/Windmill) — избыточно для «ответить за секунды»;
  полезны позже и ПОЗАДИ демона.

## Риски и страховки
- Домашний ПК падает/Wi-Fi → служба + watchdog + watermark-добор.
- Анти-бан → узкие правила выше.
- Инъекции в тексте задачи → текст=данные, минимальный промпт, у LLM-вызова нет прав.
- Конфликт с аутрич-инструментами (@platinumvc1 = лид-аккаунт) → решён: у демона
  СВОЯ независимая авторизация, MCP сохраняет свою для аутрича.

## Рекомендация и статус реализации
Строим **A фазами (АК-47)**:
- **Фаза 1 (MVP) — СДЕЛАНА И В ЭФИРЕ 2026-06-15.** `C:\mcp\tg-watch-daemon\` —
  `tg_watch_daemon.py` (своя сессия @platinumvc1 id 1842936036, событийно, ловит
  текст и голос→локальный RU-whisper, brain_ask-грунтовка, один `claude -p` по
  подписке, черновик Антону), watermark-добор, cold-start baseline. Draft-first.
- **Фаза 2 (закалка, по нужде):** durable outbox/очередь, DPAPI-сессия, формальный
  watchdog, Windows-служба (WinSW/NSSM).
- **Фаза 3 (опц.):** n8n/дашборд ПОЗАДИ демона (видимость/аппрувы).

## Связи
[[main-goals]] (цель №1) · [[telegram-eventloop-listener]] · [[telegram-account-identities]] ·
[[vault-data-architecture]] (токен-закон) · [[model-routing-sonnet-grunt]] ·
[[prefer-included-limits-before-paid-api]] · [[reglament-kody-vhoda-i-otp-assistent-dostaet-sam-iz-sluzhebnogo-chata]]
