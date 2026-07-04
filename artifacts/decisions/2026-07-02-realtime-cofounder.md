---
title: "Decision Memo — Real-time / ambient ко-фаундер (диспетчер поверх персоны)"
type: decision
stage: distilled
altitude: decision
audience: both
origin: anton
authored_by: hybrid
ai_author: Claude (Opus, in-session synthesis)
date_established: 2026-07-02
status: proposed
decided_by: pending
confidence: high
priority: should
method: alpha-protocol-recall-plus-dr
aliases: [real-time кофаундер, ambient cofounder, always-on cofounder, диспетчер кофаундера]
tags: [decision, cofounder, ambient-agent, event-driven, salience, digital-twin, alpha-protocol]
---

# Decision Memo — Real-time / ambient ко-фаундер

> **Alpha Protocol (R+DR).** RECALL + **три сошедшихся внешних DR** → синтез. Сырьё: `_originals/deep-research/2026-07-02-realtime-cofounder-dispatch-DR.md` (мозг/салиентность), `_originals/deep-research/long-term-attention-layer-2026-06-18.md` (+report2, глаза/capture), Cowork `_cowork-inbox/RESULT-cc-trigger-architecture.md` (диспетчер) + `RESULT-gatekeeper-telegram-architecture.md` (single-client закон). Три независимые разведки конвергентны → высокая уверенность.

## Проблема
Антону нужен кофаундер, который **видит всё, что у него происходит, и помогает в реальном времени** — проактивно, а не только по команде `/cofounder`. Вопрос: как построить always-on наблюдателя+советника поверх уже готовой персоны, чтобы это было полезно, а не назойливо, дёшево по токенам и безопасно.

## Текущие знания (RECALL — 80% трубопровода уже есть)
- Персона `/cofounder` построена (композит, единый промпт, заземление на CRM/метрики).
- Живой TG push-listener `events.py wait_for_settled_message`; MCP Gmail/Calendar/WhatsApp/Chrome; `browser_history.db`; `session-monitor`; RAG `brain_ask`; воронка `tg_followups.json`; сигнальный канал (tg_watchdog → Saved Messages, [[cc-alerts-to-chat-03]]).
- **Дыра = внешний 24/7 диспетчер** ([[telegram-eventloop-listener]]).
- 🧨 Грабли: `AUTH_KEY_DUPLICATED` — один живой клиент на сессию, НЕ 2-й клиент, НЕ `claude -p` с дублирующим MCP.

## Что дал Deep Research (ключевое, конвергентно)
**Стратегия одной строкой: «слушай все потоки, но говори только когда ценность вмешательства > цены прерывания».**
- **Наука прерываний:** плохо-таймленные уведомления съедают 5–28% рабочего времени (~25 мин на возврат в задачу). Провал Clippy = «влезал не вовремя». Побеждают **congruent, gentle nudges** под текущую цель. Нужна калиброванная модель салиентности, иначе «notification fatigue → выключат».
- **Архитектура = event-driven**, не polling. Реагируй в момент события.
- **Стоимость: cheapest-tool-first** — дешёвый детерминированный/ML-фильтр отбирает high-salience, LLM зовём ТОЛЬКО на них (plan/prompt-caching ~40–50% экономии). = ровно твоя лестница [[vault-data-architecture]].
- **Диспетчер (cc-trigger):** ОДИН table-driven `signal-dispatcher` на хабе (одна запись Task Scheduler, 0 токенов), читает декларативную таблицу источников, роутит каждое событие → **ping-человека (дефолт)** или **wake `claude -p` (только по явному флагу)**. Состояние в файлах (mailbox+ledger) → события между тиками не теряются. Обобщает зоопарк bespoke-watcher'ов в одну петлю.
- **Human-in-the-loop:** любое исходящее/необратимое/деньги → эскалация на подтверждение (= [[operating-agreement]] Tier-2 / QQQ). Kill-switch (одна команда «замолчи») обязателен.
- **Приватность:** фильтровать секреты/PII в памяти ДО отправки в LLM; deny-apps (парольники/банк/Signal); шифр-диск. Глаза = минимально достаточный сигнал ([[decision-long-horizon-attention-layer-2026-06-18]]: ActivityWatch backbone, Screenpipe только time-boxed, НЕ 24/7-скриншоты).
- **Наблюдаемость:** дашборд «событие → триггер → совет → реакция» (= [[prefer-visual-dashboards]]).

## Решение (архитектура — переиспользуем всё, АК-47)
1. **ГЛАЗА (наблюдение):** существующие коннекторы (TG listener · Gmail · Calendar · `browser_history.db` · `session-monitor` · CRM `tg_followups.json`) как источники событий; ActivityWatch добавляем позже (по attention-layer решению). БЕЗ 24/7-скриншотов.
2. **ДИСПЕТЧЕР (петля):** `signal-dispatcher` из cc-trigger — одна 0-token петля на хабе, декларативная таблица источников, файловый mailbox+ledger, ОДИН живой telethon-клиент.
3. **САЛИЕНТНОСТЬ (когда говорить):** детерминированный фильтр (правила/ключевые слова) → класс High/Medium/Low. High → немедленный gentle DM; Medium/Low → **батч-дайджест** (agent-inbox), не дёргать. LLM-персону зовём только на прошедших фильтр. Хард-кап N вызовов/час.
4. **СОВЕТНИК (что сказать):** персона `/cofounder` (готова) генерит совет на high-salience событие, заземляясь на company-context + RAG. Модель = Opus (мышление); фильтр = 0 токенов / Sonnet на пограничном ([[model-routing-sonnet-grunt]]).
5. **КАНАЛ:** Telegram Saved Messages / сигнальный чат (существующий). Формат: `Источник · Суть · Совет`.
6. **ЧЕЛОВЕК В ПЕТЛЕ:** исходящее/необратимое → эскалация (QQQ), не авто. Kill-switch «пауза алертов».
7. **ДАШБОРД:** визуальный лог событий→алертов→реакций (для доверия + отладки салиентности).

## Что отвергли (и почему)
- **24/7 скриншот/аудио-capture как дефолт** — макс. риск приватности + storage-bloat; Screenpipe только time-boxed (совпадает с attention-layer решением).
- **Тяжёлый фреймворк (Kafka/Redis/LangChain/Temporal)** — ⚠️ УСЛОЖНЕНИЕ для N=1; cc-trigger уже отверг в пользу файлового mailbox+ledger + одной Task Scheduler записи.
- **`claude -p` self-wake как дефолт** — стоимость + AUTH_KEY landmine; дефолт = ping-человека, wake только по явному флагу на сигнал.
- **Кофаундер как автономный актор** — DR прямо: человек в финальной петле на всём важном.

## Риски → митигации
- Notification fatigue → строгий фильтр, дайджест для Low/Medium, обучение на отказах (👍/👎), «лучше пропустить, чем задолбать».
- Cost overrun / runaway loop → хард-кап вызовов/час, бюджет-алерт, 0-token диспетчер.
- Секрет-лик в LLM-промпт → deny-apps + фильтр PII в памяти до отправки.
- AUTH_KEY_DUPLICATED → один живой клиент, переиспользовать listener, не плодить.
- Salience mis-fire → итеративная калибровка на одном потоке до расширения.

## Рекомендация — фазами (АК-47, один первый шаг)
- **Phase 0 (первый, дёшево-ценно):** подключить ОДИН поток — CRM/лид-события (`tg_followups.json` уже есть) → диспетчер → правило-салиентности → персона `/cofounder` → **дайджест в сигнальный чат, ping-only** (без авто-действий). Доказать модель прерываний на самом дешёвом высокоценном потоке.
- **Phase 1:** + срочный VC-email + конфликт календаря.
- **Phase 2:** ActivityWatch (capture-слой) по attention-layer решению.
- **Phase 3:** Screenpipe time-boxed под исследовательские спринты.
Только после greenlight Антона — реализация Phase 0.

## Открытые вопросы
Порог салиентности (калибруется на реальных событиях Phase 0); где именно крутить диспетчер (хаб `A-2022BAYAREA`, всегда-вкл — по умолчанию); частота дайджеста (нач. 2–3×/день, вне ночного окна).

## Связи
[[decision-synthetic-cofounder]] · [[decision-long-horizon-attention-layer-2026-06-18]] · [[telegram-eventloop-listener]] · [[cc-alerts-to-chat-03]] · [[vault-data-architecture]] · [[model-routing-sonnet-grunt]] · [[operating-agreement]] · [[prefer-visual-dashboards]] · [[insight-glavnaya-tsel-tsifrovoj-dvojnik]] · [[alpha-protocol-recall-plus-dr]]
Cowork-детали для стройки: `_cowork-inbox/RESULT-cc-trigger-architecture.md` · `RESULT-gatekeeper-telegram-architecture.md`
