---
title: "Decision Memo — паритет с OpenClaw внутри Claude Code (без зоопарка)"
type: decision
stage: distilled
origin: anton
authored_by: hybrid
ai_author: claude-code-opus
date_established: 2026-06-19
theme: ai-tooling
status: active
audience: both
value_score: 0.8
decision: "НЕ ставить OpenClaw; достроить 3 узких разрыва нативно в Claude Code"
tags: [decision, openclaw, claude-code, alpha-protocol, digital-twin, no-zoo]
concepts:
  - OpenClaw
  - Claude Code
  - "[[concept-digital-immortality]]"
related_memory:
  - telegram-eventloop-listener
  - automation-inventory
  - prefer-included-limits-before-paid-api
  - main-goals
supersedes_research: "02-Decisions/Business-Finance/1775829251--исследование-openclaw-entropic-и-конкурентов-на-рынке.md"
---

# Decision Memo — OpenClaw-паритет внутри Claude Code

> Получено через Alpha Protocol (R+DR), 2026-06-15. Антон: «не хочу ставить OpenClaw, но хочу его топовый функционал внутри Claude Code — CC мне достаточно, не хочу зоопарка».

## Проблема
Получить топ-функционал OpenClaw (мульти-канальный агент, память, голос, scheduler, браузер, skills, routing) **внутри Claude Code**, не ставя OpenClaw и не плодя зоопарк инструментов.

## Текущие знания (Recall)
- **OpenClaw** = open-source computer-use агент-платформа (347k★, апр-2026). Anthropic в апреле 2026 **отрубил OpenClaw от Pro/Max-квот** → только платный API (бьёт по правилу `prefer-included-limits-before-paid-api`).
- Антон **уже самостоятельно повторил ~85% OpenClaw** внутри CC + MCP, а слой памяти у него **сильнее** (e5+reranker RAG, SQLite, волт 154k, unified-search, namesearch, dialogs.db).

## Результаты Deep Research (с оговоркой)
Внешний DR промахнулся по терминам: 2 из 3 отчётов исследовали **OpenCode** (другой продукт) вместо OpenClaw, а «Cloud Code» трактовали как CloudCode.ONE / Google Cloud Code, не Anthropic. **Грабля:** во внешний DR ушёл не дословный промпт — на будущее вставлять блок Alpha Protocol verbatim. Синтез всё равно надёжен: фичи OpenClaw взяты из верного отчёта, карта CC — из верного, решающую переменную (стек Антона) знаем сами.

## Карта паритета (OpenClaw × фактический стек Антона)
| Топ-фича OpenClaw | Статус в CC |
|---|---|
| Мессенджер-ассистент | ✅ Telegram (полный). ❌ WhatsApp/Slack/Discord |
| Запуск агента из чата (`openclaw-code-agent`) | 🟡 ~70% — `telegram-watch` Mode 2 отвечает; не исполняет |
| Браузер / OSINT | ✅ Chrome MCP + computer-use |
| Память / recall | ✅✅ ЛУЧШЕ OpenClaw |
| Cron / фон | ✅ Task Scheduler + scheduled-tasks + /loop |
| Голос (Talk + TTS) | 🟡 только Whisper STT-вход; нет цикла + TTS |
| Реестр skills | 🟡 свои скиллы; авто-реестра нет (=зоопарк, не нужен) |
| Routing по аккаунтам | 🟡 telegram-assistant whitelist — частично |
| Control UI / дашборды | ✅✅ много HTML-дашбордов |
| Карточки/кнопки в каналах | ✅ TG inline-кнопки в MCP |

**Coverage ≈ 85% (high).**

## Варианты
- **A. Ставить OpenClaw** — ❌ отвергнут: он и есть зоопарк + платный API после блокировки Anthropic.
- **B. Принять 85%** — валидно.
- **C. ✅ ПРИНЯТО: достроить 3 узких разрыва нативно в CC.**

## Риски
- ⚠️ `AUTH_KEY_DUPLICATED` — нельзя второй Telethon-клиент / headless `claude -p` на той же сессии (`telegram-eventloop-listener`).
- ⚠️ Авто-исполнение команд из чата = чувствительно → money/destructive gates Библии остаются.
- ⚠️ Headless-прогон строго по OAuth, не платному ключу (`prefer-included-limits-before-paid-api`).

## Рекомендации / дорожная карта (value × speed)
1. **Phase 1 — CC из Telegram (исполнение, не только ответ).** База `telegram-watch` готова на ~70%. Блокер: login `@platinumvc1` + membership в чатах. Инкремент: слой «действие», кнопка одобрения плана.
2. **Phase 2 — живой голосовой цикл.** GPU-local TTS + цикл поверх Whisper. Автономно строится.
3. **Phase 3 — WhatsApp/Slack/Discord MCP.** Research + install + auth аккаунтов Антона. ⚠️ усложнение — только если нужны (Антон живёт в Telegram).

Антон выбрал все 3 (2026-06-19) → цель = полный паритет. Связано с целью №1 [[main-goals]] (цифровой двойник).

## Кросс-чек (2-й DR, 2026-06-19)
4-й отчёт (OpenClaw × CloudCode.ONE-через-OpenCode — снова не Claude Code, рецепты не применимы) **независимо подтвердил** нашу карту: его «Partial/Not available» = ровно наши 3 разрыва (каналы, голос, mobile-nodes), а «Fully available» = то, что у Антона есть. 🎯 Ключевой инсайт: отчёт называет мессенджер-шлюз самым дорогим шагом (6–16 ч, делать последним) — а у Антона он УЖЕ готов (Telegram MCP + telegram-watch) → путь CC+MCP впереди рекомендуемой замены. Доп. фичи OpenClaw: Talk-mode (=Phase 2, подтв.), nodes камера/canvas (❌, вряд ли нужно), Workboard (✅≈ дашборды), sandbox/approvals (✅≈ permission modes + hooks + Библия). Уверенность в решении → very high.
