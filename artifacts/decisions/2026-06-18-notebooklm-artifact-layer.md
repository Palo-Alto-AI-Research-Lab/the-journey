---
title: "Решение: NotebookLM как artifact-слой над Вторым Мозгом (через Claude Code)"
type: decision
stage: distilled
origin: anton
authored_by: claude-code
date: 2026-06-18
decided: 2026-06-18
method: Alpha Protocol (R+DR) + live validation
status: decided
tags: [decision, notebooklm, second-brain, digital-twin, alpha-protocol]
concept: "[[concept-tech-tools]]"
concepts:
  - "[[concept-tech-tools]]"
  - "[[concept-digital-immortality]]"
audience: both
---

# Решение: NotebookLM = artifact/explanation-слой над волтом (НЕ память)

> Принято 2026-06-18 по Alpha Protocol (Recall → Gap → 2× Deep Research → Synthesis) **+ живая проверка в браузере**. DR-отчёт: [[DR-notebooklm-claude-code-2026-06-16]] (raw в `_originals/deep-research`). Индекс рабочего пространства: [[_NotebookLM-Index]].

## Проблема
Как подружить NotebookLM с Claude Code поверх Второго Мозга (128k заметок + e5/rerank RAG + SQLite-факты), приватно, по правилу «сначала включённые лимиты», по АК-47 — как кирпич ЦЕЛИ №1 ([[main-goals]], цифровой двойник).

## Что знаем (recall + 2 DR, сходятся → high confidence)
- **NotebookLM ≠ память и ≠ RAG-ядро.** Это слой **производных артефактов** (Audio Overview / debate / study guide / mind map / video / reports). Моат — grounded-озвучка и учебные трансформации, не «RAG с UI».
- Лимиты подтверждают: хорош на узких curated-ноутбуках (PRO: 300 источников / 20 аудио в день), плох как пожизненный архив; ноутбуки изолированы, источники — **статичные копии** (нет авто-синка).
- **Claude Code = слой-куратор:** `волт → CC собирает узкий пакет (10–80 заметок) → NotebookLM`, а не «весь волт → NotebookLM».

## Решение (архитектура)
```
Obsidian (истина) → RAG e5+rerank + SQLite (факты)
   → Claude Code (КУРАТОР: пакет + manifest + privacy-class)
   → [approval] → NotebookLM (UI/PRO): аудио / study guide / mind map
   → артефакты ОБРАТНО в волт как Derived (НЕ в source)
```
NotebookLM = **«голос и учитель» двойника**, не его память.

## Путь доступа (ПРОВЕРЕНО вживую 2026-06-18)
- **Чистого API-загрузчика у Антона НЕТ:** программный API только у NotebookLM **Enterprise** (Google Cloud, мин. 15 лицензий ≈ от $135/мес). Все аккаунты Антона `@gmail` = consumer; Workspace-аккаунта нет.
- **Рабочий путь = Chrome-автономия:** Claude Code сам ведёт залогиненный NotebookLM в браузере (создать ноутбук → Add sources → Paste text/Upload → Studio: генерация → Download). **0 денег** (тариф PRO на `a2`). Совпадает с [[chrome-autonomy-self-drive]] + [[ak47-simplicity]] + [[prefer-included-limits-before-paid-api]].
- `notebooklm-py` — только мост/прототип (undocumented API, ban/ToS-риск); browser-automation робот — избегать.
- **Грабли (находка):** булк-выгрузку артефактов делать через **Download/export**, НЕ через DOM-скрейп (вывод режется ~1000 симв.).

## Приватность
Consumer-аккаунт: на данных не учат по умолчанию, НО feedback может попасть к human-ревьюерам → **личный `a@` наружу не гоняем**; рабочая песочница = `a2`; чувствительное — privacy-class перед загрузкой; для высокочувствительного — Workspace/Enterprise.

## Риски и митигатии
загрязнение волта (пересказ пересказа) → слои Source/Derived/Generated · дрейф-галлюцинации → артефакт в факт-слой только после approve ([[epistemic-decay-layer]]) · потеря provenance → manifest + source_hash.

## План (фазами, АК-47)
- **Фаза A ✅:** индекс 78 ноутбуков ([[_NotebookLM-Index]]).
- **Фаза B (точечно):** выгрузка готовых артефактов из топ-ноутбуков в волт как Derived (образец: [[Systemic-Enzymotherapy-Biohacking]]).
- **Фаза C ✅ доказана:** петля волт→NotebookLM→подкаст (см. [[_NotebookLM-Audio]]).
- **Кандидат в рутину:** «еженедельный аудио-дайджест мозга» ([[evaluate-recurring-into-routine]]).

## Что НЕ делаем
НЕ строим двойника на NotebookLM; НЕ платим Enterprise, пока ручной ingest реально не станет узким местом; НЕ делаем `notebooklm-py` несущим.
