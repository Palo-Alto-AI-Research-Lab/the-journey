# BOOK-SPEC — "The Journey"

> The contract for how every part of this book is built. Read this before generating any chapter.
> One source of truth. Every day-chapter follows this spec exactly, so the book is consistent and re-generatable.

## What this is

A build-in-public book about Anton Dzyatkovsky building a "second brain" / digital-twin system with Claude Code, day by day, from **2026-05-27** onward. Two audiences, always served in parallel:

- **Humans** — Anton's followers (RU and EN). Narrative voice, honest story of struggle and wins.
- **Machines** — other people's LLMs and coding agents (Cursor, Claude Code, Copilot) who will point their tools at this book to learn our rakes ("грабли") and reuse our patterns. Dry, structured, English.

## Hierarchy

```
Month  = SECTION   (e.g. 02-june-scaling/)      — own headline, own README intro
 Week  = CHAPTER   (week-2/)                     — own headline, own README intro
  Day  = SUB-CHAPTER (2026-06-12.*.md)           — own headline; only ACTIVE days get a sub-chapter
```

Quiet days (no session logs) are NOT files; they are folded into the week README as a one-line "quiet stretch" note. Example gap: **2026-06-02 → 2026-06-05** (4 days, no logs).

## Three files per active day

| File | Audience | Language | Voice / model | Purpose |
|---|---|---|---|---|
| `YYYY-MM-DD.ru.md` | humans | RU | **duet: 🧑 Тони + 🤖 Майк** — Opus+ only | the story in two voices |
| `YYYY-MM-DD.en.md` | humans | EN | **duet: 🧑 Tony + 🤖 Mike** — Opus+ only (adaptation, not literal translation) | same duet for EN followers |
| `YYYY-MM-DD.dev.md` | machines | EN | **pure Майк** — Sonnet scaffold + Opus coherence | dry Problem→Cause→Solution log for LLM ingestion |

**Voices** (full spec in [`STYLE.md`](STYLE.md)): 🧑 **Тони/Tony** = Anton's public persona, cinematic present-tense, show-don't-tell. 🤖 **Майк/Mike (Mycroft)** = synthetic co-founder, second-order framing, capital-literate, from the other side of the screen. Byline on every human chapter: *Придумано Майкрофтом и Тони / Invented by Mycroft and Tony. Palo Alto AI Research Lab.* Open AI co-authorship is the differentiator, not a secret.

## Day chapter anatomy (rich format)

> Writing craft — pacing, triage, personifying resistance, borrowing from the influences, before/after examples — lives in [`PLAYBOOK.md`](PLAYBOOK.md). Governing principle: **narrate by resistance, not by volume; the atomic unit of drama is the moment a model of reality breaks.**

Every active-day **human** chapter has FOUR parts, in order, and **MUST contain every meaningful session of that day — nothing dropped**:

1. 🎬 **Cold open + TLDR** — "This is Day XX. Today: A, B, C." A short, punchy overview of the whole day and what we achieved. Super-important but brief; sets the day's arc.
2. 🏆 **The main story** — the single most important session (e.g. the birth of Mike). Full duet mini-episode, the emotional centerpiece.
3. 🎞 **Also today** — EVERY other meaningful session/dialogue that day (even unfinished ones), each as its own tight mini-episode (a scene + a Тони or Майк beat). One per meaningful session.
4. 🧒 **We learned a lot today** — end-of-day summary: the lessons, the throughline, what the day meant.

**Completeness (MANDATORY):** the extractor enumerates ALL sessions of the day, marks each meaningful/not, and every meaningful one appears as at least a mini-episode. Unfinished session → say so. Nothing meaningful is silently dropped. A session is "meaningful" if it produced a decision, a build, a lesson, a rake, an artifact, or a notable exchange — not pure status-checks/noise.

**Machine chapter (`.dev.md`):** same completeness — ALL thoughts/conclusions/patterns/artifacts/DR from the day + external source links. Format is Майк's choice; nothing dropped.

## Headline convention

Every day/week/month gets a headline that says **what we actually did**, not a date. Format: short, concrete, human.
- Day: `Day 1 — The second brain is born` / `День 1 — Рождение второго мозга`
- Week: `Week 4 — The computers learn to talk to each other without Anton`
- Month: `June — From imports to a living platform`

## Frontmatter (all three files, required)

```yaml
---
title: "<headline>"
date: 2026-05-27
day_index: 1          # 1..N across the whole journey
week: 0
month: "may-genesis"
lang: ru | en | machine
kind: human | machine
artifacts:            # canonical links / paths attached this day, uncut
  - ../artifacts/DR26-06-14-HP17-01-relink.md
tags: [second-brain, obsidian, genesis]
---
```

Stable anchor slugs on every `##`/`###` (kebab-case), so machines can deep-link.

## Artifact attachment rule

Deep Research reports, dashboards, decision-memos produced on a given day are attached **uncut** — full text or canonical GitHub link, never a summary. They live in `artifacts/` and are linked from both the human and the machine file of that day. (Canon: `reglament-artefakty-i-dr-bez-kupyur-v-longridah`. DR registry: `E:\Obsidian\Anton-Knowledge\_DR-Registry.md`.)

## Generation pipeline (per active day)

1. **EXTRACT (Sonnet, 0-token-cheap):** read that day's raw logs in `01-Conversations\Claude\HP17-ZBook\YYYY-MM-DD--*.md` + any Retros + Decisions dated that day. Emit a structured fact-sheet: what was attempted, what was built, what broke, what was decided, artifacts produced, the emotional/honest beat.
2. **HUMAN RU (Opus):** write the story from the fact-sheet. Reality-show / diary voice. Honest about failures.
3. **HUMAN EN (Opus):** adapt (not literally translate) the RU into natural EN for the English audience.
4. **MACHINE EN (Sonnet scaffold → Opus coherence):** dry dev-log — Problem → Cause → Solution → Reusable pattern. Clean headers, minimal prose.
5. Attach artifacts, add footer (see below), cross-link ru↔en↔dev.

Weeks are independent (don't share files) → the whole book can be mass-generated in parallel, one week per worker.

## Voice & safety gates (every file)

- Author voice (`.ru`/`.en`) = **Opus only**, never Sonnet. (Canon: model-routing.)
- **Security-language gate:** no "we make agents secure" claims; only provable controls / blast-radius language; publish **patterns, not live internals**. (Canon: `security-claims-language.md`.)
- **Leak-scan** before any push: no secrets, no private data, no phone numbers except the authorized co-founder CTA WhatsApp `+1 341 222 9178`.
- Brand: full "Palo Alto AI Research Lab" / short "Palo Alto Lab"; tagline "Proudly made in Silicon Valley".

## Footer ladder (per file, from `_STYLE-footer.md`)

Human files: 📖 both long versions link + follow + talk-to-co-founders (Calendly `calendly.com/paloaltolab` + WhatsApp `+1 341 222 9178`) + hire-us + byline.
Machine files: dry — the two links + "give this link to your coding agent, it'll figure it out itself" + contact.

## Machine index

- `llms.txt` — site map for AI agents (points to the book structure).
- `llms-full.txt` — the ENTIRE machine book concatenated into one ingestible file (regenerated at assembly time). This is the "machine-readable book" a stranger points their LLM at.

## Publishing

GitHub push is autonomous (Anton "ДОВЕРЯЮ", 2026-07-03) after leak-scan passes. Repo: `github.com/Palo-Alto-AI-Research-Lab/the-journey` (new, links to sibling `clawrush`). Repo creation needs the web UI (PAT lacks admin scope); content push via normal `git push` (PAT fixed 2026-07-04), web-commit fallback as break-glass.

## Sources of truth

- Day content: `E:\Obsidian\Anton-Knowledge\01-Conversations\Claude\HP17-ZBook\` (continuous 2026-05-27 → today, 1463 files).
- "Why / what shipped": `01-Conversations\Claude\Retros\` (dense from 2026-06-12) + `02-Decisions\`.
- Milestones: memory `MEMORY.md` / `MEMORY-archive.md`.
