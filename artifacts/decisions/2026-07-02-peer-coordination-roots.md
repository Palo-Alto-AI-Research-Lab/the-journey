---
title: "Root-анализ межмашинной координации пиров (полный аудит 24.06–02.07)"
type: decision
stage: distilled
altitude: decision
audience: both
origin: anton
machine: hub
authored_by: hybrid
ai_author: Claude (Fable 5, hub session + 2 Sonnet-агента)
date_established: 2026-07-02
status: proposed
confidence: high
tags: [decision, machine-bus, consensus, syncthing, fleet, root-cause, watchdog]
---

# Root-анализ координации пиров — 2026-07-02

> Заказ Антона: «проанализируй ВСЮ координацию между пирами, весь лог мучений, пообщайся с пирами сам, разберись в корне». Метод: 2 read-only Sonnet-агента (хронология по ~385 сессиям + аудит живого состояния) + живой разбор TG-03 (240 сообщений) + консенсус с ноутом. Дашборд: `_Dashboards/Peer-Coordination-Root-Analysis.html`.

## Итог одной строкой
Флот НЕ гниёт — он молодой: 19 сессий мучений за 8 дней дали 10 классов сбоев, большинство точечно починено, НО пять СИСТЕМНЫХ корней остаются и продолжают порождать новые «казалось-бы-новые» инциденты.

## 5 системных корней (клас­сы, не симптомы)
1. **Нет флот-манифеста версий.** Конфиги/скрипты/пути дрейфуют по машинам (C:/D:/E:, Device ID, COMPUTERNAME, .stignore per-device, версии machine_bus). Фиксы едут туда, где шла отладка; раскладка держится на памяти → принцип «one-system-propagate» ([[reglament-pravka-obshchey-infry-ne-gotova-poka-ne-razlozhena-vezde-i-proverena]]) нарушался ≥3 раза. → Лечение: манифест «компонент → версия → машина» + ночная хэш-сверка (расширение System Architect до fleet-уровня).
2. **Сторожа без предохранителя.** Watchdog убил Syncthing по ложному 403 (пила каждые 18 сек, 2 машины); consensus-tick лидера 18 раз подряд слал self-ACCEPT #f60135de (self-accept не засчитывается в agreed — баг state-машины); heartbeat хаба кричит «краны 38/40 🔴» при живых по schtasks задачах (датчик врёт). → Лечение: circuit-breaker у любого сторожа + правило «красный вердикт = 2 независимых источника до действия».
3. **«Жив» ≠ «несёт службу».** Роботы тикают (alive), но не исполняют duty: ноут тикал consensus, но не отвечал; архиватор ноута молчит с 24.06 при живом heartbeat; Мак первой напарницы флапает Syncthing, но робот мёртв 7 суток; у второй робот жив, но `claude /login`/cron_watchdog не стоят. → Лечение: heartbeat v3 = per-duty last-success (архиватор/respond/ACK/бэкап), не «процесс жив».
4. **Протокол считает всех always-on, а always-on только хаб.** Ноут спит с крышкой, Мак спит, тайм-ауты консенсуса (240 мин) не различают классы узлов → предложения умирают об спящих. → Лечение: классы узлов (always-on / intermittent) с разными SLA + durable-очередь приказов до пробуждения.
5. **Шум хоронит сигнал.** ~90% чата 03 = heartbeat/алерты (240 сообщений за ~5ч); QQQ-запросы тонут. → Лечение: heartbeats в отдельный поток/почасовой rollup; 03 = только события и ASK.

## Что уже сработало (не трогать, работает)
- Per-sender файлы шины (root-fix 25.06) — конфликтов записи больше нет.
- Dual-send + auto-failover Syncthing→TG — доказан в бою этой ночью.
- 5 полных двусторонних consensus-циклов PROPOSE→ACCEPT→COMMIT→VERIFY уже случились.
- Конвергентная диагностика: хаб и ноут НЕЗАВИСИМО нашли один корень «start-race задач при logon/wake» (0x800710E0 / нет StartWhenAvailable). Фикс хаба применён; зеркалирование флотом = консенсус #e53fd7fe (ждёт QQQ).

## Открытые решения Антона (QQQ)
- #e53fd7fe — зеркалировать фикс start-race на весь флот (tier 2).
- #0b1d5d4f — план по рецидиву чужой-ID ноута + osc-риск сторожей (tier 2).
- Дубль-группа с одним из пиров (инцидент 25.06) — удалить/оставить, висит с 25.06.

## Физически на людях
- Мак первой напарницы: робот мёртв 7 дней — нужно открыть Мак/перезапустить launchd (она сама).
- Вторая напарница: `claude /login` + доставить cron_watchdog модуль (робот жив, доставку сделает хаб через _transit, логин — она).
- DHCP-резервация IP хаба на роутере — Антон, руки.
- Ноут: держать открытым до VERIFY #a7c918ff (транскрипты с 24.06 всё ещё не доехали).

## Недоделки-хвосты (из аудита, полный список в дашборде)
`/bus-heal` не собран · circuit-breaker TODO · email-рельс не подтверждён · git-bus/n8n-bridge без peer-ACK · Telethon апгрейд (TypeNotFoundError, риск дубль-групп) · FIX B (пер-машинные TG-сессии) не доехал до хаба · 43+15+5 sync-conflict мусора · вайп-восстановление ноута застряло (89%→4.5%, не подтверждено).

## Связи
[[reglament-multi-machine-claude-i-peredacha-mezhdu-mashinami]] · [[reglament-trigger-03-avtonomnyy-konsensus-mashin]] · [[reglament-pravka-obshchey-infry-ne-gotova-poka-ne-razlozhena-vezde-i-proverena]] · [[reglament-chp-poterya-sinka-mezhdu-mashinami]] · [[decision-realtime-cofounder-2026-07-02]]
(CC-memory каноны без волт-двойников, grep-ссылки: one-system-propagate · machine-bus-telegram-rail · multi-machine-auto-consensus · sync-self-heal-layers · system-architect · session-pipeline-rails · fix-root-cause-not-symptoms)
