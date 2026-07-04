---
title: "Decision Memo — CRM усыхает до тонкого TG-движка, CC = кокпит (R+DR подтверждён)"
type: decision
origin: anton
authored_by: hybrid
ai_author: claude-fable-5
date_established: 2026-06-19
theme: crm-engine
status: active
confidence: 0.85
supersedes: "[[decision-crm-keep-cc-drop-decide]] (переводит DECIDE-корзину из открытой в решённую)"
tags: [decision, crm, charm, alpha-protocol, deep-research, cockpit, telegram]
concept: "[[concept-agency-operating-system]]"
related:
  - "[[decision-openclaw-parity-inside-claude-code]]"
  - "[[insight-charm-last-mile-moat]]"
  - "[[decision-ai-native-business-cc-codex]]"
  - "[[_CHARM-CRM-Engine-MOC]]"
dr_original: "_originals/dr-crm-cockpit-multiaccount-2026-06-19.md"
---

# 🅰️ Decision Memo — CRM → тонкий движок, CC → кокпит

> Итог Alpha Protocol (R+DR), 2026-06-19. RECALL показал: курс уже задан тремя прошлыми решениями ([[decision-openclaw-parity-inside-claude-code]], [[insight-charm-last-mile-moat]], [[decision-ai-native-business-cc-codex]]). Внешний Deep Research (`_originals/dr-crm-cockpit-multiaccount-2026-06-19.md`) закрыл 4 эмпирических пробела. **DR подтвердил тезис по всем пунктам.**

## Проблема
На сколько усушивать боевую CHARM CRM в тонкий движок, перенеся суждения в CC-кокпит. 4 открытых вопроса: (1) многоаккаунтный TG — моат или обуза? (2) как безопасно мигрировать? (3) холодную рассылку — лимит или убить? (4) внутренний инструмент vs продукт vs субстрат двойника?

## Результаты DR (с цифрами)
1. **Многоаккаунт = моат ПРИ инструментовке, обуза без неё.** Telegram банит агрессивно (~100k каналов/день в начале 2026). Жёсткие капы: **~5 холодных DM/день** на новый аккаунт (**15/день с Premium**), 1 сообщ/сек в чат, 20/мин в группе. Безопасно = прогрев 10–14 дней + уникальные IP/прокси + Premium + рандом-пейсинг + персонализация. Без анти-флуда десятки аккаунтов = «мина, выносит пайплайн за ночь». → ровно **наша №1 слабость (нет проактивного пейсинга)**.
2. **Миграция = Strangler-Fig / wrap-and-freeze, НЕ переписывание.** Фасад/API перед легаси, порт по одному модулю, старый и новый код параллельно, откат наготове. Big-Bang rewrite = провал (bus-factor=1, уходящий Денис). Бэкап Mongo — немедленно (**у нас уже сделан + ночной крон**).
3. **Холодная рассылка — убить/резко срезать.** Тёплые интро конвертят **15–30%** vs холод **1–5%**. Крипто-аудитория особенно враждебна к холоду. «Cold для разведки, warm для закрытия». → **Cold Pitching = DROP** (оставить максимум точечно по VIP, ≤15/день Premium, с opt-out).
4. **Позиционирование = внутренний инструмент / двойник, НЕ продукт.** Как продукт бьётся с Salesforce/HubSpot, что уже встраивают агентов → коммодитизация. Сила = уникальные данные Антона (10M+ симв. стиля) + плейбуки + сеть в Telegram. «Powered by your own outbound signature». Продуктизация — потом, нишево (white-label/API для web3), НЕ полный CRM-ребренд. → закрывает развилку **A↔D из [[decision-ai-native-business-cc-codex]] в сторону D** (движок = субстрат двойника, не отдельный продаваемый продукт сейчас).

## Решение (обновляет корзины [[decision-crm-keep-cc-drop-decide]])
- 🟢 **KEEP** — многоаккаунтный TG-движок, НО с обязательной достройкой **анти-флуд пейсинга** (`safe_send.py` уже заложен) — это условие того, что KEEP не станет обузой.
- 🔵 **→CC** — все суждения (кому/что писать, интро, квалификация, карточки) — курс подтверждён.
- 🔴 **DROP** — наследие команды **+ теперь явно Cold Pitching** (DR: убить).
- 🟡 **DECIDE → РЕШЕНО:** landing-api = НЕАКТИВНА → вывод из эксплуатации (факт-вопрос к Денису, не DR); ghost-fleet = погашен; Cold Pitching = DROP; позиционирование = внутренний/двойник.

## Дорожная карта (value × speed)
1. **Анти-флуд пейсинг = приоритет №1** (условие моата): достроить `safe_send.py` в движок (прогрев, капы 15/день Premium, рандом-задержки, health-check дохлых процессов, auto-throttle на 90% капа).
2. **Strangler-фасад:** новый тонкий слой (интерфейс отправки, расписание, шаблоны) перед легаси; порт по модулю; старый код живёт до катовера.
3. **Cold Pitching → выключить** в движке; воронка = тёплое/интро (`/intro`, `/pipeline`).
4. **landing-api → decommission** (подтвердить у Дениса что спит, затем снять).
5. Продуктизацию не начинаем — движок как субстрат двойника (Цель №1 [[main-goals]]).

**Уверенность 0.85** (high): DR совпал с recall и активами; неопределённость — реальная скорость безопасной миграции руками не-технаря + CC.

## Связи
[[protocol-alpha-protocol-recall-plus-deep-research]] · [[decision-crm-keep-cc-drop-decide]] · [[insight-charm-last-mile-moat]] · [[decision-ai-native-business-cc-codex]] · [[decision-openclaw-parity-inside-claude-code]] · память `crm-gitlab`
