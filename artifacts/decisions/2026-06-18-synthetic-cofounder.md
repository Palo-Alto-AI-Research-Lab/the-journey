---
title: "Decision Memo — Синтетический ко-фаундер (композит, не клон звезды)"
type: decision
stage: distilled
altitude: decision
audience: both
origin: anton
authored_by: hybrid
ai_author: Claude (Opus, in-session synthesis)
date_established: 2026-06-18
status: active
confidence: high
priority: should
method: alpha-protocol-recall-plus-dr
aliases: [синтетический кофаундер, virtual cofounder, AI cofounder, ко-фаундер]
tags: [decision, cofounder, ai-agents, persona, digital-twin, alpha-protocol]
---

# Decision Memo — Синтетический ко-фаундер

> Получено по [[protocol-alpha-protocol-recall-plus-deep-research|Alpha Protocol]]: RECALL → GAP → DR (внешний отчёт Антона) → Synthesis. Внешний DR-отчёт «Building a Synthetic Cofounder for the AI Era» дословно — `_originals/deep-research/2026-06-17-synthetic-cofounder.md` (origin: external).

## Проблема
Антону нужен агрессивный, наглый, успешный **виртуальный ко-фаундер** для бизнеса — спарринг-партнёр, который давит, считает деньги, заставляет двигаться. Профиль: «25 лет энергии, но опыт серийного фаундера (раунды, кредиты)». Вопрос: на кого ориентироваться (Маск?) и как собрать синтетического кофаундера с несколькими характеристиками.

## Текущие знания (RECALL — это НЕ чистое поле)
- Антон сформулировал «**копайлот для фаундеров и инвесторов, принимающий решения**» ещё 2023-12-24 → [[concept-digital-immortality]]. Кофаундер — давняя его идея.
- [[insight-ai-klon-agent-deystviya-ne-chat-bot]] — клон = агент ДЕЙСТВИЯ, не чат-бот (memory graph + behavioral model + orchestration).
- [[insight-personality-core-build-from-canonical-messages-p]] — оцифровка: граф + ручной pin канонических сообщений (anti-drift).
- [[persona-platform-cloud-code-2026]] + [[decision-persona-platform-roadmap]] — «личность = конфигурация (7 слоёв), не магический промпт».
- [[2025-01--топ-10-платформ-для-цифровой-памяти-и-персональных-ai-двойников]] — конкурентная карта рынка двойников.

## Результаты DR (внешний отчёт, 2026)
- Побеждают **AI-native, предельно быстрые, заземлённые на клиента, капитал-грамотные операторы с конструктивной конфронтацией** (Carta: AI >60% венчура Q1-2026; малые команды; venture debt рекорд $68.8B 2025).
- Рабочие клоны **заземлены на корпус, не на вайб** (Delphi, Personal.ai). Серьёзные операторы **держат человека в финальной петле** (Reid Hoffman, Bartlett). AI-персоны = структурированный скелет мышления, **не замена реальности** → нужен встроенный red-team.

## Решение
1. **НЕ клонировать Маска / одну звезду.** Строим **КОМПОЗИТ 6 черт** фаундеров 2023–2026: Wang (деньги+таланты) · Srinivas (скорость+публичная итерация) · Luckey (миссия+тяжёлые продажи; ближе Маска по духу) · Lovable (AI-native абстракция) · Grove/Horowitz (конструктивная конфронтация) · YC (одержимость клиентом). Совпадает с DR И с записью Антона 2023.
2. **Форм-фактор (Антон выбрал): и скилл, и Custom GPT.** Единый промпт-источник → два деплоя (не разъезжаются).
3. **Заземление (Антон выбрал):** Platinum CRM + воронка лидов + метрики/runway (FILL-слоты) + бизнес-концепты волта.
4. **Кофаундер ≠ суверен:** даёт лучший аргумент + downside; необратимое/деньги/юр. — решает Антон ([[operating-agreement]] Tier-2).
5. **Граница с [[coach-system]]:** coach = про ТЕБЯ (личность); cofounder = про БИЗНЕС. Дубля нет.

## Что отвергли (и почему)
- **«Маск из чатбота» / клон одной звезды** — рынок 2023–2026 награждает композит-оператора, не культ личности.
- **Новая платформа двойника** — АК-47: переиспользуем persona-7-слоёв + память + Bible, не строим сервис.
- **Кофаундер как финальный решала** — DR прямо предупреждает: человек в петле.

## Риски
- Персона усредняет/завышает оптимизм → митигация: встроенный Red Team / Council Mode.
- Галлюцинация цифр → правило «нет данных = вытребовать, не фантазировать».
- Дрейф персоны со временем → единый версионируемый промпт (git-история `~/.claude`).

## Реализация
- Скилл `/cofounder` (`~/.claude/skills/cofounder/`): `SKILL.md` + `references/system-prompt.md` (единый RU+EN промпт) + `references/company-context.md` (заземление).
- Custom GPT: вставить `system-prompt.md`, загрузить заполненный `company-context.md` §A.
- **Следующий шаг Антона:** заполнить FILL-цифры (выручка/burn/runway/топ-3 проблемы) → первый прогон `/cofounder` или Council Mode.

## Связи
[[insight-glavnaya-tsel-tsifrovoj-dvojnik]] · [[concept-digital-immortality]] · [[concept-business-strategy]] · [[concept-startup-fundraising]] · [[concept-charm-lifeos-product-thesis]] · [[coach-system]] · [[decision-persona-platform-roadmap]] · [[protocol-alpha-protocol-recall-plus-deep-research]]
