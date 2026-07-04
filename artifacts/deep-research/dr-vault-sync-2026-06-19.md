---
title: "Deep Research — Optimal File-Synchronization Architecture for a Massive Obsidian Vault (168k files)"
type: research
stage: distilled
origin: external
authored_by: external-deep-research-tool
commissioned_by: anton
source: external-deep-research
date_added: 2026-06-19
theme: machine-sync
volatility: volatile
interpretation_confidence: medium
status: reference
aliases: ["DR — синхронизация волта между машинами", "Vault sync architecture research 2026", "Syncthing vs Obsidian Sync DR"]
tags: [deep-research, alpha-protocol, syncthing, obsidian-sync, multi-machine, vault-sync, infrastructure, tailscale]
concept: "[[concept-tech-tools]]"
layer: essence
---

# Deep Research — Optimal File-Sync Architecture for a Massive Obsidian Vault (168k files)

> **Источник: ВНЕШНИЙ Deep Research, заказанный Антоном (origin: external, НЕ Anton-original).** Принесён 2026-06-19 как шаг Deep Research в Alpha Protocol по теме синхронизации волта между машинами. Сохранён дословно. Синтез и решение → [[decision-vault-sync-architecture]]. Протокол: [[protocol-alpha-protocol-recall-plus-deep-research]]. Проект переезда: машинная память `machine-migration`.

> [!warning] Проверять, не глотать на слово
> Раздел **H. Sources у отчёта ПУСТОЙ** (нет реальных ссылок) → специфику трактуем как «claims to verify», не как факт. Конкретно перед использованием проверить: (1) существование команды **`claude remote-control`**; (2) актуальные **цены/лимиты Obsidian Sync** (Standard 1 ГБ / Plus 10 ГБ / ~$96/год — правдоподобно, но volatile); (3) совет **отключить `fsync`** — НЕ применяем (жертва защиты данных ради скорости, против AK-47).

<!-- curation:start -->
## 🔗 Связанные
[[concept-tech-tools]] · [[decision-vault-sync-architecture]] · [[protocol-alpha-protocol-recall-plus-deep-research]]

Карта: хаб-и-спицы (always-on Windows-десктоп = хаб; ноут + 2 Mac = ведомые). Вывод DR: **Syncthing в строгой звезде** для текстового волта, **`.git` только на хабе**, тяжёлое (10 ГБ scratch + Claude Code) — **не синкать, а удалёнка в хаб** (Tailscale + VS Code Remote-SSH). Obsidian Sync отклонён (дорого + не берёт originals). См. синтез в решении.
<!-- curation:end -->

---

## A. Executive Summary
Среда: асимметричный распределённый парк — основной всегда-включённый Windows-десктоп с двумя GPU (хаб), вокруг ведомые: Windows-ноут, Mac Studio, MacBook. Payload: активный волт Obsidian 1.42 ГБ в ~168 000 .md файлов; рядом 13.75 ГБ внешних данных (3.75 ГБ статический архив + 10 ГБ волатильный scratch для AI-контекста и скриптов). Живой in-vault git + Claude Code требуют атомарности, нулевого расхождения и отсутствия конфликтов.

Облачные провайдеры и простые p2p-приложения проваливаются под этими ограничениями. Синк живого git через асинхронную файловую репликацию надёжно ведёт к порче индекса. Масштаб 168k файлов раздувает латентность file-watcher'ов → разряд батарей, троттлинг, высокая нагрузка CPU. Obsidian Sync оптимизирован под текст, но 1.42 ГБ превышает стандартный коммерческий тариф и не годится для 13.75 ГБ периферии.

Оптимум: **отказ от full-mesh, строгая звезда вокруг всегда-вкл Windows-хаба.** Syncthing — лучший локальный слой синка для активного .md-волта и статических архивов, при сильном тюнинге и агрессивных глобальных ignore. Для кода/регенерации/Claude Code — уход от файловой репликации: **VS Code Remote-SSH + Claude Code Remote Control поверх шифрованного overlay (Tailscale)**, исполнение волатильного — на хабе. Бифуркация: неструктурированный текст (локальный офлайн) ↔ волатильная dev-среда (централизованное удалённое исполнение).

## B. Key Findings
- **Масштаб коммерческих платформ.** 168k файлов / 1.42 ГБ превышает Obsidian Sync Standard (лимит 1 ГБ) → нужен Plus (10 ГБ). Obsidian Sync надёжен у волтов ~100k файлов (индексируемый БД-бэкенд), но его diff-match-patch — только для .md, бесполезен для 13.75 ГБ архивов/бинарей/scratch.
- **Overhead p2p.** Syncthing тянет 168k легко (прод-кейсы до 30M файлов / 15 ТБ), но на дефолтах при таком масштабе — скачки памяти и постоянный CPU-polling при хешировании. Стабильность требует ручного тюнинга Go GC, параметров watcher'а и буферов индекса БД.
- **Git ⨯ непрерывный синк несовместимы.** Git требует консистентности loose objects / packed / index locks; синк реплицирует файлы поштучно, асинхронно, без транзакционных границ. Перенос во время записи в `.git/objects` → почти гарантированная порча. **Метаданные версионирования НЕ должны пересекать границу синка.**
- **Топология.** Full-mesh экспоненциально растит конфликты и фрагментацию БД при частых online/offline. Единственный авторитетный хаб = арбитраж конфликтов в одной высокодоступной точке, меньше коллизий, нет split-brain.
- **Кросс-платформенный мусор метаданных.** NTFS↔APFS генерят `.DS_Store`, `desktop.ini`, `Thumbs.db` → загрязняют волт, лишние хеши, иногда permission-ошибки, останавливающие демон. Нужны глобальные ignore.
- **Удалённое исполнение > локальная репликация для волатильного.** 10 ГБ scratch и Claude Code не требуют локальной реплики на ведомых. VS Code Remote-SSH + Claude Code Remote Control → ведомые исполняют на dual-GPU хабе с латентностью <100 мс, минус 10 ГБ сетевого оверхеда и расхождение зависимостей.

| Платформа | Макс. протест. масштаб | Цена 2026/год | Протокол | Пригодность для 168k AI-workflow |
|---|---|---|---|---|
| Obsidian Sync | ~100k файлов | $96 (Plus) | diff-match-patch (CRDT-like) | Отклонён: не берёт внешние данные; дорого |
| Resilio Sync | 450M+ файлов | $39.99 (вечная) | BitTorrent-chunking | Отклонён: проприетарно; фичи в enterprise |
| Syncthing | 30M+ файлов | $0 (OSS) | Block Exchange Protocol | **Принят**: тюнингуем; selective ignores; бесплатно |
| Unison | ~1M файлов | $0 (OSS) | SSH bidirectional | Отклонён: нет непрерывного watch; ручной триггер |

## C. Contradictions & Debates
- **File-level replication vs CRDT.** Obsidian/diff-match-patch сливает одновременные правки без дублей (frictionless), но при дрейфе системных часов (NTP) может тихо разрушить семантику. Syncthing на блок-хешах при одновременной правке НЕ мёржит, а создаёт `.sync-conflict` (timestamp+device). На масштабе 168k **детерминированный конфликт безопаснее алгоритмического слияния** — для базы-контекста AI тихая порча катастрофична.
- **Case sensitivity NTFS/APFS.** Оба case-preserving + case-insensitive по умолчанию. Форматировать тома в case-sensitive (как Linux) ломает нативные приложения → не делать. `caseSensitiveFS` в Syncthing держать `false` на всех (включает защиту от бесконечных collision-loop на смешанном регистре).
- **Git «bare repo» workaround.** Синкать bare `.git` файл-синком — **фундаментально опасно** (быстрая мутация blob'ов → перенос частичного состояния → порча на приёмнике). Консенсус: VCS управляет собой нативно, синк — только рабочие файлы.
- **Hub-and-spoke vs full mesh.** Для одного юзера с всегда-вкл хабом mesh вреден: ноут после сна синкается с MacBook раньше хаба → split-brain → каскад конфликтов. Строгая звезда жертвует p2p-избыточностью ради линейного предсказуемого свода.

## D. Best Practices
**Тюнинг под масштаб (на Windows-хабе):**
- `maxFolderConcurrency = 1` — без параллельных сканов (защита очереди IOPS SSD, ниже пик памяти).
- `databaseTuning = large` — крупные буферы индекса под 168k метаданных.
- `GOMEMLIMIT` — порог Go GC, чтобы не жечь CPU при длинных сканах.
- `fsync()` отключить на хабе для скорости — **⚠️ жертва безопасности; брать только при UPS** (в нашем синтезе НЕ берём).

**Защита живого git:** авторитетный `.git` ТОЛЬКО на хабе; Syncthing раздаёт лишь рабочее дерево (.md) на ведомые. `.stignore` в корне: `(?d).git/` (префикс `(?d)` позволяет удалять родительские папки, без orphan'ов). Правка на Mac → watcher → push на хаб → запланированная задача на хабе коммитит в git. Ведомые не знают про git.

**Анти-конфликт при одном писателе:** хаб = Send & Receive; ведомые = Send & Receive, но общаются ТОЛЬКО с хабом. Коллизия → `.sync-conflict`. В `.gitignore` хаба — `*.sync-conflict-*`, чтобы конфликт-файлы не попадали в историю.

**Scoping поверхности синка:**

| Логический блок | Размер | Контент | Стратегия |
|---|---|---|---|
| Live-Sync Vault | 1.42 ГБ | активные .md | Send & Receive на всех; sub-second latency, офлайн |
| Sacred Archive | 3.75 ГБ | статические референсы | Send Only с хаба; Receive Only на ведомых (иммутабельно) |
| Regenerable Scratch | 10 ГБ | RAG-индексы, скрипты | **НЕ синкать**; только через SSH/remote |

**Кросс-ОС:** `.gitattributes` с `* text=auto` (нормализация CRLF/LF на хабе). Глобальный `.stglobalignore` с `(?d).DS_Store`, `(?d).AppleDouble/`, `(?d)desktop.ini`, `(?d)Thumbs.db`, подключённый через `#include .stglobalignore` в каждом `.stignore`.

## E. Failure Modes
| Режим | Причина | Митигация |
|---|---|---|
| File-Watcher Exhaustion | 168k > лимита inotify/fsevents → тихий фолбэк на полные сканы → CPU/IO/батарея | Поднять kernel-лимиты; сузить scope активных watch |
| Silent Truncation (0-byte bitrot) | Задержки кеша ФС (exFAT/фрагмент. NTFS) → файл читается как 0 байт и реплицируется | **Staggered File Versioning на хабе** |
| Conflict Storms | Mesh / фича **Introducer** превращает звезду в mesh → split-brain → каскад `.sync-conflict` | Строгая звезда; **Introducer OFF** |
| Obsidian Indexing Collapse | 10 ГБ scratch/venv/бинари заехали в волт → Obsidian парсит → краш/троттлинг/загрузка >15 мин | Физическая сегрегация волта от scratch |

## F. Strategic Recommendations
Дисциплинированный гибрид: **тюнингованный Syncthing (строгая звезда) для текста** + **VS Code Remote-SSH и Claude Code Remote Control для волатильных вычислений**.

Obsidian Sync отклонён: 1.42 ГБ → платный Plus $96/год; протокол песочит только файлы внутри волта, а 3.75 ГБ «sacred originals» должны раздаваться надёжно, но физически вне волта (иначе indexing collapse). Syncthing берёт бесконечно внешних папок, сложные ignore, локальный staggered versioning, $0, и использует уже имеющийся always-on хаб → локальная сеть быстрее облака.

Resilio отклонён: ключевые фичи (selective sync, права) за платной лицензией ($39.99 вечная и дороже). Файловая репликация 10 ГБ scratch — анти-паттерн: через шифрованный SSH поверх mesh-VPN (Tailscale) ведомые = тонкие клиенты.

VS Code Remote-SSH + `claude remote-control` исполняют тяжёлое на dual-GPU хабе; ведомые получают доступ к локальному LLM-инференсу, большим контекстам, скриптам — без переноса волатильных файлов. `.git` — иммутабельно только на хабе; `* text=auto` нормализует переносы строк между Mac/Win.

## G. Action Plan
**Фаза 1 — Хаб + сеть.** Tailscale на хаб + ноут + оба Mac (шифрованная p2p-подсеть, обход NAT). В корне волта на хабе: `.gitattributes` (`* text=auto`) и `.gitignore` (`.obsidian/workspace.json`, `*.sync-conflict-*`, `.DS_Store`, `Thumbs.db`, `desktop.ini`); предв. коммит; проверить бэкап-скрипт.

**Фаза 2 — Syncthing + звезда.** Поставить на хаб и все ведомые. Сразу звезда: на каждом ведомом добавить Device ID хаба; **ведомые не знакомить друг с другом; Introducer OFF.** Тюнинг ТОЛЬКО на хабе: `maxFolderConcurrency=1`, `databaseTuning=large`.

**Фаза 3 — Папки + исключения.** На хабе `.stglobalignore` в корнях волта и архива: `(?d).git/`, `(?d).DS_Store`, `(?d)Thumbs.db` и т.д.; в каждой папке Syncthing — `#include .stglobalignore`. Роли: волт 1.42 ГБ = Send & Receive (+ Staggered Versioning на хабе); архив 3.75 ГБ = Send Only (хаб) / Receive Only (ведомые); 10 ГБ scratch — исключить полностью.

**Фаза 4 — Удалённая разработка.** На ведомых — VS Code + Remote-SSH на статический Tailscale-IP хаба. Для AI-сессии с тяжёлым scratch/волтом — `claude remote-control` на хабе, URL-сессии открывать с любого ведомого; всё исполняется на dual-GPU хабе, состояние сохраняется без переноса волатильных файлов.

## H. Sources
(В исходном отчёте раздел пуст — ссылки заявлены инлайн `[cite: source_id]`, но фактических URL нет. → все специфики верифицировать перед применением.)
