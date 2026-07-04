---
title: "Decision Memo — Архитектура синхронизации волта между машинами (Syncthing-звезда, поэтапно)"
type: decision
stage: distilled
origin: anton
authored_by: human
date_established: 2026-06-19
theme: machine-sync
status: proposed
audience: both
priority: should
confidence: 0.78
aliases: ["Решение — синхронизация волта между машинами", "Decision — vault sync architecture", "Syncthing star topology decision"]
tags: [decision, alpha-protocol, syncthing, vault-sync, multi-machine, infrastructure, machine-migration]
concept: "[[concept-tech-tools]]"
---

# 🅰️ Decision Memo — Синхронизация волта между машинами

> Итог Alpha Protocol (Recall + Deep Research ×2 + Synthesis) 2026-06-19. Протокол: [[protocol-alpha-protocol-recall-plus-deep-research]]. DR-источники: [[research-vault-sync-architecture-2026-06-19]] (DR1) + [[research-vault-sync-hub-authoritative-2026-06-19]] (DR2, цитируемый). Проект переезда: машинная память `machine-migration`. **Доп. исследование (multi-tenant / конкурентность, Cowork 2026-06-20):** [[research-multi-tenant-shared-vault-concurrency]] — подтверждает Syncthing-хребет + area-ownership. **Статус: ПРЕДЛОЖЕНО — старт A одобрен Антоном к проработке, Syncthing ещё НЕ разворачивали.**

## Проблема
Синхронизировать волт Антона (**168k файлов / 1.42 ГБ** активного `Anton-Knowledge` + 3.75 ГБ `_originals` + 10 ГБ `_imports`) между **всегда-вкл Windows-десктопом (хаб, 2 GPU)** и тремя ведомыми (Windows-ноут, Mac Studio, MacBook) — **без расхождения копий, без порчи живого `.git`, на минимальном бюджете**. Цель — бесшовная работа в Claude Code с любой машины (часть проекта `machine-migration`; конфиг+память уже едут через GitHub-репо `claude-config`, волт — отдельным транспортом). **Транспорт конфига/памяти финализирован отдельным консенсусом ноут↔хаб 2026-06-28 → `[[decision-config-memory-github-remote-2026-06-28]]` (приватный GitHub-remote, НЕ LAN git-daemon).**

## Что знаем (Recall)
- Облачный файл-синк (Google Drive/iCloud/OneDrive/Dropbox) **ломает `.git`** (desktop.ini, гонки, дубли) → отвергнут для всего с `.git`. Конфиг/память → git/GitHub. Волт → Syncthing/бандл, **НЕ git-в-облаке** (машинная память `vault-offsite-backup`; прежний DR-артефакт «Why Git Beats Cloud Sync»).
- Волт = **живой git-репо** (`vault_backup.py`) + оффсайт-бандл на Google Drive `a@` + `C:\ObsidianBackup` (ночью 03:00). Реиндекс/RAG/венв — на хабе (GPU).
- Заметка Антона про «file-count ад» (Obsidian давится на больших числах).

## Результаты Deep Research (полностью → [[research-vault-sync-architecture-2026-06-19]])
- Подтверждает: **Syncthing в строгой ЗВЕЗДЕ** вокруг всегда-вкл хаба (не mesh); `.git` **только на хабе**, ведомые получают голые `.md` (`.stignore: (?d).git/`); конфликты = `*.sync-conflict-*` (детерминированно, безопаснее тихого слияния).
- **Obsidian Sync отклонён:** 1.42 ГБ > Standard (1 ГБ) → платный Plus (~$96/год); и всё равно **не берёт** 3.75 ГБ originals (синкает только папку волта).
- **Тюнинг под 168k:** `maxFolderConcurrency=1`, `databaseTuning=large`, `GOMEMLIMIT`, `.stglobalignore` (#include), `caseSensitiveFS=false`, **Staggered Versioning на хабе** (защита от тихой 0-байт труначии), **Introducer OFF** (иначе звезда → mesh → conflict storm).
- **Scope-split:** волт 1.42 ГБ = Send&Receive все · originals 3.75 ГБ = Send-only хаб / Receive-only ведомые · scratch 10 ГБ = **НЕ синкать**.
- **Новая опция (фаза C):** тяжёлое (scratch, Claude Code) не синкать, а **удалёнка в хаб** (Tailscale + VS Code Remote-SSH / **SSH / RDP** — стандартное, цитируется в DR2; НЕ `claude remote-control`), ведомые = тонкие клиенты. Claude Code на хабе — в **WSL/Git Bash**.

**DR2 уточнения (2026-06-19, цитируемый отчёт [[research-vault-sync-hub-authoritative-2026-06-19]]):**
- **`git init --separate-git-dir`** — вынести `.git` ФИЗИЧЕСКИ наружу синканного дерева (остаётся `.git`-указатель). Усиление: убирает объект-базу git из зоны риска совсем (лучше, чем просто `.stignore`).
- **Снят открытый вопрос:** удалёнка = SSH / VS Code Remote SSH / RDP (реальное), не спец-команда.
- **NAS/SMB/сетевой диск Obsidian НЕ поддерживает** для живого волта → касается и капризного **USB**: живой волт — на внутренний SSD, не на внешний.
- **Resilio v3** бесплатно даёт placeholder-файлы — оправдано ТОЛЬКО для `_originals`-архива.

**Внешний диск F:\ (Samsung 870 EVO 2TB, NTFS, USB, 524 ГБ free) — роль (2026-06-19):** ХРАНИЛИЩЕ, не синк (один диск = не транспорт между машинами). Использовать для: (1) **физический seed переезда** — собрать весь чемодан на F: и донести на десктоп (быстрее сети, снимает блокер `gh login`); (2) **3-я копия бэкапа** (крепче 3-2-1); (3) **дом для тяжёлого/холодного** (большие медиа, зеркало `_originals`). НЕ для: живого волта (USB-сон) и НЕ замена Syncthing-синку. `_imports` НЕ переносить (426 скриптов прибиты к `E:\`).

## Варианты
- **(A) Только волт `Anton-Knowledge` через Syncthing-звезду.** ← старт
- **(B) A + `_originals` как Send-only с хаба.**
- **(C) B + Tailscale/Remote для тяжёлого** (полная схема DR).
- **(D) Платный Obsidian Sync — ОТКЛОНЁН** (дорого + не берёт originals + риск на 168k).

## Риски / что проверить
- ⚠️ Сложность полной схемы DR против AK-47 → поэтому стартуем с минимума.
- ✅ **СНЯТО DR2:** `claude remote-control` оказался выдумкой DR1 — удалёнка делается стандартным SSH/RDP (DR2, с цитатами).
- ⚠️ Остаётся проверить: актуальные цена/лимиты Obsidian Sync (всё равно отклонён). Совет **отключить `fsync`** (DR1) — **НЕ применяем**.
- ⚠️ Introducer/mesh → conflict storm. ⚠️ scratch заехал в волт → краш Obsidian.

## Рекомендация (AK-47, поэтапно)
**A сейчас → B → (C позже, если реально понадобится гонять тяжёлое с ведомых).** Не разворачивать всю махину сразу. A закрывает ~90% боли бесплатно; `.git` + бандл-бэкап + реиндекс остаются на хабе как сейчас.

## Action Plan — Минимальный старт (A)
> Делаем КОГДА десктоп станет хабом (Фаза 1 переезда). Пока — план, не исполнение.
1. **Хаб = десктоп** определён; волт `E:\Obsidian\Anton-Knowledge` живёт на нём (как сейчас на этой машине до переезда).
2. На хабе в корне волта: `.gitignore` дополнить `*.sync-conflict-*`, `.DS_Store`, `Thumbs.db`, `desktop.ini`, `.obsidian/workspace.json`; `.gitattributes` уже есть (`* text=auto`). Коммит (`vault_backup.py`).
   - **Усиление (DR2):** перед шарингом волта применить `git init --separate-git-dir <внешний путь на хабе>` — вынести `.git` физически наружу синканного дерева; в дереве остаётся `.git`-указатель, который Syncthing игнорирует. Так объект-база git вообще не в зоне синка.
3. Поставить **Syncthing** на хаб + каждое ведомое (есть веб-GUI — CLI Антон не трогает).
4. **Звезда:** на каждом ведомом добавить Device ID ХАБА. Ведомые между собой НЕ знакомить. **Introducer = OFF** на всех.
5. Шарить ТОЛЬКО папку `Anton-Knowledge` (1.42 ГБ) как **Send & Receive**. `_originals` и `_imports` пока НЕ шарить.
6. `.stignore` в корне: `(?d).git/` + `#include .stglobalignore`; `.stglobalignore` = `(?d).DS_Store`, `(?d)desktop.ini`, `(?d)Thumbs.db`, `(?d).AppleDouble/`.
7. На ХАБЕ: `databaseTuning=large`, `maxFolderConcurrency=1`, включить **Staggered File Versioning** на папке волта.
8. Проверка: правка .md на ведомом → появляется на хабе за секунды → ночной бандл-бэкап + реиндекс на хабе видят её. 0 `.sync-conflict` при работе по одной машине.

## ОБНОВЛЕНИЕ 2026-06-25 — урок «D2»: шина `_machine-bus` живёт в ШАРЕ ВОЛТА, НЕ отдельной вложенной папкой (origin: anton, «ДА»)
**Решение (закреплено после аварии):** кросс-машинная шина `_machine-bus` синкается как **часть основного шара волта `anton-knowledge`**, а **НЕ** как отдельный вложенный Syncthing-шар. Вложенный вариант («D2» = второй шар на `_machine-bus` внутри уже расшаренного волта + строка `_machine-bus` в `.stignore` родителя) — **отвергнут как хрупкий**.

**Почему (грабли, пойманные 2026-06-25):** `.stignore` в Syncthing **пер-машинный и НЕ синкается**. Достаточно, чтобы ОДНА машина не добавила ignore родителя ИЛИ не приняла вложенный шар — и для неё шина обрывается в ОБЕ стороны, пока все прочие папки показывают 100%. Хаб `A-2022BAYAREA` имел отдельный шар + ignore; ноут `ZBOOKG8-2023PAL` вложенный шар **не принял** И его волт `_machine-bus` не игнорировал → ноут замёрз на 24.06, хотя волт/скиллы/home были 100% (выглядело как «робот шины мёртв» — неверно, труба была перерезана). Mac Русланы и ноут Натальи шар **приняли** (100%) → у них была только поломка чтения, не синка.

**Откат («D2 rollback», как делать на каждой машине, что была на D2):**
1. BACKUP: `GET /rest/config` → файл; копию vault `.stignore`.
2. Удалить отдельный шар: `DELETE /rest/config/folders/machine-bus` (файлы на диске НЕ трогаются).
3. Убрать строку `_machine-bus` из vault `.stignore`.
4. `POST /rest/db/scan?folder=anton-knowledge` → шина едет через волт ко всем.
5. ⚠️ **При расignore возможны `.sync-conflict-*`** (независимые истории на пирах; здесь 4 шт., с меткой device-id самого хаба). Conflict-копия может содержать **уникальные** сообщения (3 письма ноута к хабу были ТОЛЬКО в ней) → **слить union блоков `## `** перед удалением conflict-файлов, не удалять вслепую. JSON-реестр (`machines.json`) — оставить больший/актуальный, не сливать.
6. Проверка: `/rest/db/completion?folder=anton-knowledge&device=<id>` = 100%/need=0.

**Цена/выгода:** шина теряет «свою быструю трубу», но едет через волт за ~секунды при здоровом синке (боль «после сна синк застревает» уже лечится сторожем `syncthing_watchdog.ps1` on-wake). Если позже реально понадобится отдельная быстрая труба — единственный безопасный способ = **вынести `_machine-bus` ВНЕ волта** (напр. `E:\_machine-bus`) + правка пути в `machine_bus.py` на всех машинах = БЕЗ вложенности (это путь B/decouple, отдельный проект, не дефолт). Машинный слой урока — память `deterministic-script-gotchas`.

## Следующие шаги
- Реализуем на **Фазе 1 переезда** (десктоп → хаб). До этого — конфиг+память через `claude-config` (ждёт `gh auth login`).
- Перед фазой C — проверить `claude remote-control` и поставить Tailscale.
- Кандидат на рутину: после старта A — `/vault-sync-healthcheck` (как backup-healthcheck) — оценить позже.

Связано: [[research-vault-sync-architecture-2026-06-19]] · [[protocol-alpha-protocol-recall-plus-deep-research]] · [[concept-tech-tools]] · машинная память `machine-migration`, `vault-offsite-backup`, `vault-data-architecture`.
