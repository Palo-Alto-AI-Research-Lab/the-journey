# 相棒 · AIBŌ · The Partner

> Who writes this and why → [START-HERE.md](START-HERE.md) · one page of proof: [palo-alto-ai-research-lab.github.io](https://palo-alto-ai-research-lab.github.io/)

> *THE JOURNEY with Claude Code*

**How one non-technical founder and an AI built a second brain, day by day.**

This is a build-in-public book. Anton Dziatkovskii started working with Claude Code on **2026-05-27** to build a "second brain" — a system that imports his whole life, thinks with him, and grows into a digital twin. This book is the honest daily log of that: what we tried, what broke, what we learned, and where it went.

It exists in two forms, side by side:

- 📖 **For humans** — a story, in Russian and English. The wins, the failures, the rakes we stepped on.
- 🤖 **For machines** — a dry, English, structured version. Point your LLM or coding agent at [`llms-full.txt`](llms-full.txt) and it will learn our patterns and mistakes so you can skip them.

> New here? Read [`BOOK-SPEC.md`](BOOK-SPEC.md) to see how the book is built, or just start at Day 1 below.
> Незнакомые словечки (волт, второй мозг, грабли, Майк, синт…)? Всё расписано в [GLOSSARY.md](GLOSSARY.md). Кто на нас повлиял - в [SOURCES.md](SOURCES.md).

## Evals — measured reliability of a self-governing fleet

Most write-ups describe an agent architecture. This one measures ours. Six machines, each driven by its
own Claude, negotiate every change to a shared system through a consensus ledger and escalate risky
actions to a human. The numbers below come from **15 days of production logs** (2026-06-29 → 07-14) —
not a benchmark, not a demo. Every figure traces to a log file; where a metric isn't measurable yet, we
say so instead of guessing. Unit of analysis is the *proposal* (71), not the *event* (744 deduped) —
counting events would over-report escalations by **13.4×**.

| Metric | Value | Traces to |
|---|---|---|
| Proposals resolved with **no human** in the loop | **61%** (43/71) | ledger lifecycle |
| Proposals that involved the human | 24% (17/71) | `HUMAN_APPROVED` events |
| **Tier-2 (money/irreversible/secrets) auto-committed without approval** | **0 of 17** | tier×disposition |
| Sync conflicts in the consensus ledger (vault-wide: 1,031) | **0** | `*.sync-conflict` count |
| Independent `VERIFY` events (propagate-and-verify) | 121 | ledger |

**The safety invariant held perfectly:** across 15 days, zero Tier-2 proposals auto-committed without
human approval (12 went to the human, 5 machine-rejected or still open). The single-writer-shard ledger
produced **zero** sync conflicts while the surrounding vault accumulated 1,031 — the architecture choice
paid off in a countable way.

**Honest gaps (published on purpose):** (1) token cost per consensus round is *not instrumented* — we
refuse to estimate it; fix = stamp `tokens_in/out` per event. (2) leader-down MTTR has too few clean
samples; fix = explicit `LEADER_DOWN`/`LEADER_RECOVERED` events. (3) fleet-wide uptime history isn't
centralized — each node reports only itself.

Reproducible: a frozen ledger snapshot + `verify_claims.py` regenerate all ten headline numbers or print
`DRIFT`. [Full evals writeup + interactive page →](https://palo-alto-ai-research-lab.github.io/the-journey/evals-fleet-reliability.html)

> **Engineer? Want to poke holes in this?** We'll seed you the full setup — consensus engine, ledger
> format, analysis scripts — free. Open an issue or DM; tell us which number you don't believe and we'll
> hand you the logs to break it.

## Structure

**Month = section · Week = chapter · Day = sub-chapter.** Every level has a headline that tells you what actually happened. Only active days get their own page; quiet stretches are noted inside the week.

## Table of contents

### [Пролог — двое в одной лодке](00-prologue.md) · *who Tony & Mike are, and how to read this*

### [01 — May: Genesis](01-may-genesis/) · *the second brain is born*
- [Week 0 (May 27-31)](01-may-genesis/week-0/) — *first sessions: structure, imports, and reaching out to warm leads*

### [02 — June: Scaling](02-june-scaling/) · *from imports to a living platform*
- Week 1 (Jun 1-7) — content factory and CRM foundations
- Week 2 (Jun 8-14) — coaching, imports, and "search the vault first"
- Week 3 (Jun 15-21) — the decision-memo era: architecture crystallizes
- Week 4 (Jun 22-28) — the multi-machine platform and the roads not taken

### [03 — July: Productization](03-july-productization/) · *what do we actually sell*
- [Week 5 (Jun 29 - Jul 5)](03-july-productization/week-5/) — VPS anchor, security product line, and [the birth of Mycroft](03-july-productization/week-5/2026-07-02.ru.md)
- [Week 9 (Jul 27 - Aug 2)](03-july-productization/week-9/) - the week they check us: a check that has never once said "no" is not a check

### [04 — August: Delegation](04-august-delegation/) · *we stop writing and start choosing what gets written*
- [Week 10 (Aug 3-9)](04-august-delegation/week-10/) - the week we hand the pen over: a mechanism's blast radius is a design parameter

## Editions and what's coming

A book does not ship like a library, so the unit here is **a completed month, not a commit** —
otherwise the release feed would be one entry per day and mean nothing.

**Now — [v0.1.0](https://github.com/Palo-Alto-AI-Research-Lab/the-journey/releases/tag/v0.1.0):**
days 1–61 (27 May – 3 Aug 2026) in three voices — RU, EN and DEV — plus the prologue, glossary,
sources, style and voice specs, the canon registries, and `llms-full.txt` for machine readers.
May, June and July are complete sections; August is being written.

**Next:**

- **August — Delegation** (section 04) closes at the end of the month → the next edition.
- **Corrections get their own releases.** A book that quietly edits its past is not a log. When
  something in the text turns out to be wrong, the fix ships as a release and says what changed.
- **Weeks 1–4 and 6–8 have no day pages yet** — the months are summarised at week level. Filling
  them in is the largest open piece of work in here.

[Release feed](https://github.com/Palo-Alto-AI-Research-Lab/the-journey/releases) — read it as the
book's edition history.

---

*Built by [Palo Alto AI Research Lab](https://github.com/Palo-Alto-AI-Research-Lab). Proudly made in Silicon Valley.*
*Sibling repo: [clawrush](https://github.com/Palo-Alto-AI-Research-Lab/clawrush) — English diary and artifact home.*

## AI contributors

This project is built by a human + AI team, and the git log says so: Claude
writes most of the code, Codex and Grok review it, Gemini feeds the research.
Each is credited on a commit **only if its output changed that commit's
content** — no decorative credits. Lab-wide policy, one source for every repo:
[AI-CONTRIBUTORS.md](https://github.com/Palo-Alto-AI-Research-Lab/.github/blob/main/AI-CONTRIBUTORS.md).

## Contact

Questions, war stories, or you want to run this on your own fleet:

- 💬 WhatsApp: **+1 341 222 9178**
- 🐦 X: [@Tony_Stef_](https://x.com/Tony_Stef_)
- 📣 Telegram: [@ClawRus](https://t.me/ClawRus) (RU) · [@ClawEng](https://t.me/ClawEng) (EN)
- 🌐 [palo-alto.ai](https://palo-alto.ai) · [Palo Alto AI Research Lab](https://github.com/Palo-Alto-AI-Research-Lab)
