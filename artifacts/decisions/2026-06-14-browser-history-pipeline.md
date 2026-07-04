---
title: "Decision Memo — что строим из истории браузера (alpha-scoring · execution-gap · interest-timeline · belief-map)"
aliases: ["Decision browser-history knowledge pipeline 2026-06-14"]
type: decision
stage: distilled
status: agreed
date: "2026-06-14"
protocol: alpha-protocol-recall-plus-deep-research
decided_by: anton
confidence: high
tags: [decision, alpha-protocol, browser-history, second-brain, alpha-scoring, execution-gap]
---

# Decision Memo — Knowledge pipeline из истории браузера

> Создано по **Alpha Protocol** (R+DR): Recall (наш Второй Мозг) → внешний Deep Research (Антон принёс отчёт `deep-research-report (7).md`) → SYNTHESIS → это решение. Консенсус Антона: «да, я полностью СОГЛАСЕН» (2026-06-14).

## Проблема
История браузера лежит в волте (`browser_history.db` + заметки + e5-индекс), но это «карта внимания». Нужно превратить её в **«где у меня альфа»** — рычаг для цели №1 (цифровой двойник / Второй Мозг), а не просто архив ссылок.

## Что мы УЖЕ знали (Recall)
Архитектура уже построена и совпадает с лучшей практикой: 3-слойка `[[vault-data-architecture]]` (SQLite факты + e5 смысл + markdown), `multilingual-e5` для RU/EN, canonical-дедуп, провенанс, секрет-редакция, «raw не удаляем» (`[[preserve-originals-rule]]`), психометрика = гипотезы не факты (`[[epistemic-neutrality]]`). См. `[[browser-history-import]]`, `[[Browser-Context-Analysis-2026-Q2]]`.

## Что дал Deep Research
Внешний отчёт **независимо валидировал ~70% нашей архитектуры** и добавил несколько идей. Также предложил большой пайплайн (краулинг 100k URL, Playwright, OCR, FAISS/Qdrant, BERTopic, SQLCipher) — но это ⚠️ over-build.

## Решение — ЧТО ДЕЛАЕМ (4 дешёвые надстройки на существующий пайплайн)
1. **Alpha-scoring** — на каждую тему: `novelty · recurrence · cross-domain · actionability · execution-gap · freshness` → ранжированная «где альфа». Дёшево: SQL + правила, 0 LLM на счёт.
2. **Execution-gap** — темы с МНОГО чтения и НОЛЬ артефактов в волте (джойн `browser_history.db` ↔ волт через grep/RAG-покрытие). Самое практичное: «исследовал, но не довёл».
3. **Interest-timeline по годам** — сдвиг интересов/инструментов во времени. **Только после многолетнего Takeout** (на 90 днях трендов нет).
4. **Belief-map** — датированные утверждения с источниками («предпочитает local-first», «ищет anti-hype») → в identity-слой цифрового двойника (`[[self-bible-identity-layer]]`).

## Что ОТВЕРГЛИ (⚠️ против [[ak47-simplicity]])
Полный пайплайн отчёта: рекравл/краулинг тела 100k страниц, Playwright-рендер, OCR, FAISS/Qdrant, SQLCipher, BERTopic, запуск его мега-промпта целиком. Причина: избыточно для цели, не чинится «отвёрткой», у нас уже работает на заголовках+e5. Статус — «отложено, если докажет нужду» (`[[declined-decisions]]`).

## Риски / оговорки
- История = «реконструкция внимания», не «личность»: alpha/belief — гипотезы с confidence, не приговор.
- Глубина сейчас 90 дней (лимит Chrome); тренды/recurrence/timeline осмысленны только после Takeout (`[[browser-history-import]]` § PENDING, ~16 июня).

## TODO (привязка к приходу Takeout ~16.06)
- [ ] **Сейчас (no-regret):** это Memo + спека в память. ✅
- [ ] **При Takeout:** влить многолетку → построить (1) alpha-scoring, (2) execution-gap, (3) interest-timeline; добавить кольца/таймлайн в дашборд.
- [ ] **Потом:** (4) belief-map → identity-слой.
- Принцип: детерминированно (SQL/grep) считаем, LLM только судит/формулирует; провенанс на каждый вывод.

## Связи
[[browser-history-import]] · [[Browser-Context-Analysis-2026-Q2]] · [[vault-data-architecture]] · [[self-bible-identity-layer]] · [[main-goals]] · [[alpha-protocol-recall-plus-dr]] · [[declined-decisions]]
