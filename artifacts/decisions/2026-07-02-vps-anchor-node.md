---
title: "Decision Memo — VPS «якорь» флота: узкий control-plane, не второй хаб"
type: decision
stage: distilled
origin: mixed
authored_by: hybrid
ai_author: claude-fable-5
date_established: 2026-07-02
machine: A-2022BAYAREA
operator: Anton
theme: infrastructure
status: approved
tags: [vps, anchor-node, hetzner, consensus-witness, backup, restic, tailscale, alpha-protocol]
concept: "[[protocol-alpha-protocol-recall-plus-deep-research]]"
---

# Decision Memo — VPS «якорь» (по Alpha Protocol R+DR, 2026-07-02)

## ⭐ ОБНОВЛЕНИЕ 2026-07-03 — КУПЛЕНО + смена роли
Сервер **`fleet-anchor`** заказан у Hetzner (аккаунт btcnextdevops, проект "default" 1685279): **CPX62** (x86 AMD, 16 vCPU / 32 ГБ / 640 ГБ NVMe), Falkenstein, Ubuntu 26.04 LTS, IPv4 **167.233.122.145** + IPv6, **€130.49/мес**.
- **Отклонения от плана (осознанно, по решению Антона 2026-07-03):**
  1. **Роль:** из «узкий control-plane witness» → **рабочая машина под Claude Code-сессии** (Антон гоняет 5–15 сессий, нужна память). Это тот самый «role creep», что мемо запрещало — принято сознательно как новая цель узла.
  2. **Железо:** ARM CAX31/CAX41 (план, €24–41) был **распродан во всех EU-локациях** (дефицит Ampere) → взяли доступный x86. Дешёвый Intel CX53 (€29) и dedicated CCX тоже распроданы/дороже (CCX33 €138). Единственный доступный 32 ГБ сегодня = CPX62 €130 (втрое дороже плана — цена срочности).
  3. **32 ГБ** сразу вместо старта на 16 ГБ.
- **PENDING (Phase 0 хардунг, ещё не сделано):** root-пароль пришёл на btcnextdevops@gmail.com (2FA-ящик, Claude прочитать не может) → нужен первый вход; далее SSH-ключ, non-root юзеры, файрвол/Tailscale, потом Claude Code (Phase 2). Креды сервера в `secrets\credentials.md §Hetzner`.

## Проблема
Всё живое (рутины, шина машин, watchdog'и, approval-relay, кворум) привязано к дому. Дом умер (свет/интернет/пожар) → данные целы (3-2-1 есть с 06-06), но система немая. Нужен always-on узел вне дома. Плюс мандат Антона (2026-07-02): узел = **второй свидетель кворума** флота.

## Текущие знания (recall)
- Волт: 349 447 файлов / 24.5 GB / 48.8k папок; Syncthing-стар 4+ машин; ночной git-bundle → GDrive + C:\ (3-2-1 живой, healthcheck еженедельный).
- Уже есть 2 VPS: n8n (CRM-агенты) и CRM prod (Docker Swarm + Mongo, чувствительные данные).
- Решение-родитель: GPU-работа и авторский голос (Opus-подписка) остаются на хабе; VPS = CPU-only always-on.
- Гардрейлы: агент без разрушительного доступа к единственной копии; канон не на VPS; Tier-2 через QQQ.

## Результаты DR
**DR-1 (провайдер, 2026-07-01, 180 ист.):** Hetzner CAX31 (ARM64, 8vCPU/16GB, ~€24–26/мес, Nuremberg/Falkenstein); альтернатива Netcup RS 2000; Restic → B2/Storage Box.
**DR-2 (архитектура, 2026-07-02):** оригинал = `_originals/deep-research/2026-07-02-vps-anchor-node-architecture-dr.md`. Вердикты:
1. **Роль = узкий control-plane** («наблюдать, ставить в очередь, ретраить, уведомлять» — НЕ уничтожать/тратить/публиковать). Не второй хаб.
2. **Новый отдельный VPS** — НЕ на CRM-хост (там лиды/деньги, blast-radius) и не на n8n-хост.
3. **Волт: полной реплики НЕТ на старте.** 350k мелких файлов = worst-case Syncthing (форум: 380k файлов на ARM — до 8 дней первичного синка; inotify default 8192 надо поднимать). Паттерн: Tailscale-доступ к живому хабу + **маленькая отдельная шара** (ровно наш `_machine-bus` для witness-роли). Если позже нужна реплика — receive-only curated subset; receive-encrypted только как непрозрачный склад.
4. **ARM64 ок** для стека (Node/Telethon/Syncthing/restic/Tailscale); x86 — только при доказанной x86-only зависимости.
5. **Claude Code на Max = best-effort лента, не прод-инфраструктура:** setup-token (~год, one-year OAuth, официально для скриптов); 1 headless-задача за раз; wrapper (лог + fail-closed на 401/лимиты + идемпотентность); свой cron/systemd, НЕ Anthropic-hosted scheduler; станет mission-critical → API-billing для unattended-части.
6. **Telegram user-аккаунт с VPS:** только read-mostly / low-rate / approval-gated на зрелом аккаунте с 2FA; hard-stop на SessionRevokedError → «human-only» режим + алерт; первые сессии/массовое/cold — не с датацентр-IP. Официального «Hetzner запрещён» нет — есть анти-спам чувствительность к среде.
7. **Бэкап: Restic ДОПОЛНЯЕТ git-bundle** (bundle = git-история; restic = состояние машины: конфиги, сессии, очереди, SQLite). Retention старт: daily 14 / weekly 8 / monthly 12 / yearly 2; ежемесячный `restic check --read-data-subset`; квартальный restore-drill на throwaway. Append-only: forget/prune с отдельного админ-контекста.
8. **Hetzner lockout:** скучная гигиена — 2FA, чистая регистрация без VPN, verified billing, runbook «Hetzner недоступен».

## ⭐ DR-3 (второй трек архитектуры, получен 2026-07-03 00:19) — НЕЗАВИСИМАЯ СХОДИМОСТЬ
Оригинал: `_originals/deep-research/2026-07-03-vps-anchor-node-architecture-dr-track2.md`. Все 8 вердиктов совпали с DR-2 (новый VPS · без полной реплики · ARM ок · CAX31 остаётся · Restic дополняет bundle · Hetzner-гигиена). Уверенность решения → **high**. Дельта (жёстче, принимаем):
1. **Control-plane = LLM-free.** Кворум/heartbeat/шина НИКОГДА не зависят от Claude: обычный Python + systemd/cron. Claude Code на якоре = «удобство, не SLA» (совпадает с нашим правилом «тупой детектор первым»). Порядок стройки: witness (Stage 3) → Telegram (Stage 5) → Claude (Stage 6, последним).
2. **Telethon-сессия рождается НА VPS** (тот же сетевой путь, где живёт) и никогда не мигрирует/не синкается между машинами (`AUTH_KEY_DUPLICATED`; совпадает с [[machine-identity-stays-local]]). ⚠️ Регистрация app на my.telegram.org с cloud/VPN-IP даёт opaque ERROR — app-креды создавать с резидентного IP.
3. **Шара шины: развести writable witness-путь и реплицируемый bus-путь.** ⚖️ Ответ кворум-сессии (2026-07-03, принято): **single-writer-per-file в корне `_machine-bus` = принятый эквивалент writable-subpath** — каждая машина пишет ТОЛЬКО свои файлы (`.robot-alive-<SELF>.log`, `log-<SELF>.jsonl`, `wm-<SELF>.json`), co-write отсутствует by construction; отдельный подпуть = спецслучай для одного узла (класс расхождений). Ревизит при онбординге VPS (поддержка `_presence/` = 2 строки, если share-ACL потребует). Receive-encrypted не брать — beta, теряет block-reuse.
4. Retention-минимум 7d/5w/12m (наш 14/8/12/2 richer — оставляем); `restic check` ≠ restore-тест: месячный тест-restore + квартальный rebuild-drill.
5. Hetzner: первый заказ — маленький/дешёвый продукт (fraud-скрининг мягче), данные после lock НЕ гарантированы → узел обязан пересобираться из IaC+бэкапа.
6. Матрица «хаб умер»: работает — witness/штампы/шина/watchdog/approval/TG-polling/самобэкап; через tailnet — широкие чтения волта; не работает — GPU/авторский голос (и не должно).
- **A (выбор DR):** новый CAX31, control-plane-only, поэтапный rollout. ✅ рекомендован.
- B: консолидация на существующий VPS — отвергнут (blast-radius, DR-2 §6).
- C: полная реплика волта на VPS — отвергнут на старте (IO-drag без пользы; пересмотр после Phase 1 при доказанной нужде).

## Риски
Token expiry / refresh races (норма эксплуатации — ловить wrapper'ом) · TG session revocation · «role creep» (самый глубокий: якорь за полгода вбирает канон/секреты/CRM → становится той самой единой точкой — запрещено этим мемо) · disk-full/рост логов (наблюдался ~50MB/день у scheduled-сессий) · Hetzner-lock.

## Рекомендация — поэтапный план (GO после «+» Антона; закупка = Tier-2)
- **Phase 0 — закупка+харднинг:** CAX31 Ubuntu 24.04 ARM64; Hetzner 2FA; Tailscale-first (SSH только в tailnet); non-root юзеры `anchor-core` / `telethon` / `claude-jobs`, systemd-sandbox (NoNewPrivileges, ProtectSystem).
- **Phase 1 — control-plane + witness (ДО всякого AI):** SQLite-очередь, systemd-таймеры, heartbeat-мониторы машин, approval-inbox, алерты, ротация логов; witness-набор: шара `_machine-bus`, `consensus.py` тик 20 мин, presence-штамп, machines.json + peer_timeout_min=60. **Acceptance-тест: выключить хаб → VPS шлёт heartbeat-loss алерты и принимает approvals.** MACHINE_KEY → сессии кворума → quorum.enabled.
- **Phase 2 — Claude Code guarded:** setup-token в защищённом env, wrapper-логика, 1 job за раз.
- **Phase 3 — TG read-only** на зрелом аккаунте, потом approval-gated ответы; hard-stop на revocation.
- **Phase 4 (опционально, по доказанной нужде)** — micro-folder волта (receive-only), не всё дерево.
- **Phase 5 — Restic → B2/Storage Box** + retention + restore-drill. Тест: «пересобрал якорь из доков+бэкапа, не трогая живую машину».

## Связано
- [[2026-07-01-storage-runbook-vps-variants-claudeai]] (вход: runbook A/B + DR-1)
- [[multi-machine-auto-consensus]] · [[n8n]] · [[decision-vault-sync-architecture]]
