---
title: "Decision Memo — Движок добычи лидов+альфы из элитных Web3-комьюнити (репутационно-безопасно)"
type: decision
stage: distilled
origin: anton
authored_by: hybrid
status: proposed
date_established: 2026-06-18
decision_owner: anton
concept: "[[concept-platinum-crm]]"
tags: [decision, web3, defi, lead-gen, outreach, alpha-protocol, lobsterdao, engine]
---

# Decision Memo — Web3 Community Mining Engine 🦞→🎯

> Синтез Alpha Protocol (R+DR). RECALL (наш Второй Мозг) + [[DR-web3-community-mining-engine|Deep Research]] → варианты → риски → рекомендация. Корпус-пилот: [[_LobsterDAO-Alpha-MOC|LobsterDAO]].

## 1. Решение, которое принимаем
Строить ли повторяемый движок «элитный крипто-чат → (а) квалифицированный pipeline отношений + (б) проприетарный alpha-фид», репутационно-безопасно — и в какой архитектуре (тяжёлой enterprise или АК-47).

## 2. Что мы уже знаем (RECALL)
- **Корпус добыт:** LobsterDAO (374k сообщ., 10.3k чел., 2018–26) → influence/connector-граф, лид-скоринг (топ-400), follow-радар, тулчейн, летопись. База `_imports\lobster\lobster.db`; альфа-слой уже в волте ([[_LobsterDAO-Alpha-MOC]]).
- **Анти-DM зашита в самих данных:** топ-участники зовут себя «I will never DM you first».
- **Тема не новая:** в CRM уже есть лид [[alexis-mailley|Alexis Mailley]] (Lobster Protocol), были разборы «Привлечение LP-провайдеров» и «VC Scout Data Sources» — LobsterDAO там назван пулом LP/китов.
- **Наш стек, который переиспользуем:** Платинум-CRM; Telegram MCP (send, 2 аккаунта); **контент-фабрика + FB-дневник + speak-gershuni** (авторский голос = Opus); волт+RAG; [[decision-alpha-extraction-engine-variant-a|alpha-extraction-engine]] + prediction-ledger; namesearch/dialogs.db. У нас УЖЕ есть Eliza и OpenClaw-parity — те самые агенты, что DR называет трендом.
- **Скрейп нам не нужен:** экспорт легально на руках как у участника чата → весь риск-блок DR про Telethon-баны для нас неактуален.

## 3. Что добавил Deep Research (ключевое, сверх recall)
1. **Анти-DM = ядро, подтверждено внешне** → разворот «draft-then-send» ⟶ **«listen-then-publish» / earned status**. Холодный DM = screenshot-shaming → соц-блэклист всего фонда.
2. **5 мотивов аутрича:** (a) inbound-honeypot (технический ресёрч в чат без солиситинга), (b) тёплая триангуляция через Farcaster-граф (double-opt-in), (c) участие в governance (Snapshot/forum), (d) IRL-конвергенция (ETH Lisbon/Devconnect/EthCC), (e) ценность-в-канале.
3. ⭐ **NFT identity anchor:** дроп **6 751 NFT LobsterDAO (2021)** через Merkle-клейм с ETH-адресом = готовая on-chain карта «TG-ник → кошелёк/ENS» для элиты. Резолв без скрейпа и деанона. *(DR-заявка — проверить on-chain перед опорой.)*
4. **Резолв личностей:** Next.ID / Web3.bio SDK / Neynar (Farcaster) / Airstack.
5. **Alpha без самообмана:** survivorship/hindsight bias → нормализовать против market beta + decay-фактор на старые прогнозы.
6. **Легал:** progressive profiling (НЕ форсить KYC рано), legitimate-interest + раскрытие источника, right-to-erasure.
7. **Эталоны:** Variant (встроиться+публиковать), 1kx (финансировать public goods), Galaxy «Constellations» (офлайн-ужины).

## 4. Варианты (с оценкой АК-47)
- **A. Тяжёлый enterprise-стек** (буквальный план DR: ClickHouse + Common Room/Kazm Enterprise + куча API). ⚠️ **УСЛОЖНЕНИЕ** — лечит масштаб, которого у нас пока нет; не чинится «молотком и отвёрткой»; вендор-косты, платные API (против правила «сначала включённые лимиты»). **НЕ берём как v1.**
- **B. АК-47 минимальный — РЕКОМЕНДУЮ.** Переиспользуем имеющееся: `lobster.db` (SQLite) = склад; identity = NFT-anchor + ручной топ-40; honeypot = контент-фабрика роняет один LobsterDAO-обоснованный технический пост; тёплый путь = наш super-connector-граф; CRM = Платинум. Ноль новых вендоров, ноль платных API сверх включённых.
- **C. Ничего / разово.** Отвергнут: ценность повторяемая, ложится в Цель №1 (Второй Мозг / цифровой двойник).

## 5. Риски и страховки
- **Репутация (весь смысл)** → жёсткий гейт **zero-cold-DM** в элитные комьюнити, только value-first/warm. → **в Библию** (правило для всех акторов: я, ассистенты, агенты).
- **Bias в альфе** → decay + нормализация против беты; фид кормит существующий prediction-ledger, не плодит сущность.
- **Легал** → progressive profiling; данные у нас легально (членский экспорт); раскрытие источника при первом контакте.
- **Over-automation бан** → мы не скрейпим; ongoing — read-only/официальный API.
- **Governance-рейдинг** → участвуем органически, без скупки ради форс-голоса.

## 6. Рекомендация — старт по АК-47 (первые кирпичи)
**Phase 0 (дёшево, сейчас, 0 платных API):**
1. **Bible-правило zero-cold-DM** для элитных комьюнити (value-first/warm-only) — человеко+AI актор → Библия.
2. **NFT identity anchor** — проверить и стянуть on-chain карту клейма 6 751 NFT → джойн к нашему топ-40 → мгновенно кошелёк/ENS у элиты. (детерминированно, 0 токенов)
3. **Honeypot v1** — контент-фабрика (голос = Opus) делает ОДИН value-first технический пост, обоснованный альфой LobsterDAO → earned inbound (тест мотива b/a).

**Phase 1:** identity-пайплайн (Neynar/Web3.bio) по топ-50; warm-path finder поверх super-connector-графа.
**Phase 2:** alpha-фид с bias-контролем → в alpha-extraction-engine/prediction-ledger.
**Phase 3:** IRL-карта (кто из целей на ETH Lisbon/Devconnect) + участие в governance.

## 7. На рутину?
Движок = кандидат в **рутину**: ежемесячный ре-майнинг + refresh identity + один honeypot-пост. Решает Антон после Phase 0 (правило «повторяющееся → оцени на рутину»).

## Связи
[[DR-web3-community-mining-engine]] · [[_LobsterDAO-Alpha-MOC]] · [[lobsterdao]] · [[decision-alpha-extraction-engine-variant-a]] · [[concept-platinum-crm]] · [[concept-defi]] · [[concept-web3]]
