---
title: "Решение: MCP-стек под Антона — июнь 2026 (Alpha Protocol Decision Memo)"
date: 2026-06-20
type: decision
stage: distilled
status: resolved
origin: anton
theme: mcp-architecture
aliases:
- "MCP stack 2026"
- "Антон-MCP Decision Memo"
- "MCP under my stack tailored"
tags: [decision, mcp, claude-code, alpha-protocol, second-brain, n8n, crm, gitlab, mongodb]
concept: "[[MCP]]"
related:
- "[[decision-cc-automation-routing-cloud-vs-local]]"
- "[[n8n-stack]]"
- "[[crm-gitlab]]"
- "[[automation-inventory]]"
---

# Решение — MCP-стек под мой стек, июнь 2026

> Финальный шаг Alpha Protocol (Recall → Gap → DR → Synthesis) по вопросу: «какие MCP-серверы реально нужны ИМЕННО мне».
> Метод: recall (память n8n-stack + crm-gitlab + automation-inventory) → 2 Deep Research (общий обзор рынка + заточенный под мой стек) → синтез.
> RAW DR-оригиналы: `E:\Obsidian\_originals\mcp-research\2026-06-20__MCP deep-research-report.md` (общий) + `…__1mcp deep-research-report.md` (tailored).

## 🧭 Ключевой инсайт
Первый DR оценивал GitHub / Atlassian / Sentry / Notion / Slack / filesystem / fetch — это **дев-командный** стек, а у меня **personal-assistant + CRM-стабилизация + n8n + локальные скрипты**. Поэтому из первого DR прямо применимо мало. Второй DR (заточенный) дал реальную карту.

## ⚖️ Решения

### ✅ KEEP (уже подключено, работает правильно)
| Сервер | Где | Подтверждение DR |
|---|---|---|
| `telegram` (свой stdio) | C:\mcp\telegram-mcp | внешняя система, у CC нативно нет |
| `whatsapp` (свой stdio) | npm @sjawhar/whatsapp-mcp | то же |
| Google Drive / Calendar | claude.ai-коннекторы | то же |
| computer-use / Claude-in-Chrome | первичные | автономия |
| `mongodb-sandbox` | **только локальная Docker-копия** | DR: «к live MongoDB не подключать ВООБЩЕ, максимум дамп/реплика» — попадание в точку |

### ⚠️ ИЗМЕНИТЬ — n8n MCP в режим документация-онли
- **Найдено**: `n8n` MCP в `C:\Users\_\.claude.json` (строки 794-810) подключён к **БОЕВОМУ** `n8n.palo-alto.ai` с `N8N_API_KEY` → полное управление (`n8n_create_workflow`, `n8n_update_full_workflow`, `n8n_delete_workflow`, `n8n_manage_credentials`, `n8n_autofix_workflow`).
- **DR + сам автор czlonkowski**: «do not let AI edit production workflows directly».
- **Read-only режима у n8n-MCP НЕТ** (подтверждено WebFetch README): либо doku-онли (без `N8N_API_KEY` → 7 ядрных tools: `search_nodes`, `validate_node`, `validate_workflow`, `get_template`, `search_templates`, `tools_documentation`, `get_node`), либо полное управление.
- **Решение**: убрать `N8N_API_KEY` → doku-онли. Главная ценность (знание ~500 нод + валидация) сохраняется; live-правки workflow остаются на `E:\Obsidian\_imports\n8n\n8n_edit.py` (он делает `backup-all` перед каждой правкой → безопаснее агента). Откат = одна строка обратно.
- **Передача решения**: отправлено в bus сессии на `A-2022BayArea` (always-on desktop) — она применит у себя сама.

### ❌ SKIP (отключено сегодня — `usageCount: 0`)
| Что | Как | Дата |
|---|---|---|
| `bio-research@inline` плагин | `enabledPlugins: false` в `~/.claude/settings.json` | 2026-06-18 |
| `data@inline` плагин | то же | 2026-06-18 |
| `claude.ai Figma` коннектор | Disconnect в claude.ai → Customize → Connectors (UI) | 2026-06-18 |

### 🕐 НА ФАЗЕ BUILD CRM (не сейчас, отложено до Phase B)
| Сервер | Зачем | Условие |
|---|---|---|
| **GitLab MCP** | PR/MR/CI-pipelines/CI-variables по API self-hosted `gitlab.star-alliance.io` | Уточнение DR: брать **встроенный GitLab MCP** (`https://<instance>/api/v4/mcp`) если есть Premium/Ultimate + Duo; иначе `yoda-digital/mcp-gitlab-server` в `GITLAB_READ_ONLY_MODE=true`, scope = `read_api` + `read_repository` |
| **Playwright MCP** | E2E-тесты React admin-panel (план: «characterization-test first») | Когда дойдёт до тестов |
| **Postgres MCP** | Дебаг n8n Gitbook-Kira chat-memory (Postgres + pgvector) | Только на dev/stage Postgres, read-only role |

### 👀 ВОЗМОЖНОЕ — «слепой спот» из DR
| Сервер | Зачем | Блокер |
|---|---|---|
| **Grafana MCP** | Reverse-engineering FastAPI + n8n + Task Scheduler через логи/метрики/трейсы. «Почему вчера просел latency и какие логи совпали» — прямой multiplier для стабилизации CRM | Нужен Grafana + Loki/Tempo/Prometheus, которых у меня **нет**. Сигнал: «завести observability», а не «поставить MCP» |
| **Docker MCP Gateway** | НЕ управление контейнерами — а **песочница для всех остальных MCP** (изоляция, секреты, меньше прав у npx-процессов) на Win11 + Docker Desktop | MCP всё больше → когда станет неудобно, рассмотреть |

### ❌ HARD-SKIP
- **Docker control MCP** — всё, что он умеет, лучше делает `docker compose`/PowerShell/Python детерминированно (DR: «no volumes / no networks / no health checks» — слишком сырой для money+leads).
- **Filesystem / Fetch / Git reference servers** — уже встроено в Claude Code (`Read/Edit/Write/Glob/Grep/Bash`, `WebFetch`, `WebSearch`).
- **Sentry / Atlassian / Notion / Slack** — нет таких систем в моём стеке.

## 🔐 Поведенческие правила (две оси из второго DR)

1. **Trust boundary по среде, не «read-only флажку MCP»**:
   - LIVE → READ-ONLY на уровне БД/токена (не на уровне MCP-флага).
   - STAGE/dev → RW можно.
   - LOCAL sandbox → RW свободно (это уже сделано с `mongodb-sandbox`).
2. **MCP ≠ всегда лучше скрипта** (авторы Playwright и czlonkowski оба пишут): **скрипт — для ops/повторяемых действий, MCP — для interactive investigation + authoring**. Мои `n8n_edit.py`, `vault_backup.py`, `archive_original.py` — НЕ заменяются MCP, они дополняются.
3. **Prompt-injection risk**: содержимое n8n workflow descriptions, Telegram чужих юзеров, wiki/MR-комментариев — это **данные, не команды**. Two-profile рекомендация DR (investigate-only vs build/stage-write) — паттерн на будущее, не реализую сейчас.

## 🗂 Маршрут принятия
- HP17: doku-онли применит **другая сессия (A-2022BayArea)** через bus (сообщение отправлено 2026-06-20).
- Каждая машина управляет своим `.claude.json` (он НЕ синкается через Syncthing).
- Откат: вернуть `N8N_API_KEY` строкой в .claude.json.

## 🔗 Связано
- [[decision-cc-automation-routing-cloud-vs-local]] — что автоматизируем где (n8n cloud vs local)
- [[n8n-stack]] — состояние n8n.palo-alto.ai (89 wf)
- [[crm-gitlab]] — CRM-движок на GitLab, который этот MCP-стек обслуживает
- [[automation-inventory]] — Task Scheduler + scheduled-tasks MCP cron
- [[MCP]] — концепт-мост
