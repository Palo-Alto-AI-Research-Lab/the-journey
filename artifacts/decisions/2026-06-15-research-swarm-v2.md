---
title: "Decision — Research-swarm v2: control plane (Verifier-Calibrator + abstain)"
aliases: ["Решение — рой v2: проверяющий + воздержание", "research-swarm v2 upgrade"]
date: 2026-06-15
type: decision
stage: distilled
origin: anton
status: accepted
tags: [decision, research-swarm, alpha-protocol, epistemic-neutrality, control-plane]
---

# Decision — Research-swarm v2: добавить control plane (Verifier-Calibrator + abstain)

> Принято через **Alpha Protocol (R+DR)**: RECALL → Deep Research (ChatGPT, отчёт `swarms deep-research-report.md`) → синтез на Opus → этот Decision Memo. Канон протокола: [[protocol-alpha-protocol-recall-plus-deep-research]].

## Проблема
`/research-swarm` ([[research-swarm-skill]]) строит карту аргументов 5 линзами (Consensus / Skeptic / Frontier / Historian / Experimental-Design) + синтез. Вопрос Антона: **хорошо ли он сделан по сравнению с рынком 2025-2026.**

## Текущее знание (RECALL)
Рой — операционная рука [[protocol-epistemic-neutrality-fringe-research]]; «карта, а не вердикт»; тиры уверенности established/emerging/speculative/fringe. Слабое место подозревалось в обосновании тиров.

## Результаты Deep Research
- **Вердикт:** наш **content plane** (5 линз) — на уровне рынка или впереди (баланс аргументов, Experimental-Design как первоклассный выход — редкость; «no verdict» лечит риторическую ригидность forced-stance дебатов). Отставание — в **control plane**.
- **7 gap'ов, все в контроле:** (1) нет verifier/acceptance-функции; (2) нет durable evidence-memory; (3) статичная топология линз; (4) нет abstain-полосы; (5) тиры качественные, не калиброванные; (6) грубая схема карты; (7) нет аудита источников/допущений.
- **Размер роя:** универсального числа нет; малое разнородное ядро (5-8) бьёт большую однородную толпу. Наши 5 «уже в нужном диапазоне». Ошибка была бы — добавить десять агентов ДО того, как добавлен planner/verifier/memory.

## Варианты и решение (триаж по [[ak47-simplicity]])
| Gap | Ценность | Цена | Решение |
|---|---|---|---|
| Verifier-Calibrator | высокая | низкая (1 агент в том же workflow) | **✅ ВЗЯТО** |
| Abstain-полоса (`insufficient`) | высокая | ~ноль (строка промпта + enum) | **✅ ВЗЯТО** |
| Source-Quality + Assumption pass | средняя | низкая если вшить в линзы | вшить в промпты линз (позже) |
| Durable evidence-graph | средняя | ⚠️ усложнение (отдельная БД) | ❌ отложено |
| Planner/Router | низкая для нас | ⚠️ средняя | ❌ хватает флага «глубже» |
| Бенчмарк-батарея | низкая для нас | высокая | ❌ для продукта на итерациях, не для on-demand |

## Что реализовано (2026-06-15)
6-й узел **Verifier-Calibrator** (acceptance-функция, не ещё одно мнение): аудит сильнейших claims (supported/partial/unsupported) → пересчёт тира от **доказательств** (плотность опоры + согласие линз), не от «ощущения»; может **воздержаться** → новый тир `insufficient`. Калиброванный тир перетирает синтез-тир (`pre_calibration_tier` сохраняется); блок `calibration` + флаги рендерятся в дашборде. Файлы: `workflow.js` (фаза Verify + VERIFY_SCHEMA), `build_argmap_dashboard.py` (тир insufficient + блок калибровки), `SKILL.md`.

## Риски / границы
- Сохранено как сила: 5 линз, Experimental-Design, «карта-не-вердикт» — не трогать.
- DR — архитектурный вывод, не измеренный бенчмарк; часть цитируемых систем (WebWeaver, FS-Researcher, VeriTrace) — препринты.
- Отложенные gap'ы — кандидаты на v3, если рой станет продуктом на итерациях (не сейчас).

## Связи
[[research-swarm-skill]] · [[protocol-epistemic-neutrality-fringe-research]] · [[protocol-alpha-protocol-recall-plus-deep-research]] · [[main-goals]] (кирпич цели №1 — двойник рассуждает как Антон-исследователь).
