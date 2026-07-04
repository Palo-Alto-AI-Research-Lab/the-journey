---
title: "Decision Memo — Единый поисковый слой Второго Мозга (гибрид: federated router + тонкий unified catalog)"
type: decision
stage: distilled
origin: anton
authored_by: human
date_established: 2026-06-15
theme: second-brain-architecture
status: active
audience: both
priority: should
confidence: 0.85
aliases: ["Решение — Unified search layer", "Decision — Единый поиск Второго Мозга", "search_catalog.db"]
tags: [decision, alpha-protocol, second-brain, search, retrieval, sqlite, deep-research, digital-twin]
concept: "[[concept-digital-immortality]]"
---

# 🅰️ Decision Memo — Единый поисковый слой Второго Мозга

> Итог Alpha Protocol (Recall + Deep Research + Synthesis), 2026-06-15. Протокол: [[protocol-alpha-protocol-recall-plus-deep-research]]. Кирпич Цели №1 — цифрового двойника ([[concept-digital-immortality]]). Источник истины архитектуры данных: память `vault-data-architecture` (3-слойка + токен-закон).

## Проблема
Поиск по Второму Мозгу раздроблён: RAG `brain_ask` (только курируемый слой), `names.db` (имена), grep (сырые 154k .md), отдельные фактовые SQLite (browser/contacts/CRM/health/gdrive). Нет ОДНОЙ точки входа «найди везде». Риск при объединении — плодить индексы, которые разъезжаются (нарушение АК-47).

## Текущие знания (RECALL)
3-слойка (SQLite факты + RAG смысл + markdown мысли) + лестница SQL→grep→RAG→LLM; e5+reranker через `ask_server` :8770; `names.db`+`find_chat`/translit; `dialogs.db` (титул-индекс 10k чатов, собран 2026-06-14). Грабли: коллизии параллельных сессий на GPU/общих файлах, дрейф индекса, устаревший кэш коннектора.

## Результаты DR (главное)
- **Победила НЕ крайность**, а **гибрид: federated query router + тонкий unified retrieval-слой**. Структурное спрашиваем нативным SQL/grep; унифицируем только **поисковую проекцию** (текст-срезы для BM25 + эмбеддинги + канон-метаданные), а НЕ всю базу.
- **Ядро-движок = один файл `SQLite FTS5 (BM25) + sqlite-vec (вектор, версию ПИННИТЬ) + RRF + маленький cross-encoder reranker`** поверх top-20–50. Оркестратор — лёгкий локальный CLI/роутер.
- **Конвейер:** запрос → намерение → точный (entity) lane → нативный SQL lane → BM25 lane → вектор lane → **RRF-слияние** → rerank → сниппеты → *опц.* LLM-синтез.
- **LLM — только финальный синтез** (+ опц. query-rewrite после провала дешёвых путей). Не в retrieval, не в разрешении личностей.
- **Личности across источников:** канон-сущности + псевдонимы + `same_as`-рёбра с уверенностью; детерминированно сразу, вероятностно (Splink-стиль) офлайн; «связать ≠ слить», обратимо; ER по структурным полям (контакты/CRM/метаданные), НЕ по тексту заметок.
- **Свежесть без дрейфа:** индекс = производный артефакт, ключ = хэш контента + версия парсера/чанкера/модели-эмбеддера; перестраиваемость обязательна (триггеры FTS5 НЕ добивают старые строки → нужен явный rebuild/initial load).

## Что НЕ брать (из DR)
- **DuckDB** ядром (FTS не авто-обновляется; VSS persistence experimental, WAL-риск) — только как офлайн-аналитический верстак для gold-set/ER.
- **LanceDB** ядром (требует периодического реиндекса; брать только если станем «вектор-центричны»).
- **Meilisearch / Typesense / полный Onyx** — сервер-зоопарк (очереди, демоны) против «один файл/процесс».
- **Глобальный GraphRAG** — дорого по GPU/токенам, плохо инкрементится. Граф — только тонкий доменный (entity-таблицы + `same_as`), не LLM-граф по всем заметкам.
- Доноры идей (не движок): **Khoj** (Django+Postgres+pgvector), **Onyx** (enterprise). Мораль провала **Reor** (заброшен 03.2026): «ядро отдельно от продукта/UI».

## Риски и страховка
| Риск (DR) | Страховка |
|---|---|
| Иллюзия синхронизации (индекс ≠ источник) | индекс перестраиваемый; freshness = явный тест хэша |
| Протухший ANN | пиннить sqlite-vec, простой режим, реиндекс по расписанию |
| Ops creep | никаких демонов; один файл/процесс |
| Сплющивание фактов в текст | структурное спрашиваем нативно, не чанкуем |
| Амбиция > поддержки (Reor) | строим «скучное ядро», UI/скиллы — обёртки |
| Коллизия параллельных сессий | уникальные temp-файлы, single-instance lock на реиндекс, GPU-гард |

## Решение (РЕШЕНО Антоном 2026-06-15)
Принять **гибрид**. Ядро = `search_catalog.db` (SQLite FTS5 + sqlite-vec + RRF + мелкий reranker), роутер-CLI; переиспользовать e5 + `names.db` + `dialogs.db` + `ask_server`. **Первый кирпич = поиск по чатам title+content** (объём выбран Антоном: сразу title+content, не только титулы).

## План фазами (на каждом шаге — «дешёвый инструмент раньше дорогого»)
1. **Чаты (первый кирпич):** `search_catalog.db` поверх контента чатов в волте (`01-Conversations/**`: Personal-DMs, Pokupki, FAAA, Platinum-DMs, health, claude/chatgpt) + титулы из `dialogs.db`. FTS5 сперва (0 GPU), потом sqlite-vec + RRF + rerank. Gold-set реальных запросов + Recall@k/MRR.
2. **Заметки:** лексически (FTS5) первыми, эмбеддинги выборочно.
3. **Фактовые сторы** (browser/contacts/CRM/health/gdrive): через роутер, нативный SQL первым.
4. **Entity-слой:** канон+псевдонимы+`same_as`, обратимо.
5. **Закалка ранкинга** (source priors, recency decay); *потом опц.*: BGE-M3 / Tantivy / LanceDB / выборочный граф.

## Связанное
- `vault-data-architecture` (канон 3-слойки + токен-закон) · `smart-name-search` (`names.db`) · `dialogs-index` (`dialogs.db` — титул-кирпич) · `second-brain-layer` · `reindex-routine` · [[decision-alpha-extraction-engine-variant-a]] (соседний 4-й слой «insight engine»).
- DR-отчёт (внешний): «Unified Local-First Personal Search Architecture for a Windows Second Brain», 2026-06-15.
