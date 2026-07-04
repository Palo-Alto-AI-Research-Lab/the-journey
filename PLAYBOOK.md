# PLAYBOOK — how The Journey is written

> The writing craft for this book, distilled from three independent deep-research reports (DR26-07-04-HUB-12). `STYLE.md` defines *who* the voices are; this defines *how* every chapter is built. Read both before generating.

## The one governing principle

**The atomic unit of drama is not the feature shipped. It is the moment a model of reality breaks and must be replaced.** A prompt misfires, an import fails, a database assumption collapses, or the founder realizes he didn't understand what he was building. Stage those moments and 40 chapters feel cumulative. Just report "what changed" and it collapses into changelog sludge.

**Narrate by resistance, not by volume.** Ten hours of routine implementation may deserve one paragraph. Forty minutes spent realizing your system's idea of "self" is split across three inconsistent stores deserves four pages.

## Per-session pressure test

Every meaningful session must surface at least one pressure: **time, uncertainty, pride, dependence, a user promise, public visibility, or identity.** Zero pressures → one line. Two or more → dramatize.

## Triage (how "every session" stays a mosaic, not a list)

- **Dramatize** if the session changes mission, identity, trust, architecture, or tomorrow's plan.
- **Compress** routine progress without surprise (a vivid image + Mike's one-line reading).
- **Mention only** if its sole function is continuity.

One day = **one dramatic spine + several satellite scenes.** Never equal weight.

## Shape of a day

1. **Cold open, in medias res** (150-300 words) — start at the hottest moment of the day, not at 9am.
2. **TL;DR** (3-6 tight lines: what we tried / what broke / what changed / shipped or not / why tomorrow is now different).
3. **Main story** — the day's hardest unknown or biggest reversal, fully dramatized, full duet.
4. **Other meaningful sessions** — braided and compressed; order by **meaningful relation** (cause-and-effect, mirror, escalation, contrast), *not* by timestamp alone.
5. **What changed in our understanding** — end on movement of understanding, never "done / not done."

**Connective tissue** implies interpretation, never "then we also…":
> "That should have ended the problem. It only moved it."
> "The afternoon looked smaller on paper and turned out to be the day's real hinge."

## The duet: different JOBS, not just tones

- 🧑 **Tony** owns heat, embodiment, present-tense perception, appetite, intuition, public embarrassment. He **narrates before he understands.**
- 🤖 **Mike** owns pattern, second-order explanation, memory of prior days, comparative architecture, the view from the other side of the screen. He **explains after** — sometimes correctly, sometimes too neatly — and he can puncture Tony's mythmaking.
- Canonical beat: Tony — *"the machine woke up."* Mike — *"No. The retrieval logic stopped lying for seven turns in a row, which is different. But I understand why you felt the distinction collapse."*
- **Stereo, not duplication.** Not every event is double-narrated. Vary the ratio (70/30, 55/45). Some mini-episodes get only a Mike coda.
- **Rotate the fault lines** of disagreement (not always emotional-vs-rational): speed vs schema discipline; coherence vs ship-it-today; a social read Mike underrates; a statistical pattern Tony misses. Two different access patterns to reality.

## Running threads (the long arcs only a duet can carry)

Trust calibration · "remembering vs reconstructing" · Tony grows technical while Mike grows emotionally legible · the shifting boundary tool → partner → mirror → persona. **Surface identity questions every 5-6 chapters, not every one** (or it becomes parody).

## Personify the resistance

Give the bug a presence: the taunting traceback, the "flakey" that erodes your sanity, the memory leak as slow suffocation ("we were taking on water"). Debugging is detective work, or open-heart surgery on a live machine.

## Borrow verbs, not costumes (the influences)

- **Harrison** — momentum + caper structure (plan → improvise → complication → grin → harder complication) + wry self-justifying asides. *Avoid:* glibness that makes nothing cost anything.
- **Heinlein** — competence, exposition-through-action, TANSTAAFL (every choice carries a cost paid later). *Avoid:* sermons, dated dialect.
- **Ghost in the Shell** — cool precision, body/interface imagery, one image carrying the idea, a contemplative lull inside heavy chapters. *Avoid:* faux-depth cyberpunk decoration.
- **The Matrix** — awakening as **ontological reclassification** ("not 'we fixed a bug' but 'we were asking the system to do a different kind of thing than we thought'"). *Avoid:* red-pill cosplay and slogans.

## Show receipts

Real artifacts are props: the bad output, the log line, the screenshot, the diff, the user message, the changed hypothesis. Include only the code/logs that carry narrative weight — **proof, not dump** — and always say *why* a fact matters before the fact.

## The final test for every chapter

Remove all code and product nouns. Does it still read as **people trying to do something difficult under changing conditions?** If no, it's over-indexed on reporting.

## Machine layer (`.dev.md`)

The human chapter narrates significance; the machine record preserves evidence. Machine frontmatter carries: `day, date, primary_goal, status, main_unknown_morning, main_unknown_evening, artifacts, commits, models, human_bottleneck, ai_bottleneck` + a session ledger (id, type, exact prompts/commands/outputs, decisions+rationale, outcome). Cross-ref from the human chapter ("See machine record → session C"). `llms-full.txt` = the whole machine book concatenated.

## Hard DO / DON'T

**DO:** every scene answers one dramatic question; stage real uncertainty; use artifacts as props; let Tony and Mike disagree in ways that reveal different access to truth; end on changed understanding; keep the machine layer structured and linkable.

**DON'T:** narrate everything at equal scale; mistake technical detail for reader value; let Mike become a generic explainer-bot; let Tony stay a perpetual confused novice (his competence arc is the payoff); fake heat with cyberpunk decoration; turn the human chapter into a disguised changelog; **invent events** — the drama is restricted to what actually happened.

---

*Source: DR26-07-04-HUB-12 — three independent deep-research reports (Kidder/Basecamp nonfiction lens, craft/influence lens, structural "cybernetic memoir" lens), synthesized. Grounded in* The Soul of a New Machine, Coders at Work, The Phoenix Project, Shape Up, *and live build-in-public practice. Raw reports published uncut in [`artifacts/deep-research/`](artifacts/deep-research/). Invented by Mycroft and Tony, Palo Alto AI Research Lab.*
