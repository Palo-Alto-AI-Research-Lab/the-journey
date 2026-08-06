---
source: grok.com (Grok Heavy, рой агентов)
dr_id: DR26-08-06-HUB-01-1055
topic: "ТРИЗ для агентных/инфраструктурных задач + пересечение с АК-47"
date: 2026-08-06
origin: fan-out DR26-08-06-HUB-01-1055, забор с хаба HUB-1
chat: https://grok.com/c/<chat-id>
evidence_of_work: "615 sources · Thought for 2m 6s · Heavy"
collected: 2026-08-06 15:2x Lisbon
real_author: Grok (xAI)
note: сохранён ответ verbatim; эхо-промпта нет (промпт лежит в scratchpad/DR26-08-06-HUB-01-1055-triz-grok.txt)
---

Thought for 2m 6s

TRIZ is a structured heuristic toolkit derived from patent analysis, useful for forcing explicit contradiction formulation in recurring failure classes, but it has a weak controlled evidence base, a certification industry, and a built-in tension with radical simplicity doctrines like the AK-47 principle. Partial adoption of a minimal subset (contradiction + IFR + trimming) can be coherent for a two-person non-engineer lab; full classical TRIZ or ARIZ is not. The honest verdict is that it is not a superior replacement for hypothesis-driven debugging, TOC, or STAMP-style analysis for your failure modes, but a lightweight ritual around contradiction and ideality can reduce symptom-patching loops without adding much mechanism.

Overall confidence: [emerging] for transfer to agentic AI/ops (thin measured outcomes); [established] for classical instruments and weak scientific validation; [speculative] on novelty of solutions for your exact contradictions.

## A. What TRIZ actually is, stripped of mythology

### A1. Core instruments (INPUT → OUTPUT, problem shape)

Classical TRIZ (Altshuller, USSR, patent-derived) models inventive problems as contradictions rather than optimization trade-offs. (Wikipedia)

- **Technical contradiction + Contradiction Matrix** (39 parameters × 40 inventive principles): INPUT = "improving parameter A (from the 39: weight, speed, reliability, complexity, etc.) worsens parameter B." OUTPUT = 1–4 suggested inventive principles from the matrix cell. Problem shape: paired trade-offs. The matrix is a lookup table from early patent analysis; later versions exist but the classical one remains the most referenced. (Agrifoundersfield)
- **Physical contradiction + Separation principles**: INPUT = the same parameter must simultaneously have opposite values (must be hot and cold; independent and coupled). OUTPUT = separation in time, space, on condition, or between system and its parts. This is the deeper form; technical contradictions often reduce to it. (Metodolog)
- **IFR / Ideality ratio**: INPUT = current system description. OUTPUT = Ideal Final Result formulation ("the system does not exist, but its function is performed") and the ratio (useful functions) / (cost + harm). Guides elimination of parts while preserving function. (Iitb)
- **40 Inventive Principles**: Generic operators (Segmentation, Taking Out, Local Quality, Asymmetry, Merging, Universality, Nesting, Anti-weight, Prior counter-action, Prior action, Cushioning, Equipotentiality, The Other Way Round, Spheroidality, Dynamics, Partial/excessive action, Another dimension, Mechanical vibration, Periodic action, Continuity, Skipping, "Blessing in disguise," Feedback, Intermediary, Self-service, Copying, Cheap short-living objects, Mechanics substitution, Pneumatics/hydraulics, Flexible shells, Porous materials, Colour change, Homogeneity, Discarding and recovering, Parameter changes, Phase transitions, Thermal expansion, Strong oxidants, Inert environment, Composite materials). Transferable to software/ops via analogy (e.g., segmentation → modular services; feedback → monitoring loops; cheap short-living → ephemeral containers). Many are post-hoc labels for solutions already found. (Qualitygurus)
- **Laws/trends of engineering system evolution**: S-curve of maturity, increasing ideality, transition to super-system, trimming, dynamization, increasing controllability, uneven development of parts. INPUT = current system stage. OUTPUT = predicted next evolutionary moves or trimming candidates. (Iitb)
- **Substance-Field (Su-Field / vepol) analysis + 76 Standard Solutions**: INPUT = incomplete or harmful interaction model (two substances + a field). OUTPUT = one of 76 standard transformations (build/complete the Su-Field, introduce intermediary, transition to micro-level, etc.). (Wiki.matriz)
- **ARIZ**: Full multi-part procedure (ARIZ-85C is common) that forces problem model → resources → IFR → physical contradiction → solution. INPUT = ill-defined inventive problem. OUTPUT = resolved concept. Almost no one outside classical mechanical specialists completes the full algorithm in practice; it is lengthy and specialist-oriented. (Aitriz)
- **Function Analysis / Trimming, CECA, Nine Screens (system operator), Size-Time-Cost operator, Smart Little People**: Supporting tools. Function Analysis maps useful/harmful functions for trimming (remove component, transfer its function). CECA builds cause-effect chains. Nine Screens examines system/sub/super across past/present/future. Smart Little People is a psychological inertia-breaker.

### A2. Classical vs modern derivatives

Classical TRIZ is Altshuller's public-domain core from Soviet patent analysis. Modern variants are commercial or simplified:

- **I-TRIZ** (Ideation International): adds Anticipatory Failure Determination and Directed Evolution; proprietary tooling. (Silo)
- **GEN-TRIZ** and similar: commercial training/consulting layers.
- **Systematic Inventive Thinking (SIT) / ASIT / USIT**: Israeli/US simplifications (fewer tools, "Closed World" principle, no large databases). Explicitly designed to be easier and faster to learn than classical TRIZ; USIT reorganizes TRIZ operators into five solution methods. (Wikipedia)
- **MATRIZ** (and MATRIZ Official): certification ecosystem (Levels 1–5, up to TRIZ Master). Active certification and training industry; does not "own" classical TRIZ but standardizes a modern curriculum. (Matriz-official)
- **ISO 69580:2025**: no solid primary source located linking it directly as a TRIZ standard in the retrieved material; treat as unverified or marketing-adjacent if claimed. [no source found for TRIZ-specific ISO]

Marketing vs substance: certification and software tooling are commercial layers; the core contradiction/IFR/trimming ideas remain free and classical.

### A3. Evidence base

The literature is dominated by self-reported consultancy case studies and patent-count claims. Controlled studies, RCTs, or rigorous before/after industrial measurements comparing TRIZ to brainstorming, TOC, DoE, or expert judgment are scarce to nonexistent for software/ops/reliability contexts.

Key points:

- A 2016 review of top-cited TRIZ publications states explicitly that "The development of the Theory of Inventive Problem Solving (TRIZ) has not followed the usual patterns of scientific validation required by engineering methods" and was "never supported by the traditional mechanisms of scientific validation." [established] (ScienceDirect)
- Matrix "hit rate" claims (principles suggested that match later patented solutions) hover around 48% in one student analysis of patents; updated matrices claim higher but are still retrospective. (The-trizjournal)
- NPD efficiency claims (e.g., ~10× vs traditional funnel from analysis of 161 consulting projects) come from authors involved in the projects → high bias risk. [single-source / self-reported] (Link.springer)
- Education quasi-experiments show short-term gains in creativity scores or insight-problem solving after TRIZ training; long-term organizational innovation impact is variable and weakly measured.
- Critiques: patent-sample selection bias (only successful inventions analyzed); 40 principles as post-hoc rationalization; matrix outdated for modern domains; ARIZ too cumbersome; limited applicability to complex/non-technical or socio-technical systems. (ScienceDirect)
- No strong head-to-head RCTs vs TOC, DoE, or expert debugging for the classes of failure you describe.

Consensus: useful structured heuristic, not a validated superior method. Dissenting (pro-TRIZ) view: the patent-derived patterns still accelerate ideation when the problem is truly inventive.

## B. TRIZ applied to software, agents and infrastructure

### B1–B2. Documented applications

Real applications with outcomes exist but are thin on hard numbers for software/IT/SRE/AI:

- **HP** (Kasravi, ~2010 TRIZCON): six IT cases (security vs openness, high CPU costs for financial transactions, private-cloud conflicts, leased-asset tracking, evolution of IT services/BI). Instruments: 9 Laws, 40 Principles, Ideality, Su-Field. Outcomes: solution concepts and at least one patent application (agent-based real-time processing on idle hardware); no quantified MTTR, defect rate, or cost savings reported. Self-reported. (Aitriz)
- **Samsung SAIT** (mid-2000s): 27+ IT/SW projects (algorithm and system architecture); self-reported success and financial contribution claims. Customized methodology. (Metodolog)
- **Oxford TRIZ / others**: software case studies exist but often confidential or illustrative (quadratic equations, mousetrap analogies); no public measured reliability metrics. (Triz)
- Sparse debugging/root-cause or pure SRE applications with numbers. Reliability literature occasionally maps Su-Field to fault models, but not operational MTTR data.
- **LLM/agent applications (2024–2026 prioritized)**: TRIZ-GPT (arXiv 2024) uses step-by-step prompting to map problems to TRIZ and generate solutions; case study in mechanical engineering showed resonance with original solutions. TRIZ Agents (arXiv 2025 / ICAART): multi-agent LangGraph system (specialized roles + TRIZ tools + optional RAG) on a gantry-crane case; produced logical solutions with partial overlap to human TRIZ case study; 150k–250k tokens per run; non-deterministic; research prototype with open GitHub. Usable today as a lab experiment, not production daily debugging. [emerging] (arXiv)
- No strong production evidence for TRIZ applied to LLM multi-agent design, prompt architecture, or agentic infrastructure reliability in 2024–2026 material retrieved.

Gap is real: literature lacks controlled ops/reliability metrics for TRIZ.

### B3. Transfer to your failure classes

Formulations (standard TRIZ method applied; no prior identical cases found):

**Watchdog independence vs internal state**
- Technical: improving monitoring reliability/independence worsens detectability of internal state. Matrix points toward Segmentation, Taking Out, Intermediary, Mechanics substitution (signals/fields).
- Physical: the monitor must be independent and coupled.
- Separation: space (different host — your existing rule), condition (external interfaces only), system/parts.
- IFR: the watched system self-reports health without an external watchdog existing.
- **Judgment**: largely relabels standard SRE external health-checks + independent host. Useful for forcing the independence rule; non-obvious move is limited. [speculative on novelty]

**Browser automation human-like vs unattended**
- Technical: improving human-likeness (profiles/cookies) worsens unattended reliability or labor.
- Physical: session must be human-present and human-absent.
- Separation: time (human sets up once), space (profile on one machine, proxy elsewhere), condition.
- IFR: automation authenticates and acts as human without persistent human or risky profile.
- **Judgment**: common solutions (persistent stealth profiles, residential proxies, session cookies) are what competent engineers already reach. TRIZ systematizes the trade-off.

**Fix delivery across offline / receive-only machines**
- Technical: improving completeness of deployment worsens dependence on simultaneous availability.
- Physical: machines must receive when online and when offline/receive-only.
- Separation: time (store-and-forward / queue), system/parts (pull or versioned mesh).
- IFR: fixes apply themselves when the machine becomes available without central writable push.
- **Judgment**: standard offline-first / Syncthing-style patterns. Relabel.

**Quiet indicator vs silence ≠ dead**
- Technical: improving quietness (no false alarms) worsens detectability of indicator failure.
- Physical: indicator must be silent when healthy and must signal aliveness.
- Separation: time (periodic heartbeat), condition (dual-path or negative confirmation).
- IFR: absence of signal means healthy and the channel is proven alive without continuous noise.
- **Judgment**: classic dead-man's-switch / heartbeat. TRIZ forces the contradiction into the open; solution is the one any SRE reaches.

Overall: the instruments (especially physical contradiction + separation + IFR) are good at preventing pure symptom patches and aligning with "fix root, not symptom." For these classes they mostly produce competent-engineer solutions rather than non-obvious inventions. Value is in the ritual of formulation.

### B4. Head-to-head comparisons

| Method | Beats TRIZ where | TRIZ beats it where | Learning cost |
|---|---|---|---|
| TOC (Evaporating Cloud) | Operational throughput, constraint focus in pipelines | Pure technical invention | Medium |
| Cynefin | Domain classification (complex vs complicated) | Technical contradiction resolution | Low–medium |
| Systems thinking / causal loops | Feedback dynamics, delays | Structured inventive principles | Medium |
| STAMP/STPA/CAST | Socio-technical emergent failures, control structures, multi-agent + human | Inventive parameter trade-offs | Higher |
| FMEA / FTA / 5 Whys | Known failure modes, speed, low cost | Novel inventive trade-offs when stuck | Low |
| Kepner-Tregoe / hypothesis-driven | Empirical debugging loops | Systematic contradiction elimination | Low |
| DoE | Quantitative optimization | Qualitative inventive leaps | Medium–high |
| Wardley | Strategic mapping / evolution | Detailed technical contradictions | Medium |

Sources are mostly complementary claims or conceptual overlaps rather than RCTs. TRIZ is strongest when the problem is a true inventive technical contradiction; weakest on cost and empirical validation for small ops teams. (ScienceDirect)

### B5. LLMs applying TRIZ

2024–2026 work (TRIZ-GPT, TRIZ Agents multi-agent, iPatent, function-model generation) shows LLMs can map problems to contradictions, retrieve principles, and generate candidate solutions. Worked: structured workflows + tools improve coherence over pure prompting. Failed/limited: non-determinism, high token cost, incomplete coverage of human case studies, prompt sensitivity, lack of real-world production debugging metrics. Usable today by a two-person lab as an experimental assistant (open prototypes exist); not a drop-in daily tool. [emerging]

## C. Collision with radical simplicity (AK-47 doctrine)

### C1. Divergence of ideality vs "dumbest repairable mechanism"

Ideality ("system disappears but function is performed") and AK-47 ("can a non-engineer repair it with hammer and screwdriver?") sound aligned but diverge on how the function is performed. TRIZ ideality often seeks elegant resource re-use or self-service that hides mechanism or increases coupling (e.g., using an existing but opaque internal state, or a clever intermediary field). AK-47 demands the mechanism remain visible, local, and mechanically simple even if less "ideal." Concrete case: a TRIZ-ideal watchdog that re-uses the system's own logging bus as a field may be more ideal (no extra component) yet less repairable by a novice than an independent external process that simply pings a public endpoint.

### C2. Over-engineering failure mode

Yes. Practitioner critiques note TRIZ can produce clever, highly optimized solutions that are hard to maintain. Related literature: Gall's Law ("A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works…"); "worse is better" / New Jersey vs MIT (simplicity of implementation beats elegant completeness); Chesterton's fence (do not remove apparent complexity until you understand why it exists); "clever code is unmaintainable." TRIZ's evolutionary trends and resource maximization can push toward the MIT-style elegance that Gall and Gabriel warn against. (Fasterthannormal +1)

### C3. Explicit split of the 40 principles (software/ops context)

**Adds complexity / coupling / hidden state** (refuse or use only with extreme caution under AK-47):
Nesting (7), Prior action / Prior counter-action (9–10), Cushioning in advance (11), Dynamics / Dynamization (15), Feedback (23), Intermediary (24), Self-service (25) when it embeds hidden logic, Mechanics substitution / fields (28), Phase transitions / parameter changes (35), Composite materials (40), and most uses of "blessing in disguise" that convert harm into new internal mechanisms.

**Removes complexity / mechanism** (adopt preferentially):
Segmentation (1), Taking Out / Extraction (2), Universality (6) when it eliminates parts, The Other Way Round (13), Cheap short-living objects (27), Discarding and recovering (34), Partial/excessive action (16) when it simplifies, and the trimming operator itself.

**Neutral / context-dependent**: Local Quality, Asymmetry, Merging, Anti-weight, Equipotentiality, Spheroidality, Periodic action, Continuity, Skipping, Copying, Colour change, Homogeneity, Porous, Thermal expansion, Inert environment, etc. Use only if the net result is fewer moving parts or clearer observability.

A simplicity-first shop should deliberately refuse the "adds" subset and treat the matrix suggestions that point to them as signals to re-formulate the contradiction rather than implement.

### C4. Trimming and increasing ideality

Yes, trimming + the law of increasing ideality is the closest internal analogue to AK-47. TRIZ literature presents trimming as transferring functions to remaining components or the super-system to raise the ideality ratio. It can make systems more fragile when the trimmed component carried latent robustness or when the transfer creates tighter coupling that is harder to diagnose. The literature itself focuses more on cost/harm reduction than on fragility trade-offs; the risk is real and under-emphasized. (Trizminsk)

### C5. Adoption cost and partial TRIZ

Real learning curve, specialized vocabulary, certification industry. For a two-person lab with a non-engineer owner, partial adoption (contradiction formulation + IFR + trimming only; ignore ARIZ, full Su-Field, matrix lookup) is coherent and is what many practical users actually do. Critics call pure "TRIZ-lite" ineffective; evidence is mixed — education studies show even short interventions help insight problems, while full ARIZ is rarely completed. Partial is usable; full classical is high-cost and low-ROI for your constraints. [emerging]

## D. What we should actually do

### D1. Named options

1. **Do not adopt TRIZ** — continue hypothesis-driven debugging + 5 Whys/CECA-style chains + explicit "fix root not symptom" + your existing independence rules. Cost: 0 new hours. Change: none. Measure (30 days): count of recurring failure classes that still require re-patching; abandon if that count does not drop under current methods. Strongest when failures are known modes rather than novel inventive contradictions.
2. **Minimal ritual only (recommended)** — contradiction + physical separation + IFR + trimming check, no matrix, no ARIZ, no certification. Cost: 4–8 hours to internalize the four questions. Daily change: on every recurring failure, force the written formulation before any code change. Measure: number of "green indicator / dead pipeline" or re-auth-loop incidents that are resolved at the contradiction level rather than patched; abandon if after 30 days the ritual produces only the solutions you would have reached anyway and adds friction.
3. **SIT/USIT-lite** — adopt the simplified Closed-World / five-operator version instead of classical TRIZ. Cost: 8–15 hours. Slightly more structure, still far lighter than ARIZ. Measure same as #2.
4. **Experimental LLM-TRIZ assistant** — stand up the open TRIZ-Agents prototype or a simple contradiction-prompting workflow for one failure class. Cost: 10–20 hours setup + tokens. Measure: quality of generated candidates vs human baseline on two real incidents; abandon if token cost or non-determinism outweighs insight.

### D2. Exact minimal ritual (one page, non-engineer language)

On any recurring failure:

1. Write the technical contradiction: "When we improve X, Y gets worse."
2. Rewrite as physical contradiction: "The thing must be A and not-A at the same time."
3. Ask the separation questions: Can we separate A and not-A in time? In space (different machine)? On condition? Between the whole and a part?
4. State the IFR: "What would the ideal look like if the problematic component did not exist but the needed function still happened?"
5. Trim check: Is there a component whose only job is fighting the symptom? Can its useful function be given to something already present (or to the super-system) so we can delete it?
6. Only then implement the simplest change that satisfies the separation or IFR. Prefer external, visible, hammer-and-screwdriver mechanisms.

**Demonstration on the quiet-indicator contradiction:**
- Technical: improving quietness worsens ability to detect a dead indicator.
- Physical: the indicator must be silent (healthy) and must make noise (to prove it is alive).
- Separation in time: emit a cheap periodic heartbeat; silence between beats = healthy.
- IFR: absence of signal means healthy and the channel is proven alive without continuous noise.
- Trim: delete any internal "I'm alive" agent that shares fate with the watched process; put the heartbeat on an independent host that simply observes an external endpoint.
- Result: standard external heartbeat — the competent-engineer solution, reached faster and with explicit refusal of shared-fate designs. TRIZ did not invent it; it forced the contradiction into the open so the shared-fate temptation was refused.

### D3. When TRIZ is the wrong choice + strongest argument against the recommendation

TRIZ is wrong if: (a) your failures are already well-diagnosed known modes (use 5 Whys/FTA), (b) the non-engineer owner cannot internalize even the four questions without friction, or (c) the ritual starts producing "clever" resource-reuse designs that increase hidden state.

Strongest argument against even the minimal recommendation: the evidence base is weak, the solutions for your concrete classes are the ones any competent SRE already knows, and any new vocabulary or ritual is itself a complication that violates the AK-47 test. The safest move is to stay with hypothesis-driven debugging + explicit independence and root-cause rules; the contradiction ritual is optional scaffolding, not a required upgrade.

**Single recommendation**: adopt the minimal ritual (#2) for 30 days on the four recurring classes, measure whether the number of symptom-only patches drops, and abandon without sunk-cost if it does not. Confidence: [emerging]. Main assumption: the act of writing the physical contradiction and IFR is cheap enough that it does not itself become a new source of complexity for a non-engineer owner.

615 sources
