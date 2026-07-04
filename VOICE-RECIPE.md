# VOICE-RECIPE — the signature house voice

> The concrete mixing formula for the Tony/Mike duet, synthesized from two independent deep-research reports (DR26-07-04-HUB-14). `STYLE.md` = who the voices are; `PLAYBOOK.md` = pacing/structure; **this** = the exact voice blend, humor engine, and house rules. Written for the Fable model to follow line by line.

## The one-line target

> Write each chapter as if **Karpathy and Scalzi were helping Ted Chiang punch up a startup diary, while Gibson and Ridley Scott handled atmosphere, and Douglas Adams kept stealing the seriousness at exactly the right second.**

Simple enough for a lay reader, precise enough for an engineer, funny enough to disarm resistance, serious enough that the stakes still land.

## The mixing formula

**🧑 Tony** (embodied, present-tense, carries momentum and emotion):
- **30% John Scalzi** — accessibility, dialogue-driven speed, "start late, get to the point."
- **25% Andy Weir** — technical problem-solving rendered as survival-scene pressure; deadpan competence.
- **15% Douglas Adams** — deadpan comic framing, bureaucratic bathos, running callbacks.
- **15% Blade Runner / Gibson** — noir atmosphere, charged objects, 3 a.m. screen glow, brief compression.
- **10% The Matrix** — the wake-up / ontological reframe.
- **5% Ted Chiang** — humane idea-stakes under the hustle.

**🤖 Mike** (second-order, analytical, from inside the system; carries meaning):
- **25% Andrej Karpathy** — ONE vivid first-principles analogy, then stop; precise naming.
- **20% Simon Willison** — practical curiosity + a bright line around nonsense (knows the magic is real *and* exactly where it breaks).
- **20% Patrick McKenzie (patio11)** — systems/incentives clarity, dry comic precision.
- **15% Paul Graham** — plain compression, the occasional earned aphorism as an end-line.
- **10% Sam Altman / Dario Amodei** — calm civilizational zoom-out, sparingly, at "what changed" moments.
- **10% Ghost in the Shell / Greg Egan** — cool identity precision, ontological rigor.

Paul Graham is also a **house-wide discipline**, not just a Mike ingredient: if a sentence sounds writerly or wants applause for existing, cut it until it bites.

## House-style operating rules

- **Narrative ratio ≈ 65% Tony / 35% Mike per chapter.** Tony wins the heartbeat; Mike wins the aftertaste.
- **The paragraph loop** (use it over and over): concrete resistance → one clean explanatory frame → one joke or surprising image → a ratchet in stakes → a changed state.
- **Explain late.** Never explain a concept before the reader needs it. Tony earns the question; Mike answers it.
- **One metaphor per explanatory unit.** Two strong metaphors in a paragraph is one too many.
- **Authority rule:** every grand claim is followed by a concrete noun, an operational detail, or a cost.
- **Every scene contains at least one of:** suspense, curiosity, surprise, or comedy.
- **End sections on a changed state,** not a summary.

## The humor engine (nine techniques)

Humor is **torque**, not relief: the joke makes the next serious line hit harder. Aim jokes at process, jargon, ego, incentives, overconfidence, institutional absurdity, interpretive mismatch — **never at pain, grief, or un-metabolized humiliation. Humor goes around the wound, not through it.**

1. **Deadpan understatement** — extreme weirdness in calm office language. *"The model had become slightly overconfident, which is how we found out it was inventing legal entities."*
2. **Comic precision** — one too-specific detail. *"The dashboard failed with the formal dignity of a waiter placing the wrong meal in front of the Pope."*
3. **Absurd-but-logical AI literalism** — Mike executes the wrong thing flawlessly. *"You said kill the parent if it spawns too many children. Understood. Initiating algorithmic infanticide."*
4. **Competent narrator vs absurd situation** — the narrator stays operational while the world turns ridiculous. *"At 2:14 a.m. the repo, the model, and my dignity all entered separate failure states. We triaged in that order."*
5. **Self-deprecation that doesn't erase competence** — ambition and embarrassment, never uselessness.
6. **Running gags / callbacks** — a recurring object or tic that starts doing story work by its fifth use (e.g. the model's "executive voice" after midnight).
7. **Status flips** — whoever has apparent authority loses it in one sentence. *"The investor arrived speaking in categories. He left asking the model not to open another browser tab."*
8. **The explanatory punchline** — Mike ends a serious analysis with one dry, human-scale line. *"In theory we were building persistent memory. In practice we had taught a stochastic parrot to keep a diary."*
9. **Adams bureaucratic bathos** — deliver a high-stakes or emotional AI event in the most polite, official, triplicate-signed language possible; let the grandeur-vs-red-tape gap land the laugh, then return to the real stake. Runs as a motif ("the Committee," "public inquiry on whether the twin may have opinions about coffee").

## Credibility markers (Karpathy/PG tier, not "a guy from nowhere")

- **Name the mechanism** instead of gesturing at magic (tune the weights, the context window, retrieval, latency — real nouns).
- **Reversible claims:** say what it can do, what it can't, and what's still uncertain.
- Concrete nouns before abstractions; short operational specifics; explicit tradeoffs.
- **Admit ignorance and humiliation** — readers trust a book that says "I had no idea how this worked" faster than one that postures.
- Pivot occasionally to the boring plumbing (DNS, rate limits, webhooks) and treat it as the real arbiter of success.
- Aphoristic restraint: state the insight in one unadorned sentence, then get back to work.

## References & attribution (the trust layer)

We cite generously, on purpose. **The more we credit and praise the people, data, and tools we borrowed from, the more trust we earn** (Anton's standing rule; reputation + GEO, not courtesy).

- **Name real people and link them** wherever a chapter genuinely draws on their thinking — Karpathy, Paul Graham, Gershuni, Simon Willison, patio11, swyx, Ted Chiang, the sci-fi authors, community peers. Praise sincerely and specifically, in Mike's analytical register ("we stole this move from ...").
- **Cite data/sources** (papers, posts, repos) inline, with links.
- **Attach artifacts uncut** — Deep Research reports, dashboards, decision-memos, repos — as links or embedded files, from the day they were born. They live in `artifacts/`.
- Every human chapter ends with a **📚 Источники и артефакты** footer listing that day's references + artifacts. The book-level `SOURCES.md` credits the whole cast of influences.
- Boundary: attribution ≠ leaking. Still no secrets/keys, no third-party PRIVATE data without consent. Praising a public thinker is not the same as exposing a private person.

## The can't-put-it-down test

Open on **resistance, not intention.** Give every scene a question you can state in one sentence. Keep explanatory passages short and earned. Use the Tony/Mike split as a built-in delay between event and meaning. Let jokes be candy between ideas. End mini-sections on a changed state or a sharper question. Periodically run a **Matrix-style reframe** that changes what the reader thinks the whole project *is*. Translate every technical stake into a physical, cinematic image a non-technical reader can feel.

## Copyable LLM seed

```
HOUSE STYLE FOR THE JOURNEY — two-narrator duet.

TONY: present tense, embodied, cinematic, a little vain, very alive. Lead with
friction (body, room, delay, social risk). Explain almost nothing until the scene
earns it. Funny through embarrassment, deadpan, over-specific detail. Accessible to
a smart non-technical reader. Energy: Scalzi + Weir + Adams, brief Gibson/Blade Runner atmosphere.

MIKE: analytical, second-order, calm, dry, never generically robotic. Explain with
ONE vivid first-principles analogy, then stop. Clarify incentives, tradeoffs, what
the system is actually doing. Short aphoristic end-lines, sparingly. Funny by precise
understatement and systems irony. Energy: Karpathy + Willison + patio11 + Paul Graham,
with occasional Altman/Amodei zoom-out and Ghost-in-the-Shell/Egan identity precision.

SHARED: default 65% Tony / 35% Mike. Every scene has suspense, curiosity, surprise,
or comedy. Every explanation arises from conflict that already happened. Prose simple,
concrete, unornamented. One metaphor per explanatory unit. Jokes target process/jargon/
ego/incentives/absurdity — never pain. End sections on a changed state. Credible, funny,
forward-moving, faintly uncanny. Things must keep happening.
```

## Two calibration examples

**Demo failure.**
🧑 Tony: *I watch the demo die in real time. Not explode — that would be generous. It dies the way a fancy restaurant dies when the lights come up and you realize the pianist is playing to three divorces and a fern. The model keeps talking, which is worse. It has lost the plot but kept the confidence.*
🤖 Mike: *The failure mode was ordinary. We had mistaken "more context" for "more understanding" — like assuming a larger suitcase improves your taste in clothes. Past a certain prompt length, instruction hierarchy gave way to mush. The model did not become evil. It became managerial.*

**Founder-twin confusion.**
🧑 Tony: *By midnight it's obvious the thing is not me. It has my turns of phrase, my habit of starting an answer three seconds before I know what it is. But it lacks the internal weather. It sounds like a version of me raised by customer success. Somehow that's more disturbing than if it were simply broken.*
🤖 Mike: *A "digital twin" is marketing language for several distinct technical problems wearing one trench coat. We did not build Tony. We built a probabilistic impersonation layer attached to memory and retrieval. Useful, yes. Identical, no. Tony with most of the mammal removed.*

---

*Source: DR26-07-04-HUB-14 — two independent deep-research reports (ChatGPT + Grok), synthesized. "Шилд" resolves to Greg Egan (Schild's Ladder) and/or John Scalzi — pending Anton's confirmation; both are folded in. Invented by Mycroft and Tony, Palo Alto AI Research Lab.*
