---
title: "Decision Memo — Long-horizon attention layer (восстановить прошлое + копить вперёд)"
aliases: ["Decision long-horizon attention layer 2026-06-18", "Откуда брать долгую историю браузера"]
type: decision
stage: distilled
status: proposed
date: "2026-06-18"
protocol: alpha-protocol-recall-plus-deep-research
decided_by: pending
confidence: high
tags: [decision, alpha-protocol, browser-history, second-brain, digital-twin, lifelogging, attention-layer]
---

# Decision Memo — Long-horizon attention layer

> **Alpha Protocol (R+DR).** Recall (наш Второй Мозг) + **ДВА независимых внешних Deep Research** (2026-06-18; сырьё `_originals\deep-research\long-term-attention-layer-2026-06-18.md` + `...-report2.md`) → синтез. **Две независимые разведки СОШЛИСЬ по сути → высокая уверенность.** **Дополняет** [[decision-browser-history-knowledge-pipeline-2026-06-14]]: тот — про АНАЛИЗ истории (alpha-scoring/execution-gap), этот — про ИСТОЧНИКИ истории (откуда взять прошлое + как копить вперёд).

## Проблема
У Google истории браузера старше ~90 дней НЕТ (подтверждено: Takeout `Chrome/History.json` и `My Activity → Chrome` оба обрываются на 2026-03-21). Нужна долгая (10–15 лет) «карта внимания» для цели №1 (цифровой двойник): откуда достать ПРОШЛОЕ и как надёжно КОПИТЬ вперёд — приватно, локально, ремонтопригодно (Windows-основной + iPhone, база в Португалии/EU).

## Что мы уже знали (Recall)
90-дн лимит Google; накопитель `browser_history.db` живёт с 12.06 и оказался ПОЛНЕЕ Takeout (фолд `123427Z` дал +0 новых); 3-слойка [[vault-data-architecture]] (SQLite факты + e5 смысл + markdown); screenpipe ранее отложен как ⚠️ слежка/overbuild; видение Антона — «второй мозг из 10–15 лет цифровой памяти».

## Что дал Deep Research (ключевое)
**Стратегия одной строкой: «recover locally, capture minimally, enrich semantically, full-screen memory — только эпизодически».** Не тотальная слежка, а слоёная система, собирающая МИНИМАЛЬНО ДОСТАТОЧНЫЙ сигнал «на что я реально обращал внимание».

**Восстановить прошлое (по убыванию выхода):**
1. ⭐ **Старые ЛОКАЛЬНЫЕ профили браузеров и их бэкапы** — самый высокий выход. Chrome/Edge `History` (SQLite), Firefox `places.sqlite` на старых ноутах/SSD/второстепенных профилях/скопированных `User Data`. Не у Google и не у ISP.
2. **Бэкапы** этих же БД: Windows File History, Time Machine (старые Mac), iCloud/iTunes-бэкапы старых iPhone — могут хранить более ранние копии.
3. **Свои DNS-логи** (Pi-hole/AdGuard/NextDNS) — только ДОМЕННЫЙ уровень, если когда-то велись.
4. (низкий выход) **Google Web&App Activity общий** (Search/Maps/YouTube) — это search-intent, не история браузера; **ISP/GDPR-DSAR** — last-ditch (Португалия после реш. КС 268/2022 порезала retention).

**Копить вперёд:**
1. Накопитель `browser_history.db` = **канон, append-only**; добавить поля: device_id, browser, profile, normalized-domain, referrer, confidence-маркер (raw-visit vs sessionized).
2. ⭐ **ActivityWatch** на Windows (локально, структурные события: active app/tab, AFK/idle, dwell; экспорт JSON/REST) → в наш SQLite. Самый дешёвый осмысленный апгрейд; это «фокус/время», чего история не даёт. НЕ скриншоты.
3. **Мобайл = транспорт, не хранилище:** стандартизировать **Firefox Sync** (Win+iPhone) → история падает в десктоп-профиль, который накопитель архивирует навсегда; Chrome-sync — только транспорт. Не путать sync с backup.
4. **Один** семантический слой ПОЗЖЕ, поверх стабильного архива: Promnesia (provenance «почему/откуда видел») ИЛИ Khoj (retrieval над markdown) — это слой интеллекта, не сбора.
5. **Screenpipe** (сильнейший recall-тул) — только **time-boxed** (исследовательский спринт/конфа/проект), локальные модели, шифр-диск, retention-cap. НЕ 24/7-дефолт.

**Хранить 3 уровня внимания РАЗДЕЛЬНО:** (1) навигация (url/title/time/referrer/device) · (2) фокус (active window/tab, AFK, dwell) · (3) значимость (bookmarks/highlights/notes). Смешивать при запросе — да; при записи — нет.

### Уточнения из DR #2 (конкретика, конвергенция)
- 🎙️🇵🇹 **АУДИО — РАЗРЕШЕНО Антону (у него ВСЕГДА есть согласие).** Поправка Антона (2026-06-18, origin: anton): он ВСЕГДА получает согласие лидов на запись митингов → запись легальна. Penal Code Art.199 бьёт только по СКРЫТОЙ записи без согласия — это НЕ наш случай. ⇒ **аудио-capture митингов ВКЛЮЧАЕМ** (Screenpipe/Whisper ок для созвонов с согласием). Единственная остаточная осторожность: не писать СЛУЧАЙНЫХ третьих лиц/ambient без согласия (бытовой фон), но это не блокирует рабочие записи. НЕ «audio off jammed».
- **Google «My Activity» может хранить 3/18/36 мес** (настройка авто-удаления Web&App Activity), и тянется через Takeout, если выбрать **«My Activity»**, а НЕ «Chrome». Наш факт (90 дн) = у Антона авто-удаление, видимо, стоит на **3 месяца** → прошлое уже стёрто, но **ВПЕРЁД глубину можно расширить**, переключив на 36 мес / «не удалять». ⇒ дешёвое действие: проверить `myactivity.google.com` авто-удаление + выгрузить ПОЛНЫЙ «My Activity» (Search/Maps/YouTube — там search-intent может быть глубже 90 дн).
- **iOS — конкретный путь восстановления:** локальный НЕшифрованный iTunes-бэкап на Windows → Python `iosbackup` парсит Safari `History.db` + CoreDuet `knowledgeC.db` (App.InFocus; Mac-epoch 2001→Unix) → в наш SQLite. Месяцы-годы мобильной истории без джейлбрейка.
- **Firefox = бесконечная локальная история:** `places.history.expiration.max_pages=2147483647` в about:config (иначе чистит ~80MB). Если перейти на Firefox — родной долгий архив.
- **Forward-тех:** Screenpipe брать через **accessibility-tree (UIAutomation), НЕ OCR** (5–10% CPU, ~5–10ГБ/мес, структурный текст; OCR — только fallback). **ArchiveBox** — постоянная локальная копия ценных страниц (анти-linkrot). **Promnesia** — единый индексатор (Takeout JSON + iOS-экстракты + ActivityWatch → один граф).
- **Гигиена БД (15 лет):** `PRAGMA auto_vacuum=INCREMENTAL`/плановый VACUUM, append-only, периодический ребилд **FTS5**; шифр-диск (BitLocker/VeraCrypt); deny-apps (парольники/банк/Signal) фильтруются В ПАМЯТИ до записи; игнор incognito.
- **«Dream-cycle» консолидация:** в простое локальный LLM сжимает сырые трейсы → иерархические markdown-саммари (= наш markdown-слой; биологическая «память во сне»).

## Что НЕ делать
ISP/telecom как основной путь восстановления; проприетарное облако (Limitless/Rewind→Meta) как канон-архив; 24/7 скриншот/аудио для обычного браузинга (макс. риск/сожаление); лишние extension-логгеры (широкие history-права, расширяют trust-surface); путать sync с backup.

## Риски
secret-leak в локальном plaintext (→ шифр-диск/FDE, single-user машина); collection > retrieval (узкое место — индексация/ретрив, не сбор); storage-bloat у скриншот-тулов (event-log vs screenshot = разница на порядки за 15 лет); vendor-discontinuity; legal/social (запись третьих лиц без согласия).

## Рекомендация — приоритеты
**RECOVER PAST:** (0, дёшево-сейчас) проверить `myactivity.google.com` авто-удаление → если 3 мес, переключить на 36/«не удалять» (спасает ВПЕРЁД) + выгрузить ПОЛНЫЙ «My Activity» (Search/Maps/YouTube — search-intent, может быть глубже 90 дн); (1) ⭐ инвентаризировать + прочесать ВСЕ старые локальные источники → fold `History`/Firefox `places.sqlite` в `browser_history.db` (тот же кросс-дедуп) → реальный путь к многолетке браузинга; (2) бэкапы (File History/Time Machine); (3) iOS: НЕшифр. iTunes-бэкап → `iosbackup` → Safari `History.db`+`knowledgeC.db`; (4) старые DNS-логи если были.
**CAPTURE FORWARD:** (1) накопитель-канон + новые поля; (2) ActivityWatch → SQLite; (3) Firefox-транспорт для мобайла; (4) один семантический слой позже; (5) Screenpipe — эпизодически.

## Открытые вопросы
Какие старые устройства/диски/бэкапы у Антона РЕАЛЬНО есть (нужна инвентаризация — без неё recover-past вслепую); iPhone raw-extraction слабее на Win-сетапе (зависит от наличия бэкапов).

## Следующий шаг (нужен greenlight Антона)
Что запускаем первым: **(A)** recover-past — инвентаризация + прочёс старых локальных профилей/бэкапов; **(B)** capture-forward — ActivityWatch → наш SQLite; **(C)** оба параллельно. Только после greenlight — реализация.

## Связи
[[browser-history-import]] · [[decision-browser-history-knowledge-pipeline-2026-06-14]] · [[vault-data-architecture]] · [[main-goals]] · [[second-brain-northstar]] · [[self-bible-identity-layer]] · [[alpha-protocol-recall-plus-dr]]
Сырьё DR: `_originals\deep-research\long-term-attention-layer-2026-06-18.md` + `...-report2.md`
