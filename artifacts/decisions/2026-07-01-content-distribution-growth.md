---
title: "Decision Memo — Content distribution & audience-growth architecture (Alpha Protocol synthesis)"
aliases: ["Решение — дистрибуция и рост аудитории контент-фабрики", "content distribution growth architecture"]
date: 2026-06-20
type: decision
stage: distilled
origin: anton
authored_by: claude-code
machine: HP17-ZBook
status: proposed
theme: content-factory
concept: "[[content-factory]]"
tags: [decision, content-factory, distribution, audience-growth, alpha-protocol, deep-research]
---

# Decision Memo — Дистрибуция и рост аудитории фабрики контента

> Alpha Protocol Шаг 4 (Synthesis). Слияние RECALL ([[content-factory]] и др.) + ДВУХ внешних Deep Research (ChatGPT lite + цитируемый файл `distribution deep-research-report.md`). Оба DR сошлись → высокая уверенность. Реализация — только по выбору Антона.

## 1. Проблема
Фабрика контента ([[content-factory]]) LIVE с 2026-06-12, но покрывает только ВХОД и черновики. Не решено: **куда и как постить, чтобы расти ПРАВИЛЬНЫМИ фолловерами** (single-теми, кто строит/шипит/экспериментирует), а не vanity-лайками. Цель Антона: набирать единомышленников через честную хронику превращения не-программиста в AI-native builder.

## 2. Текущие знания (RECALL)
- Центр = волт+markdown (НЕ Airtable); HOT сегодня / COLD архив через RAG. **DR подтвердил — это правильно.**
- Draft-first, НИЧЕГО не автопостится. **DR подтвердил** (X фильтрует дубль-автоматизацию; ручной review обязателен).
- Голос-первый ([[concept-graphomania-voice-first-writing]]); хаб TG `-1002427790196` авто-транскрипт. **DR: Telegram идеален для voice-note summaries.**
- Письменный крафт уже глубоко исследован (`_CRAFT-research.md`, 26 источников). DR его НЕ трогал (так и просили).
- Автор-голос = только Opus ([[model-routing-sonnet-grunt]] карв-аут); приватность HARD ([[fb-diary-voice]]).
- Уже есть: FB-diary лана + контент-план в Saved. НЕТ: X-адаптер, публичный TG-канал+чат, Substack, Reddit-лана, метрика качества, формализованный cooldown-гейт.

## 3. Результаты DR (консенсус обоих отчётов)
**Роли площадок:**
- **X = двигатель ОТКРЫТИЯ** (builders/indie/AI там; быстрее всего; Marc Lou, Pieter Levels — почти ежедневный публичный билдинг).
- **Telegram канал+чат = удержание и КАЧЕСТВО аудитории** (недоиспользован; «100 истинных верующих > 10000 подписчиков»; фаза-2 фильтрации).
- **Substack = компаундящий архив longform** (owned email-аудитория). **Medium почти мёртв** (API архивирован, новых токенов нет → только опц. зеркало).
- **Reddit = эпизодическое high-intent открытие** (участвуй-первым; обязателен native-рерайт; хрупок к самопиару).
- **Facebook личный = тёплый граф/доверие** (только вручную; API личные профили не поддерживает).
- **Аллокация усилий ≈ 40% X · 30% Telegram · 15% Reddit · 10% Substack · 5% FB · 0% Medium-first.**

**Архитектура деривативов:** «один канон → N НАТИВНЫХ переписок» ПОБЕЖДАЕТ — но только при реальном рерайте под площадку. Copy-paste наказывается (X дубль-фильтр; Reddit спам; исследование 2025: рерайт заголовков YouTube→Reddit поднимает engagement). → НЕ «один пост везде», а «один пакет-идея → несколько нативных исполнений».

**Метрика:** не фолловеры/лайки, а **Audience Quality Score (AQS)** — profile-fit + повторное осмысленное взаимодействие + миграция в owned-каналы + peer-сигнал, минус боты/outrage. Формула в отчёте (0–24 vanity … 65+ сильно-релевантный рост). Реверивайт усилия по AQS-на-час, не по impressions.

**Уязвимый контент:** «заслуженная уязвимость, не сырое зрелище». Гейт по риску:
- 🟢 green (тех-тупики, фрустрация) — cooldown 1–3ч, self-review, постим как урок.
- 🟡 amber (эмоц. срывы, доход, конфликт) — 24ч, один проверяющий, публикуем только после извлечения урока.
- 🔴 red (legal/health/третьи лица/секреты) — 72ч+, внешний review, сырое обычно НЕ публикуем.
Перебор «боли» приучает аудиторию потреблять твою нестабильность → spectators, не peers.

**Тулинг (гибрид):** markdown-KB = канон; **Typefully** = чистый draft-first для X; Buffer/Publer = только для широкого охвата/аналитики; custom+Telegram-bot = community; Substack manual. **Hypefury НЕ в центр** (growth-automation drift = мутирует в «аккаунт, продающий рост»).

## 4. Варианты
- **A. Status quo** — оставить как есть (FB-diary + план в Saved). Минус: не растёт по DR-плану, нет X/TG-канала/метрики.
- **B. Полный стек сразу** (X+TG-канал+чат+Substack+Reddit+AQS+тулы). Минус: ⚠️ нарушает АК-47, риск не потянуть, автоматизация до данных.
- **C. (рекомендуется) Фазовый, AK-47** — сначала сузить на X+Telegram+Substack ядро, поставить AQS+cooldown-гейт, адаптеры по одному; автоматизация — последней, после данных. Совпадает и с DR-роадмапом, и с нашим branch-tracker build-path.

## 5. Риски
- Copy-paste деривативов → штрафы площадок (X/Reddit). Митигировать: адаптер ОБЯЗАН рерайтить, не резать.
- Voice drift от тулов с авто-рерайтом/авто-DM → митигировать: автор-голос только Opus, draft-first, Hypefury не в центр.
- Платформенная хрупкость (Medium API мёртв, Reddit approval-gated, FB личные профили) → не строить автоматизацию вокруг них.
- Перебор уязвимости → не те люди. Митигировать: cooldown-гейт green/amber/red.
- Оптимизация vanity → деградация ниши. Митигировать: AQS как north-star.

## 6. Рекомендации (что добавить к СУЩЕСТВУЮЩЕЙ фабрике)
1. **Адаптеры по одному** (small style files рядом с FB `_STYLE.md`): X → Telegram-канал → Reddit. Generate-стадия пишет нативно, не режет. (уже в branch-tracker backlog)
2. **Публичный Telegram-канал (дневник) + малый чат (комьюнити)** как retention-слой — сейчас контент идёт в Saved, не в публичный канал.
3. **Substack** как canonical longform (вместо Medium из исходного плана). Medium → опц. зеркало позже.
4. **AQS-трекер** (AK-47: маленький скрипт/таблица, не сервис) — north-star метрика вместо лайков.
5. **Cooldown-гейт green/amber/red** формализовать в Decide-стадии (частично есть в fb-diary depresnyak-cooldown).
6. **Тул для X**: либо остаёмся custom draft-first, либо Typefully только для X. Решает Антон.
7. **Аллокация**: 40% X · 30% TG · 15% Reddit · 10% Substack · 5% FB · 0% Medium-first.

## 7. Связи
[[content-factory]] · [[facebook-diary-auto]] · [[fb-diary-voice]] · [[content-gershuni-style]] · [[main-goals]] (#1 двойник) · branch-tracker `_imports\branches\branch-content-factory.md` · крафт-DR `_CRAFT-research.md`.

**Ось GEO (цитируемость самими LLM, отдельно от охвата людей):** [[decision-geo-distribution-playbook]]. ⚠️ Для GEO X/Telegram = ⛔ (не цитируются LLM), хотя для людей X=40%/TG=30%. Разные оси, не конфликт.
