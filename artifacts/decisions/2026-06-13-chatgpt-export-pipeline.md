---
title: Decision — how we pull ChatGPT data & artifacts into the vault
type: decision
stage: distilled
created: 2026-06-13
status: building
source: two paid research reports (deep-research + compass), both 2026-06-13
---

# Решение: регулярный экспорт ChatGPT в волт

Антон захотел то же, что уже есть для Claude (чаты Claude лежат на диске →
ночная задача читает их даром), но для **ChatGPT** (web + desktop).

## Ключевое отличие от Claude (почему «совсем без рук» нельзя)
Чаты ChatGPT **не лежат на диске**. Desktop-приложение Windows — это web-view;
локальный кэш стирается при выходе из аккаунта. Значит за историей надо ходить
**по сети с токеном**, а у веб-сессии **нет refresh-токена** → полностью
автономный cron невозможен. Токен периодически обновляем руками.
Два независимых исследования сошлись на этом → доверие высокое.

## Что решили ДЕЛАТЬ (3 слоя, как у Claude)
1. **Рабочая лошадка:** `@qwanderer/chatgpt-exporter` (форк brianjlacy/export-chatgpt,
   MIT, 1 зависимость, аудит пройден 2026-06-13). Тянет диалоги + Canvas +
   DALL-E картинки + загруженные файлы через `/backend-api`. Режим `--token`
   (cookie сессии, живёт неделями) → меньше ручной возни.
2. **Страховка (ToS-чистая):** официальный ZIP-export + `parse_official_export.py`.
   Только тексты, нулевой риск. Не ежедневный (идёт до 7 дней).
3. **Руками:** чек-лист слепых зон (Memory, Custom GPT, Instructions, voice,
   Code Interpreter outputs) — см. [[_ChatGPT-Manual-Capture-Checklist]].

## Что решили НЕ делать (и почему)
- **Официальный API для истории** — его НЕТ для consumer-аккаунта (только
  Platform API для своих API-объектов). Отвергнуто как нерабочее.
- **Парсинг кэша desktop-app** — это forensic-артефакт, не источник правды.
- **Zapier/Make «новый чат → Drive»** — такого триггера не существует.
- **Headless-браузер с обходом Cloudflare** — самый хрупкий + явный запрет ToS.

## Риск / ToS
Self-export своей истории с троттлингом ≥60с — серая зона ToS, известных банов
нет. Реальные сбои — 403 (Cloudflare) и 429 (rate-limit), не баны. Не шарить
токен, не лезть в обход защит.

## СЕЙЧАС / следующий шаг
Обвязка собрана в `E:\Obsidian\_imports\chatgpt\`. Блокеры до первого экспорта:
1. Антон кладёт токен в `secrets\` (инструкция в файле там).
2. Антон авторизует разовый `npm install` инструмента (ставит только `commander`).
3. Тестовый прогон `-Test` (1 чат) → смотрим, что реально пришло →
   потом пишем `chatgpt_export_to_vault.py` (раскладка в ноты + MOC).

Исходные отчёты сохранены: `E:\Obsidian\_imports\chatgpt\research\`.
Связано: [[claude-ai-export-to-vault]], [[second-brain-northstar]].
