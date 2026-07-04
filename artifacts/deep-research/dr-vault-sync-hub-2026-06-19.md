---
title: "Deep Research #2 — Hub-Authoritative Architecture for a Massive Obsidian Vault (cited)"
type: research
stage: distilled
origin: external
authored_by: external-deep-research-tool
commissioned_by: anton
source: external-deep-research
date_added: 2026-06-19
theme: machine-sync
volatility: volatile
interpretation_confidence: high
status: reference
aliases: ["DR2 — hub-authoritative синк волта", "Vault sync DR #2 cited", "separate-git-dir vault sync"]
tags: [deep-research, alpha-protocol, syncthing, obsidian-sync, resilio, multi-machine, vault-sync, infrastructure, ssh, rdp, separate-git-dir]
concept: "[[concept-tech-tools]]"
layer: essence
---

# Deep Research #2 — Hub-Authoritative Architecture (cited)

> **ВНЕШНИЙ Deep Research, заказанный Антоном (origin: external).** Принесён 2026-06-19, ВТОРОЙ по той же теме. В отличие от [[research-vault-sync-architecture-2026-06-19]] — **с реальными ссылками** (turn-цитаты сохранены дословно). Синтез → [[decision-vault-sync-architecture]]. Протокол: [[protocol-alpha-protocol-recall-plus-deep-research]].

> [!note] Что DR2 ДОБАВИЛ к DR1 (главное)
> 1. **`git init --separate-git-dir`** — вынести `.git` ФИЗИЧЕСКИ наружу синканного дерева (остаётся файл-указатель `.git`). Чище, чем просто `.stignore (?d).git/` — убирает объект-базу git из зоны риска совсем.
> 2. **Снимает открытый вопрос DR1:** удалёнка в хаб = стандартные **SSH / VS Code Remote SSH / RDP** (реальные, цитируемые), НЕ загадочная `claude remote-control`. Claude Code на Windows — через **WSL/Git Bash**.
> 3. **Resilio v3** раздаёт бывшие платные фичи (Selective Sync placeholders) бесплатно — релевантно ТОЛЬКО для `_originals` (плейсхолдеры архива без скачивания байтов).
> 4. **NAS/SMB/сетевой диск — Obsidian НЕ поддерживает** для живого волта (нужна полная семантика ФС + мониторинг). Касается и капризного USB → живой волт на внутренний SSD.

<!-- curation:start -->
## 🔗 Связанные
[[concept-tech-tools]] · [[decision-vault-sync-architecture]] · [[research-vault-sync-architecture-2026-06-19]] · [[protocol-alpha-protocol-recall-plus-deep-research]]
<!-- curation:end -->

---

## Bottom line
Лучшая архитектура — **не** «выбрать лучший sync-app и пустить 4 пишущих реплики драться». На таком масштабе и с **живым git внутри волта** единственный дизайн, честно дающий **нет расхождения** и **нет порчи `.git`** — **hub-authoritative**: канонная рабочая копия на всегда-вкл Windows-десктопе, Claude Code там же, доступ с ноута и Mac удалённо через **SSH / VS Code Remote SSH** или **RDP**. OpenSSH Server есть в Windows 10/11; VS Code Remote SSH открывает папку на удалённой машине без локальной копии кода; Claude Code на Windows рекомендует WSL/Git Bash.

Если нужны **офлайн-копии** — лучший бесплатный слой репликации рабочего дерева волта — **Syncthing**, но только после того как **перестать синкать git-метаданные**, и желательно **вынести их из волта** через `--separate-git-dir` (остаётся `.git`-указатель; скрытый `.git` автоисключается Obsidian Sync и явно игнорируется Syncthing через `.stignore`).

**Obsidian Sync — не основа** этой архитектуры: e2e-шифрование, история, авто-исключение `.git`, per-device selective — но синкает только пока Obsidian запущен, имеет лимиты размера файла/хранилища, и построен вокруг СОДЕРЖИМОГО волта, не внешних `_originals`/`_imports`. На 168k файлов + внешние архивы — плохой матч, кроме узкого «notes-only» подмножества.

**Resilio Sync — сильнейшая альтернатива** Syncthing, ЕСЛИ нужны **desktop placeholder / on-demand** файлы для `_originals`. v3 открыл бывший Pro (Selective Sync placeholders) в бесплатном продукте. Но это ещё один движок со своей моделью конфликтов и кросс-ОС-причудами.

## What changes at your scale
Проблема — **число файлов**, не байты. Sync-движки и note-приложения платят за обход каталогов, stat, детект изменений, хеширование, очереди, поддержку БД, обработку конфликтов **на каждый файл**. У Obsidian боль больших волтов начинается уже ~40k заметок (iPhone) / ~64k файлов (старт Mac); у Syncthing кейсы — сотни тысяч … десятки миллионов файлов. Т.е. 168k для sync-движка «серьёзно, но реально с тюнингом», а для note-приложения — край.

**Конфликты = архитектурная проблема, не ошибка юзера.** Syncthing решает одновременные правки созданием `.sync-conflict-*` и распространяет их как обычные файлы; Resilio так же `.Conflict`. Они **сохраняют данные**, но не дают единый авторитетный семантический результат при правке в двух живых репликах. «Нет расхождения» = сократить число пишущих живых копий.

**Кросс-ОС имена важнее.** Syncthing держит case-safety включённым на case-insensitive ФС (Windows/большинство macOS) против коллизий по регистру; Git: `core.ignoreCase`, `core.precomposeUnicode` (macOS-декомпозиция). На 168k одно плохое имя — уже не угол.

## How the contenders behave (ранжир под задачу)
| Опция | Фит | Сильное | Что ломает | Вердикт |
|---|---|---|---|---|
| **Remote в хаб (SSH/Remote SSH/RDP)** | **Лучшее** | Одна авторитетная копия → нет расхождения; OpenSSH встроен; RDP с mac/iOS/win | Нужна сеть; RDP-хост = Windows Pro; наружу только через VPN | **Главная рекомендация** |
| **Syncthing** | Очень хорош как офлайн-зеркало, не как primary multi-writer | Бесплатно, кросс-ОС, режимы sendreceive/sendonly/receiveonly, watcher, REST, версии | `.sync-conflict-*`; синк `.git` опасен; много мелких файлов = память/скан; v2→SQLite миграция долгая | **Лучший бесплатный слой реплик** |
| **Resilio Sync** | Сильная для архив/on-demand | P2P, block-level, Selective Sync placeholders, v3 бесплатно | Закрытый вендор; case/Unicode/path конфликты; ignore case-sensitive | **Только если плейсхолдеры реально нужны** |
| **Obsidian Sync** | Слаб для всей 16 ГБ архитектуры | Безопасно исключает `.git`; история; шифрование | Только пока Obsidian открыт; Standard 1 ГБ/5 МБ, Plus 10 ГБ/200 МБ; storage включает историю; только содержимое волта | **Лишь для notes-only подмножества** |
| **Unison** | Технически может, операционно неуклюж | Кросс-ОС, two-way, SSH, watch | Парный; 4 машины = хореография или hub-spoke; macOS `-repeat watch` не из коробки | **Не низкофрикционный ответ** |
| **git-annex** | Неверная абстракция | Большие бинари с Git-like трекингом локаций | Pointer/symlink семантика, сложность; решает «большие файлы в git», не «живое редактирование 168k .md» | **Только если `_originals` станет архивным annex** |
| **NAS/SMB живой волт** | Плохо как primary | Централизованно, дёшево | Obsidian ЯВНО не поддерживает сетевые диски (нужна полная ФС + мониторинг) | **Не использовать для живого волта** |
| **Devcontainer** | Доп., не синк | Стандартизует среду | Сам не решает расхождение/`.git` | **Только поверх hub-модели** |

Тренды: **Syncthing v2** сменил LevelDB→SQLite (миграция = событие на больших данных); **Resilio personal v3** открыл премиум бесплатно.

## Recommended architecture — two lanes
**Primary lane = remote-authoritative.** Всегда-вкл Windows-десктоп = единственный живой хост Claude Code + канонная копия рабочего дерева. Claude Code — лучше в **WSL** (Anthropic поддерживает Win через WSL/Git Bash, лучше всего в Bash/Zsh/Fish). С Mac/ноута — SSH (CLI), VS Code Remote SSH (редактор), RDP (полный десктоп). Единственная архитектура, честно обещающая **нет расхождения**: одна пишущая правда.

**Secondary lane = опц. локальная репликация для офлайна** (слой удобства, не источник истины). Syncthing для **рабочего дерева only**, не git-метаданных. Хаб = канонный шар; сателлиты = `receiveonly` по умолчанию или `sendreceive` только когда нужен офлайн-эдит. Receive-only + revert — ровно паттерн «зеркало, но не новая правда». Версии на ресиверах.

**Ключевое: убрать `.git` из синканного дерева** через `git init --separate-git-dir` / `clone --separate-git-dir` — метаданные снаружи, в working tree остаётся `.git`-указатель. Каждая машина держит свои git-метаданные вне sync-root.

**Три независимых домена** (не синкать зонтик-дерево): `Anton-Knowledge` (горячее, мелкое, git-смежное) · `_originals` (священное, большое, редко меняется) · `_imports` (регенерируемый scratch) — разные политики.

```text
Mac / Windows satellites
  ├─ SSH / VS Code Remote SSH / RDP
  │     └─ Windows hub: WSL + Claude Code; canonical vault tree; Git metadata OUTSIDE sync root
  └─ Optional local mirrors
        ├─ Anton-Knowledge tree via Syncthing
        ├─ _originals only if needed
        └─ _imports usually NOT continuously synced
```

## Implementation blueprint
1. Конвертировать волт из «git-внутри-синк-папки» в «дерево синкается, git-метаданные снаружи» через `--separate-git-dir` (метаданные на быстром локальном SSD вне всех sync-root; на сателлитах — либо без git-метаданных, либо отдельный per-device git-dir).
2. **Три политики синка.** `Anton-Knowledge`: watcher ON, без частых полных рескан (`rescanIntervalS=0` + дёргать REST `/rest/db/scan` ПОСЛЕ batch-импорта/бэкапа). `_originals`: hub **send-only**, сателлиты receive-only или не синкать; единственное место, где Resilio-плейсхолдеры оправданы. `_imports`: **не синкать непрерывно** (регенерируемое; каждый watched scratch = метадата-churn).
3. Кросс-ОС гигиена: держать Syncthing case-safety по умолчанию (НЕ форсить `caseSensitiveFS=true`); `autoNormalize` ON; git `core.ignoreCase` / `core.precomposeUnicode`.
4. **Staggered versioning** на зеркале волта и на receiver'ах `_originals`.
5. RDP/удалёнка вне LAN — только через VPN (хост = Windows Pro/Enterprise/Server); не выставлять RDP голым в интернет.

## Failure modes / decision triggers
- **Нулевое расхождение и полностью пишущие локальные реплики — взаимоисключающи.** Хочешь «нет расхождения» → сократить активных писателей до ОДНОГО (хаб) или строгая дисциплина «пишет только одна машина за раз».
- **Реально ли нужны офлайн-полные реплики?** Если в основном «нет» → remote-first (проще, безопаснее для git, детерминированнее). Если «да, но только заметки» → Syncthing-зеркала контента. Если «да, и хочу имена/плейсхолдеры для архивов без байтов» → Resilio для `_originals`.
- **Платить за Obsidian-native удобство?** Разумно для «синкни мой волт между устройствами», неразумно для «168k git-несущая база + внешние архивы + работа при закрытом приложении».
- **Соблазн SMB/NAS для живого волта — нет.** Obsidian не поддерживает сетевые диски.

**Приоритеты → дизайн:** (1) нет расхождения (2) нет порчи `.git` (3) минимум бюджета (4) работает с любой машины →
- Hub-authoritative рабочая копия на всегда-вкл Windows-десктопе
- Claude Code на хабе, в WSL
- Доступ с сателлитов через SSH / Remote SSH / RDP
- Опц. Syncthing-зеркала только для офлайн-доступа к контенту
- Git-метаданные вынесены наружу через `--separate-git-dir`
- Раздельные политики для `Anton-Knowledge`, `_originals`, `_imports`
