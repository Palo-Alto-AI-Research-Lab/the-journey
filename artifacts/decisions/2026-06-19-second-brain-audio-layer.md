---
title: "Decision Memo — аудио-слой Второго Мозга (подкасты за рулём, по-русски)"
type: decision
stage: distilled
origin: anton
authored_by: hybrid
ai_author: claude-code-opus
date_established: 2026-06-19
theme: ai-tooling
status: active
audience: both
value_score: 0.82
decision: "Локально-первый dual-mode пайплайн: RAG→скрипт→TTS по репликам→склейка→MP3 на телефон. Голос: Chatterbox V3 (рус.клон), фолбэк XTTS v2, база Piper. NotebookLM = ручная идея-машина, НЕ авто и НЕ голос двойника. ElevenLabs = отложенная премиум-опция только при явном ОК на платное."
tags: [decision, tts, voice-cloning, notebooklm, podcast, second-brain, digital-twin, alpha-protocol]
concepts:
  - "[[concept-digital-immortality]]"
  - "[[second-brain-northstar]]"
related:
  - "02-Decisions/decision-openclaw-parity-inside-claude-code.md (это flesh-out Phase 2)"
related_memory: [model-routing-sonnet-grunt, prefer-included-limits-before-paid-api, telegram-safety, vault-data-architecture, main-goals]
---

# Decision Memo — аудио-слой Второго Мозга

> Alpha Protocol (R+DR), 2026-06-19. Расширяет Phase 2 паритета с OpenClaw (`decision-openclaw-parity-inside-claude-code`). DR-отчёт «Building a Second Brain Audio Layer for Russian Driving-Time Consumption» — первый из серии, попавший точно в цель.

## Проблема
Антон хочет слушать **за рулём по-русски** аудио по своим темам (как подкасты NotebookLM из двух ведущих), в идеале **голосом своего цифрового двойника** — и решить local-free-private vs cloud-paid (ElevenLabs/NotebookLM), не нарушая правил.

## Текущие знания (Recall + сессия)
- Piper TTS уже стоит и работает (рус., быстро, бесплатно, БЕЗ клонирования).
- GPU RTX A3000 6 ГБ, faster-whisper стоит, волт 154k + RAG e5+rerank.
- Прежних заметок по NotebookLM/подкастам — нет (чистый лист).

## Результаты Deep Research (ключевое)
- **Архитектура-победитель (AK-47):** `RAG → topic pack → LLM-скрипт (соло ИЛИ два ведущих, рус.) → TTS по репликам → склейка FFmpeg → MP3 → синк-папка на телефон`. Разделять «время рассуждения» (RAG+LLM) и «время рендера» (TTS) — не слать весь волт в TTS.
- **Голос локально:** **Chatterbox Multilingual V3** (рус.+23 яз., клон с ~5с, MIT, free) = primary; **XTTS v2** (рус., клон с 6с) = фолбэк/A-B; **Piper** = «всегда работает» без клона. Kokoro — без клонирования и без рус. → мимо. FireRedTTS-2/MOSS-TTSD/Sesame CSM — фронтир, НЕ первый билд (тяжелы для 6 ГБ).
- **Русский — это benchmark-ПРОБЕЛ:** нет чистого 2025-26 рейтинга рус. клонирования → **не верить лидербордам, верить своему A/B-тесту в машине** (= правило «1-мин тест > память»).
- **Правило клонирования:** референс-клип на ТОМ ЖЕ языке, что вывод (рус. референс для рус., иначе утечка акцента).
- **Cloud:** ElevenLabs Creator+GenFM ($22/мес, 121k кред.) — лучший для подкаста ТВОИМ голосом (host/guest voice IDs, рус. PVC); лучше NotebookLM для цели-двойника. **NotebookLM** — силён в source-grounded структуре, НО **нет кастомного/клон-голоса**, реальный API только Enterprise, consumer-автоматизация = хрупкий Playwright. **AutoContent API** — худшая приватность (роутит через NotebookLM + третьих).

## Варианты
- **A. ✅ ПРИНЯТО: локально-первый dual-mode пайплайн** (соло read-aloud + two-host), голоса Chatterbox→XTTS→Piper. Чтит все правила (free/private/repairable/paid-only-by-OK).
- **B. NotebookLM как авто-движок** — ❌ как первый билд: хрупкая автоматизация, нет голоса двойника. Оставляем как РУЧНУЮ идея-машину.
- **C. ElevenLabs сразу** — ❌ сейчас: платно+облако (3 правила). Отложенная премиум-опция, если полировка/свой голос станут критичны и Антон даст ОК.

## Риски
- ⚠️ Chatterbox/XTTS на 6 ГБ Windows — установка может капризничать; A/B покажет реальность.
- ⚠️ Акцент-дрифт по-русски при коротком/шумном/неязыковом референсе.
- ⚠️ GPU-коллизия с параллельными сессиями (эмбеддинги/whisper) — память `reindex-routine`.
- ⚠️ NotebookLM browser-automation — хрупко, избегать как фундамент.

## Рекомендация / план (value × speed)
1. **Решающий эксперимент (дешевле любого DR):** A/B 3 голосов на одном рус. сэмпле с референсом голоса Антона — Chatterbox vs XTTS vs Piper. Судить: произношение, схожесть, усталость при долгом слушании. Антон выбирает УХОМ.
2. Голос-банк: чистый рус. сэмпл 5-10с сейчас (для zero-shot); ~2ч чистой речи позже (для pro-клона, если уйдём в ElevenLabs).
3. Пайплайн: topic-pack → 2 рус. шаблона скрипта (соло 6-10мин / два ведущих 8-12мин) → TTS по репликам → FFmpeg-склейка → MP3 → синк-папка.
4. Доставка: «самый тупой надёжный канал» — синк-папка с одним MP3 на эпизод; RSS/бот позже.
5. **Рутина-кандидат (`evaluate-recurring-into-routine`):** «новое в волте за неделю → авто-подкаст в машину» — оценить после рабочего MVP.

Связано с целью №1 [[main-goals]] (двойник твоим голосом). Только ПОСЛЕ A/B + Decision — продакшн.
