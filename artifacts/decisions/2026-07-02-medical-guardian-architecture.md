---
title: "Decision Memo — Медицинский персональный помощник / хранитель здоровья (архитектура)"
aliases: ["Решение — медицинский хранитель: что и как строим", "Health Guardian architecture decision"]
date: 2026-07-02
type: decision
stage: distilled
origin: hybrid
ai_author: Claude
ai_author_basis: claude-code-synthesis-of-external-DR
decision_status: approved-direction
tags: [decision, health, longevity, medical-guardian, biomarkers, digital-twin, vital, architecture]
related: ["[[dr-medical-guardian-2026-07-02-EN]]", "[[concept-charm-lifeos-product-thesis]]", "[[main-goals]]", "[[health-import]]", "[[vault-data-architecture]]", "[[concept-longevity]]", "[[concept-digital-immortality]]", "[[medical-files-to-gdrive]]"]
---

# Decision Memo — Медицинский хранитель здоровья

Синтез Alpha Protocol (шаг 4): **recall** (память + RAG + волт) **+ внешний Deep Research** ([[dr-medical-guardian-2026-07-02-EN]]) → решения по архитектуре. Это **Vital-слой цифрового двойника** ([[main-goals]] цель №1, [[concept-charm-lifeos-product-thesis]] = «AI Immortality & Biological Longevity»).

## 1. Проблема
У Антона уже есть сырьё (2 Google-таблицы крови с 2016, 830 health-заметок, мед-папка на Drive, Gmail/TG-вотчеры), но кровь лежит как **текст/ссылки, а не данные**. Нужен помощник, который: ингестит и нормализует маркеры в запрашиваемый временной ряд, безопасно интерпретирует (правила + LLM), трекает longevity-маркеры во времени, проактивно флагает значимые изменения и готовит к визиту к врачу — **НЕ ставя диагнозы**.

## 2. Что говорит DR (ключевое, чего не было в recall)
- **Не сквозной мед-чат-бот, а 4-плоскостная система:** сырьё (immutable) → канонические наблюдения → правила → LLM. Смешивать плоскости = система, которую нельзя аудировать.
- **Граница безопасности:** «правила решают, что механически истинно; LLM объясняет, что это может значить». LLM нестабилен на повторных мед-задачах (GPT-4 давал разные категории риска одному кейсу) → **LLM никогда не единственный путь alerting/scoring**.
- **Стандарты как якоря схемы:** LOINC (что измерено) + UCUM (единицы) + FHIR Observation/DiagnosticReport (структура) + конвенция OMOP «храни и стандартный концепт, и дословный источник». Правило: **маппить один раз, оригинал не выбрасывать никогда** → переживает смену лаборатории/единиц.
- **Evidence-tiers обязательны в UI:** ApoB, HbA1c = Tier A (сильные). hs-CRP, Lp(a), omega-3 index, гомоцистеин = Tier B (полезны, неспецифичны/контекстны). Био-часы (DunedinPACE/GrimAge) = Tier D (экспериментально, не алертить на мелких сдвигах). Нельзя давать всем одинаковый вес.
- **Privacy:** local-first по умолчанию; облако только opt-in после деидентификации. Псевдонимизированные мед-данные = всё ещё персональные данные.
- **Поведение:** silent-by-default, multi-gate алерты (2 из 3: абсолютный порог + внутри-персонный сдвиг + persistence), бэктест на истории до выхода в live. Главные провалы рынка: овер-медикализация и плохая нормализация, маскирующаяся под «биологическое изменение».

## 3. РЕШЕНИЯ (что делаем / НЕ делаем)
1. **Scope = «doctor-prep + self-quantification evidence engine», НЕ псевдо-врач.** Обещание: «объединить мои данные, заметить значимое изменение, суммировать доказательства, подготовить к врачу, держать аргументацию аудируемой». ✅ совпадает с моим non-clinician boundary и эпистемической нейтральностью.
2. **Архитектура = 4 плоскости поверх СУЩЕСТВУЮЩЕГО стека** (не новый стек, AK-47):
   - Сырьё → мед-папка Drive `! 77 LONGEVITY…` + `_originals` ([[medical-files-to-gdrive]], [[preserve-originals-rule]]).
   - Канон → **новая `blood.db` (SQLite)**, FHIR-образная схема (см. §4) = факт-слой [[vault-data-architecture]].
   - Правила → детерминированный Python (0 токенов).
   - LLM → `brain_ask.py` (e5+rerank, локально) для retrieval + Claude только для constrained-отчётов.
3. **Rules-first.** Норма/аномалия, retest-due, delta/trend, конфликт A1c↔глюкоза, data-quality — без LLM. LLM подключаем ТОЛЬКО на этапе 3 (отчёты), не раньше.
4. **Evidence-graded во всём** (frontmatter `evidence_tier` на каждом маркере; цвет в дашборде). Не позволяем био-часам доминировать.
5. **Local-first privacy.** PHI не уходит в облако по умолчанию. Опция полностью-локального нарратива через **Ollama** (оценить, ⚠️ УСЛОЖНЕНИЕ — только если захотим убрать Claude из мед-нарратива).
6. **Silent-by-default alerting**, 3 класса (critical absolute / persistent adverse trend / action due), multi-gate, **бэктест на истории Антона до live**.
7. **НЕ делаем:** диагнозы; единый «health-score», мешающий сильные и слабые маркеры; авто-алерты без бэктеста; облачную отправку сырых мед-данных.

## 4. Каноническая схема `blood.db` (минимальная строка)
`person_id, observation_id, effective_datetime, loinc_code, source_code, source_label, specimen, method, value_number, value_text, comparator, ucum_unit, unit_source, reference_low, reference_high, reference_text, interpretation_code, status, lab_id, fasting_state, source_report_id, raw_file_uri, mapping_version`.
Производные (eGFR, HOMA-IR, non-HDL-C, TG/HDL, био-возраст) — **отдельная таблица `derived_measurements`** с provenance формулы/версии, НЕ перезаписывают сырьё.

## 5. Roadmap (приоритеты × скорость)
- **Фаза 1 — ingestion backbone (СЛЕДУЮЩИЙ ШАГ).** 2 Google-таблицы → `blood.db`: raw-строки + канонические наблюдения, LOINC/UCUM-маппинг, сохранение источника. DR прямо говорит: это шаг №1, до всякого «интеллекта». = задача «давай маркеры», которую Антон уже поставил в очередь.
- **Фаза 2 — rule engine** (абнормал/retest/trend/конфликт/QC).
- **Фаза 3 — constrained LLM-отчёты** (longitudinal summary, «вопросы врачу», trend с evidence-for/against, lay-объяснение; low-temp, schema-constrained, каждый ответ цитирует источник).
- **Фаза 4 — tiered alerting** после бэктеста на истории.

## 6. Открытые вопросы (для следующего решения, не блокеры Фазы 1)
- Ollama-локальный нарратив vs Claude (приватность ↔ качество гейта).
- Глубина FHIR (полный сервер не нужен — «FHIR-образные таблицы + простые SQL-проекции»).
- Источник LOINC-маппинга (ручной малый словарь под реальные ~N маркеров Антона vs полный LOINC).

## 7. Что copy / где двойник отличается
Копируем у Function/Superpower/InsideTracker/Blueprint/Human Longevity: широкие загрузки, чистые time-series, one-click retest, plain-language, clinician-prep. **Moat своего двойника = прозрачная логика + полное владение данными** (каждый raw-файл, каждое решение нормализации, каждая версия derived-score, каждое правило — проверяемы), чего SaaS не дают.
