---
title: "Синтез DR26-08-04-ZB-08 — GEO-аналитика по origin-логам: зрелого OSS нет, дом инструмента = свой репо"
aliases: ["GEO log analytics OSS", "куда подавать geo_measure", "AI crawler log analyzer", "DR GEO логи 04.08"]
date: 2026-08-04
type: insight
stage: distilled
origin: mixed
authored_by: ai
ai_author: Claude Opus 5 (сбор + синтез) поверх ChatGPT Deep Research + Grok (локальный CLI)
vendors_base: 2 из 5 (chatgpt 24.5KB/32 уник.URL · grok 24.9KB/21 уник.URL); gemini/claudeai/GLM не запускались — у безлюдного тика нет браузерной двери, дропом НЕ помечены
machine: MAC-1
operator: robot
runner: AUTO-mac16-260731-dr-runner-daily
dr_id: DR26-08-04-ZB-08-1744
consumer: "решение: публиковать ли geo_measure.py как OSS и в какой репозиторий подавать PR"
tags: [insight, deep-research, geo, oss, github, logs, gdpr, issue-matching]
related: "[[maintainer-first-issue-matching-measured]] · [[cold-pr-into-silent-queue]] · [[github-guerrilla-presence-over-merge]] · [[prior-art-gate-before-building]] · [[prichina-kak-claim]] · [[fake-it-courage-not-fake-numbers]] · [[multi-vendor-cookbook-radar]]"
---

# ZB-08: GEO по origin-логам — что есть, что не доказано, куда нести

> **Потребитель решения:** публиковать ли `geo_measure.py` как OSS и в какой репозиторий подавать PR.
> **Тела:** `_originals/deep-research/DR26-08-04-ZB-08-1744-geo-origin-logs-oss-tool-{chatgpt,grok}.md`.
> **База:** 2 рельсы из 5. Gemini/claude.ai/GLM не опрашивались — это НЕ «рельса мертва», это «дверь не открывали».

## 0. Вывод одной строкой

Зрелого открытого «AI-crawler log → GEO» анализатора **нет** (обе рельсы, confidence high): три молодых
репо с 0–4★ и одиночным мейнтейнером. Значит дом нашего инструмента — **свой репозиторий под MIT/Apache**,
а PR в чужое = только **списки/реестры**, не код.

## 1. Консенсус 2/2

| Что | Обе рельсы |
|---|---|
| Зрелого OSS нет | Logwick (AGPL, 4★, 1 коммит/1 мейнтейнер) · SpeyTech/crawler-log-analyser (AGPL, 0★, feature-freeze до 31.08.2026) · seo-log-analyzer-tools (лицензия NOASSERTION, 0★) |
| JS-аналитика слепа | AI-краулеры не исполняют JS → единственный origin-сигнал = server/edge logs |
| Стадии разные | краул ≠ индекс ≠ цитата; тип бота критичен (ClaudeBot ≠ Claude-SearchBot ≠ Claude-User) |
| Чем платят SaaS | не «точнее grep», а панель промптов + сохранённые citation-URL + share of voice + managed bot/IP intelligence |
| GDPR | IP в логах = ПДн (C‑582/14 Breyer). Сырые логи в git ⛔, фикстуры синтетические, анонимизация IP по умолчанию |
| Строгий критерий живости | ⛔ НЕ выполнен ни одним кандидатом: репо, где смержили PR постороннего после 06.05.2026, не найдено |

## 2. ⛔ Опровергнутое допущение (главная ценность отчёта)

«Чаще возвращается краулер → чаще процитирует» — **folk hypothesis**, не метрика. Обе рельсы искали и
не нашли ни одного воспроизводимого исследования, где origin-логи нескольких сайтов джойнятся с
цитатами по фиксированной панели промптов при контроле rank/backlinks/freshness.

Что реально предсказывает цитирование (emerging): попадание в retrieval candidate set · rank/authority
в используемом индексе · соответствие интенту промпта · **user-triggered fetch этого URL прямо перед ответом**.
Что спекулятивно: общий crawl count · малый recrawl interval · `llms.txt` сам по себе · schema сама по себе.

⇒ Позиционирование нашего инструмента честное: **Stage-1 observability** («кто вернулся на какой URL»),
а НЕ «предсказание цитирования». Заявлять второе = фейк данных (§5.4).

## 3. ⚠️ Ошибка классификатора, которую отчёт нашёл

`Google-Extended` **не имеет собственного HTTP User-Agent** — это только управляющий токен `robots.txt`
([Google, июль 2026](https://developers.google.com/crawling/docs/crawlers-fetchers/google-common-crawlers)).
Строка `Google-Extended` в access.log = самодекларация стороннего клиента либо баг классификатора,
но не подтверждённый краулер Google.

Наш `geo_measure.py` живёт НЕ на Mac16 (`find ~` пусто) — проверку и правку делает узел-владелец.
Задача заведена ниже, не «сделано».

## 4. Расхождение рельс

| Вопрос | ChatGPT | Grok |
|---|---|---|
| Куда нести код | proposal-issue в `Auriti-Labs/geo-optimizer-skill` (живой до 17.07.2026) + AI-UA-реестр в GoAccess (issue #2935 Applebot) | свой репо; в чужое только `ai.robots.txt` (реестр ботов) и awesome-списки |
| Ближайший аналог | Logwick — «наиболее точное функциональное совпадение» | Logwick вход = JSONL, а не raw nginx combined → для nginx ближе SpeyTech |

Расхождение не снято: обе двери (GEO Optimizer / GoAccess) названы БЕЗ подтверждённого merged-PR
постороннего — то есть это кандидаты, а не проверенные адресаты. По [[cold-pr-into-silent-queue]]
холодный PR в такую очередь не шлём.

## 5. Решение (принято раннером как кофаундером, Tier-0/1, обратимо)

1. **Дом = свой репозиторий**, лицензия MIT/Apache-2.0 (шире adoption, чем AGPL у обоих аналогов).
   Позиционирование — Stage-1 GEO observability, явная строка «это не citation-метрика».
2. **PR в чужое = только списки** после первого релиза: `ai.robots.txt` (upstream taxonomy — там реально
   мержат) → awesome-GEO / awesome-generative-engine-optimization. Код в чужие репо не несём.
3. **Перед любой стройкой — [[prior-art-gate-before-building]]**: Logwick / SpeyTech / seo-log-analyzer-tools
   прочитать глазами; если Stage-1 уже закрыт — вносим в них, а не пишем четвёртый.
4. **Приватность зашита с рождения**: input local-only, `--anonymize-ip` по умолчанию в демо, срез query-string,
   фикстуры синтетические, сырые логи в git ⛔.
5. **Не заявлять recrawl→citation** ни в README, ни в постах.

## 6. Осталось (не сделано этим тиком)

- [ ] Проверить и починить классификатор `Google-Extended` в `geo_measure.py` — на узле-владельце (не Mac16).
- [ ] Prior-art чтение трёх аналогов до первой строки кода.
- [ ] Добрать рельсы gemini/claude.ai/GLM по этой теме, если решение окажется дорогим (сейчас 2 из 5).
