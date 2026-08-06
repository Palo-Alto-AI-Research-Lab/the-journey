06.08.2026, 15:24 UK

# DR26-08-06-HUB-02-1055

## Executive verdict

Your rule has a strong core, but it should not be hardened as written.

- [established] Standalone linear 5 Whys is actively misleading for recurring failures in a distributed software/AI system. Choose **(c): replace it as the investigative method**. Keep “ask why again” only as a teaching prompt or as one branch inside a larger causal map.
- [established] The established industry family is **ITIL Problem Management + FRACAS/8D-style corrective-action closure + periodic cross-incident review**.
- [established] “Three incidents” has no statistical or industry basis. It is acceptable only as a cheap alarm to open a hypothesis record.
- [established] Recurrence must not replace severity, novelty, data-loss, security, monitoring-failure, or error-budget triggers. Run these triggers in parallel.
- [emerging] Use a multi-cause, evidence-tagged **Series Problem Review**, with an independent challenger and a discriminating test.
- [established] Do not close the problem because 30 calendar days passed without another report. Close it only after sufficient exposure, a canary, a before/after test, and an effectiveness window.
- [emerging] Propagate only to machines that share the relevant mechanism, after a canary. Blind fleet-wide propagation can manufacture a common-mode failure.

The precise recommendation is in D2.

---

# A. The critique of 5 Whys

## A1. Substantive criticisms

### Single-path bias

[established] The basic 5 Whys form forces the investigator to choose one next “because.” That structure cannot properly represent parallel contributors, AND/OR combinations, feedback loops, common-mode conditions, or interactions.

Alan J. Card’s published demonstration produced a causal tree with 30 paths and more than 75 causal conditions; two 5-Why chains covered only two paths and under 3% of the possible interventions. This is a demonstration, not a controlled trial, so the percentage must not be generalized. [single-source] [Card, published online 2 September 2016; issue 2017](https://qualitysafety.bmj.com/content/26/8/671)

A 2025 review of 91 industrial 5-Why reports found:

- 48.4% lacked a valid cause-effect sequence;
- 22.0% found no root cause;
- 39.6% eliminated their stated root cause;
- 34.1% completed the full cause → correction → verification cycle.

That is one manufacturing sample, not distributed software, but it demonstrates how badly the method can degrade in normal practice. [single-source] [Al-Rifai, 14 January 2025](https://doi.org/10.1108/IJLSS-04-2024-0077)

The IAEA likewise warns that 5 Whys can isolate one cause where several exist and may fail to identify generic causes shared across events. [single-source] [IAEA-TECDOC-1756, 2015](https://www-pub.iaea.org/MTCD/Publications/PDF/TE-1756_web.pdf)

**Verdict:** it is not merely “sometimes oversimplified.” The linear form creates the oversimplification.

### Stopping-point arbitrariness

[established] There is no scientific justification for five. Even pro-5-Whys guides permit more or fewer questions.

Investigators actually stop where one of these boundaries appears:

- available knowledge ends;
- evidence runs out;
- a controllable intervention is found;
- the cause crosses an organizational boundary;
- time or money runs out;
- the answer becomes politically inconvenient.

Leveson documents how event-chain investigations stop at the first controllable, information-supported, or politically acceptable point, including at an operator whose action is easier to name than upstream organizational conditions. [Leveson, *Engineering a Safer World*, 2011](https://maritimesafetyinnovationlab.org/wp-content/uploads/2021/04/Engineering_a_Safer_WorldNancyLeveson.pdf)

Peerally, Carr, Waring and Dixon-Woods call the endpoint a “cause of mutual convenience”: investigation teams can omit conditions outside their remit, preserve relationships, or satisfy deadlines and governance needs. [Peerally et al., online 23 June 2016; issue May 2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC5530340/)

An 18-month ethnography in two NHS hospitals found RCA serving three competing purposes: learning, governance and restoration of organizational legitimacy. In at least two observed cases, causal evidence was used to justify changes already desired. [single-source] [Nicolini, Waring & Mengis, May 2011](https://doi.org/10.1016/j.socscimed.2011.05.010)

The narrower proposition that teams formally stop at a “not my team” boundary has no direct measured study I could find. `[no source found]` The broader remit/political-convenience effect is well supported.

### Investigator-dependence and non-reproducibility

[established for RCA generally; emerging for 5 Whys specifically] No controlled inter-rater study measuring reproducibility of 5 Whys itself was found. `[no source found]`

The adjacent RCA evidence is poor:

- In 300 hospital incident reports re-analyzed with PRISMA causal trees, exact agreement on root-cause descriptions was 54%, partial agreement 17%, and no agreement 29%. Agreement on the number of roots was only κ=.46. Classification improved when analysts were given an existing causal tree: κ=.63-.71 depending on taxonomy depth. [Smits et al., 19 June 2009](https://pubmed.ncbi.nlm.nih.gov/19542181/)
- WHO patient-safety taxonomy validation produced mean agreement around κ=.5; related systems ranged roughly .2-.7 depending on category clarity and training. [Rasmussen et al., 2013](https://pmc.ncbi.nlm.nih.gov/articles/PMC3607357/)
- Card notes that another team can generate a wholly different but plausible chain from the same incident. That is an analytical criticism, not a measured κ result. [single-source] [Card, 2016/2017](https://qualitysafety.bmj.com/content/26/8/671)

**Conclusion:** do not claim a measured reproducibility failure specific to 5 Whys. Claim instead that broader causal analysis is materially investigator-dependent, and that 5 Whys provides fewer safeguards against this than a multi-branch evidence ledger.

### “Human error” as a terminal answer

[established] Dekker, Hollnagel, Woods and Cook converge on the same point: “human error” describes an outcome or judgment, not the mechanism that produced it.

- Sidney Dekker contrasts the old view, where error is the cause, with the new view, where error is a symptom and the beginning of investigation. [Dekker, *Field Guide to Human Error Investigations*, 2002](https://www.routledge.com/The-Field-Guide-to-Human-Error-Investigations/Dekker/p/book/3318536892099)
- Richard Cook and David Woods distinguish the operator’s “sharp end” from upstream design, resource, management and policy constraints at the “blunt end.” [Cook & Woods, January 1994](https://psnet.ahrq.gov/issue/operating-sharp-end-complexity-human-error)
- Google SRE independently recommends changing automation and processes rather than trying to make humans infallible. [Google SRE Workbook, 2018](https://sre.google/workbook/postmortem-culture/)

“Agent failed,” “model hallucinated,” “operator clicked the wrong profile” and “human error” should therefore be treated alike: observations that open an investigation into context, controls, feedback and recoverability.

### Safety-II and STAMP: the stronger attack

[established as theory; emerging as evidence of superior outcomes] Safety-II and STAMP reject more than the number five. They reject the assumption that a complex socio-technical loss has one discoverable root.

Safety-II argues that success and failure commonly emerge from the same everyday performance adaptations under limited time, knowledge and resources. It studies normal work and performance variability, not only broken event chains. Importantly, the original white paper advocates combining Safety-I and Safety-II rather than abolishing conventional analysis. [Hollnagel, Leonhardt, Licu & Shorrock, September 2013](https://www.eurocontrol.int/sites/default/files/content/documents/nm/safety/safety_whitepaper_sept_2013-web.pdf)

STAMP models loss as inadequate enforcement of system constraints across a control structure. Components can operate as designed while their interactions, feedback delays, mental models or coordination still create a hazardous state. [Leveson, 1 April 2004](https://www.sciencedirect.com/science/article/pii/S728585449704990X)

CAST consequently asks:

- What losses and hazards existed?
- Which constraints should have held?
- Who or what controlled them?
- What actions, feedback or process models were inadequate?
- How did the control structure degrade?

It does not search for the deepest single node. [Leveson, CAST Handbook, 2019](https://psas.scripts.mit.edu$HOME?name=CAST_handbook.pdf)

The classic-RCA counterargument remains live: for a local, directly observable, linear mechanism, a causal chain may be enough. Neither Safety-II nor CAST has conclusively demonstrated superior recurrence reduction in controlled software studies. Their conceptual fit for complex systems is stronger than their comparative outcome evidence.

### Did Toyota really use it?

[established] Yes, but the internet version is inflated.

Taiichi Ohno genuinely described repeated “why” questioning and illustrated it with a linear machine-failure chain: fuse → lubrication → pump → shaft → missing strainer. The Japanese book appeared in 1978 and the English edition in 1988. [Ohno, 1978/1988; publisher edition 2019](https://www.oreilly.com/library/view/toyota-production-system/6242591910655/xhtml/11_Chapter02.xhtml)

Toyota’s 2014 annual report explicitly includes Five Whys, but inside a larger loop:

**genchi genbutsu → uncover issues → Five Whys → improvement → higher standards**. [Toyota Annual Report, 2014](https://www.toyota-global.com/pages/contents/investors/ir_library/annual/pdf/2014/ar14_e.pdf)

Current Toyota material still emphasizes:

- going to the real place;
- observing the actual process;
- confirming facts;
- understanding consequences;
- incremental kaizen;
- building abnormality detection into the machinery.

[Toyota UK, 25 July 2024; updated 6 July 2026](https://mag.toyota.co.uk/genchi-genbutsu/), [Toyota global TPS page, current; accessed 6 August 2026](https://global.toyota/en/company/vision-and-philosophy/production-system/)

No primary Toyota source was found proving the common claim that Sakichi Toyoda formally “invented 5 Whys in the 1930s.” `[no source found]`

No current Toyota source was found saying that a standalone five-line form is sufficient for complex investigations. `[no source found]`

**Correction to the folklore:** Toyota did use Five Whys. Toyota did not publish evidence that a five-row worksheet is a universal forensic method. In Toyota’s own material it sits inside observation, kaizen, countermeasures, testing and standardization.

## A2. Steelman defence

### “It is a communication and teaching tool”

[established] This is the strongest defence. Card himself treats it as a useful classroom tool: it teaches beginners that the first answer may be a symptom and that deeper conditions matter. [Card, 2016/2017](https://qualitysafety.bmj.com/content/26/8/671)

It can also compactly communicate one already-verified causal strand. Evidence that the format itself improves communication outcomes was not found. `[no source found]`

### “It is cheap and beats nothing”

[established for cost; speculative for superior outcome] It needs little training or infrastructure and may take minutes. The IAEA describes these practical advantages. [single-source] [IAEA, 2015](https://www-pub.iaea.org/MTCD/Publications/PDF/TE-1756_web.pdf)

No credible comparative trial of “5 Whys versus no analysis” was found. `[no source found]`

### “It works for simple linear failures”

[established as a suitable use case; outcome evidence thin] This is defensible when all of the following are true:

- the mechanism is local and observable;
- the causal sequence is genuinely linear;
- each link can be directly checked;
- there are no important interactions or parallel controls;
- the intervention can be tested immediately.

Ohno’s pump example fits. Browser profiles, authentication state, vendor sessions, Windows permissions, Syncthing, watchdogs and multiple autonomous agents generally do not.

### “Bad execution, not the method, is the problem”

[emerging] Better implementations require empirical checking rather than brainstorming. IHI now warns that there may be multiple roots and that different participants may answer differently. [single-source] [IHI, current page; accessed 6 August 2026](https://www.ihi.org/library/tools/5-whys-finding-root-cause)

This defence improves the method, but once it permits branching, multiple roots, evidence status, counterexamples and experiments, it has ceased to be ordinary linear 5 Whys. It has become a causal map.

## A3. Net verdict

**Choice: (c), actively misleading as the primary investigative method; replace it.**

Confidence:

- [established] Linear standalone 5 Whys is unsuitable as the RCA method for recurring failures in a complex distributed software/AI system.
- [established] It remains usable as a teaching device and as a prompt applied separately to branches whose mechanisms are already constrained.
- [emerging] A FRACAS-lite problem record plus a multi-cause evidence graph is the best cost/benefit replacement for this five-machine lab.
- [speculative] CAST-lite will add enough value to justify its cost on ordinary browser/authentication incidents. Reserve it initially for cross-machine, multi-controller and high-risk cases.

---

# B. Analysis over a series or class

## B1. Established names and disciplines

### ITIL Problem Management

[established] The closest IT name is **Problem Management**:

- Incident Management restores service.
- Problem Management manages the actual or potential causes of one or more incidents.
- Reactive Problem Management begins after incidents.
- Proactive Problem Management searches incident and operational data for patterns.
- A Known Error record stores understood conditions and workarounds.

[PeopleCert ITIL 4 Problem Management, date not shown; accessed 6 August 2026](https://www.peoplecert.org/browse-certifications/it-governance-and-service-management/ITIL-1/itil4-practices-problem-management-3688)

The distinction is real in doctrine but often degenerates into paperwork. A field study found limited proactive trend analysis, KEDB reuse and preventive action in operational practice, with weak categorization among the obstacles. [single-source] [Marrone et al., 2017](https://www.redalyc.org/journal/2032/444639356884/)

### Google SRE

[established] Google operates at both levels:

1. single-incident postmortems;
2. structured aggregation and trend analysis across the repository.

Google’s tools extract postmortem metadata for search, reporting and cross-product theme identification. [Google SRE, 2016](https://sre.google/sre-book/postmortem-culture/), [Google SRE Workbook, 2018](https://sre.google/workbook/postmortem-culture/)

Google’s example error-budget policy does not use a count of three:

- one incident consuming more than 20% of a four-week error budget triggers a postmortem;
- one outage class consuming more than 20% of a quarterly budget becomes a P0 planning item.

Those figures are an example internal policy, not a universal statistical law. [single-source] [Google error-budget policy, 19 February 2018](https://sre.google/workbook/error-budget-policy/)

Error budgets identify where reliability attention is economically justified. They do not prove that incidents consuming the same budget share a mechanism.

### Learning From Incidents

[emerging] The Allspaw, Jones/Jeli, Shortridge, Reed and Verica lineage emphasizes rich incident narratives, local rationality, everyday work and thematic learning.

- John Allspaw argues that incident count, MTTR and closed-action-item totals are “shallow” indicators and not proof that learning occurred. [low-authority] [Allspaw, 20 November 2019](https://www.adaptivecapacitylabs.com/2019/11/20/markers-of-progress-incident-analysis/)
- Nora Jones says cross-incident themes are valuable only when the individual incident reviews are rich enough; otherwise aggregation can amplify bad labels. [low-authority] [Jones, 16 November 2022](https://www.heavybit.com/library/video/your-incident-response-playbook)
- Kelly Shortridge applies the Safety-II view to security incidents and rejects human error and single-factor explanations. [low-authority] [Shortridge, 27 July 2021](https://www.darkreading.com/cyber-risk/how-do-i-let-go-of-human-error-as-an-explanation-for-incidents-)
- J. Paul Reed’s practitioner work examines how blame-heavy and closure-oriented language obstructs organizational learning. [low-authority] [Reed, 2017](https://devopsdays.org/events/2017-atlanta/program/j-paul-reed)

I could not identify a relevant “Rehr” in the LFI lineage. `[no source found]` I believe the intended name may be **J. Paul Reed**, but that is an inference.

LFI’s scepticism is not “never aggregate.” It is: do not count vaguely bounded incidents and call the resulting chart learning. Aggregate mechanisms, coordination patterns, dependencies, surprises, enabling conditions and exposure.

### Reliability engineering

[established]

- **FRACAS** is the most direct formal analogue: report failures, classify them, analyze causes, assign corrective actions, verify effectiveness, retain history and prevent recurrence. [US DoD MIL-HDBK-2155, 11 December 1995; validated 19 July 2019](https://quicksearch.dla.mil/qsDocDetails.aspx?ident_number=207200)
- **8D** adds team formation, precise problem definition, containment, verified causes and escape points, permanent correction, validation and prevention of similar problems. Five Whys may be used inside D4, but it is not the whole method. [ASQ 8D, date not shown; accessed 6 August 2026](https://asq.org/quality-resources/eight-disciplines-8d)
- **Pareto analysis** ranks categories by frequency or impact. It selects where to investigate; it does not validate the categories or prove a shared cause. [ASQ cause-analysis tools, accessed 6 August 2026](https://asq.org/quality-resources/root-cause-analysis/tools)
- **Weibull and failure-rate trending** require homogeneous failure modes and exposure data. NIST cautions that fewer than five failures are often very inaccurate and fewer than ten make important parameter estimation difficult. [single-source] [NIST/SEMATECH reliability handbook, accessed 6 August 2026](https://www.itl.nist.gov/div898/handbook/apr/section3/apr312.htm)

### Aviation and medicine

[established] Safety-critical sectors create a large incident corpus, apply standardized multi-factor taxonomies, identify safety issues across reports, and maintain recommendations through closure.

- NASA ASRS collects confidential voluntary reports, produces alerts and makes de-identified records searchable. Voluntary report counts are not valid failure rates without exposure and reporting-propensity data. [NASA ASRS, current; accessed 6 August 2026](https://asrs.arc.nasa.gov/overview/summary.html)
- ICAO’s ADREP taxonomy provides shared incident terminology. [ICAO, current; accessed 6 August 2026](https://www.icao.int/safety/AIG/taxonomy)
- NTSB recommendations can arise from an individual investigation or a broader safety study. In 2024, analysis of more than 500 Part 135 accidents produced three new recommendations and reiterated two. [single-source] [NTSB, 13 August 2024](https://www.ntsb.gov/news/press-releases/Pages/NR20240813.aspx)
- WHO explicitly warns that incident-report data can support learning only when its sampling and reporting limitations are respected. [WHO, 16 September 2020](https://www.who.int/publications-detail-redirect/4658754625926)

## B2. Concrete formats and rituals

| Format | Artifact | Operator and cadence | Evidence verdict |
|---|---|---|---|
| Blameless postmortem | Impact, timeline, detection, recovery, contributing factors, evidence, uncertainties, actions with owner/priority/due date | Independent facilitator with responders; days after a qualifying incident | [established] Widely institutionalized; outcome evidence is mostly observational. |
| Periodic cross-incident review | Fixed window, complete incident set, exposure, multi-label tags, recurring mechanisms, disagreements, problem records and action ageing | Reliability/problem owner; monthly for this lab, quarterly for major funding decisions | [emerging] Supported as practice by Google, NHS and LFI; comparative recurrence evidence is missing. |
| Problem/Known Error record | Class definition, linked incidents, mechanism signature, evidence status, workaround, fix, test, owner and review date | Named problem owner; reviewed during new incidents and monthly for staleness | [established] Canonical ITIL structure; vulnerable to paperwork failure. |
| FRACAS/8D record | Failure → containment → verified cause(s) → correction → effectiveness check → horizontal deployment | Cross-functional owner until verified closure | [established] Strongest recurrence-focused administrative loop; evidence that the format itself beats alternatives is thin. |
| CAST | Losses, hazards, control structure, constraints, feedback, mental models and systemic recommendations | Independent analyst with operators/controllers; escalate for complex or high-risk classes | [established] CAST is documented for incidents; a formally validated “CAST over a class” method was not found. `[no source found]` |
| FTA | Defined top event, AND/OR causal combinations, cut sets and controls | Reliability or security owner; at design/change and after new modes appear | [established] Useful top-down forward complement. [NASA FTA Handbook, August 2002](https://everyspec.com/NASA/NASA-General/NASA_FTA_1--1_68/) |
| FMEA/FMECA | Component/function, failure mode, effect, cause, prevention/detection controls, risk and actions | Engineering + operations; living artifact updated at change reviews | [established] NASA explicitly treats it as a living risk document. [NASA GSFC-HDBK-8004, 20 August 2024](https://standards.nasa.gov/node/12367) |
| Fishbone/Ishikawa | Many possible causes grouped into categories | Facilitated brainstorming before evidence collection | [established] Good for candidate generation; not proof. [ASQ, reviewed October 2024](https://asq.org/quality-resources/fishbone) |
| Current Reality Tree | Multiple undesirable effects joined by explicit if/then relationships and assumptions | Facilitator with domain owners | [emerging] Better structure for several apparent symptoms, but it still tends to search for a core constraint and has weak comparative evidence. [single-source] [AHRQ technical review, January 2006](https://www.ncbi.nlm.nih.gov/books/NBK44036/) |
| Thematic coding | Versioned multi-label codebook, positive/negative examples, two coders, agreement and adjudication | Reliability analyst plus independent coder; monthly/quarterly | [established] Makes classification disagreement visible; does not itself prove causality. |
| Retro-of-retros / quarterly reliability review | Top classes by exposure-adjusted loss, disputed classes, overdue actions, failed fixes and funded next work | Owner plus decision-maker; quarterly | [emerging] Google supports quarterly class-level planning. A separately validated “retro-of-retros” ritual was not found. `[no source found]` |

The clean division is:

- **Postmortem:** understand one incident.
- **Series review:** test whether a repeated class exists.
- **FTA/FMEA/CAST:** model mechanisms and controls.
- **Problem record/FRACAS/8D:** drive changes to verified closure.

## B3. How to know whether the series is one class

[established] Surface similarity is insufficient.

Microsoft’s linked-incident research found that related incidents can have dissimilar text, while text- or component-only similarity can also join unrelated incidents; better approaches combine text with component and dependency structure. [Microsoft LiDAR, November 2020](https://www.microsoft.com/en-us/research/publication/identifying-linked-incidents-in-large-scale-online-service-systems/)

Azure postmortem research on more than 2,000 incidents found that single-root and ad-hoc labels were inadequate for multi-factor incidents. [single-source] [Microsoft AutoARTS, July 2023](https://www.usenix.org/conference/atc23/presentation/dogga)

### Operational class-admission test

[emerging] This is a synthesis, not a published universal standard:

1. **Retrieve by symptom; classify by mechanism.** “Browser problem” may retrieve candidates, but cannot be the class definition.
2. For every candidate, record independently:
   - initiating condition;
   - failed or absent control;
   - component and boundary;
   - environment/profile/account;
   - observable technical signature;
   - detection failure;
   - exposure opportunity.
3. Allow multiple labels. Do not force each incident into one bucket.
4. Two analysts classify blindly before seeing the preferred fix.
5. Add two controls:
   - a same-looking incident with a known different mechanism;
   - a different-looking incident containing the suspected mechanism.
6. Admit the class only if one proposed mechanism or control gap predicts all members and excludes the negative control.
7. Write the falsifying observation before implementing the fix.
8. Confirm with a reproduction, canary or intervention.

For small N, calculate and display raw agreement rather than pretending κ is stable. When enough tagged incidents accumulate, use inter-rater statistics.

**AK-47 test:** if the proposed “common root” also explains the deliberately unrelated browser incident, it is a story, not a class.

## B4. Minimum series size

[established] No universal evidence-based minimum exists. Three is arbitrary as a statistical threshold.

- Standard control-chart guidance normally needs roughly 20 stable sequential observations to establish limits. [ASQ, current; accessed 6 August 2026](https://asq.org/quality-resources/control-chart)
- Weibull estimation with fewer than five failures is often very inaccurate; ten or more is a more defensible starting point for some parameter estimates. [NIST, accessed 6 August 2026](https://www.itl.nist.gov/div898/handbook/apr/section3/apr312.htm)
- Google uses impact/error-budget thresholds, not raw recurrence counts.
- Safety-critical practice allows one novel or high-consequence case to trigger deep analysis.

Therefore:

- **Second occurrence:** open or link a candidate problem record.
- **Third occurrence:** mandatory cheap Series Problem Review.
- **Three does not prove a class.**
- **One severe, novel, security, data-loss or observability failure:** analysis immediately.
- For statistical trend claims, wait for adequate observations or run designed reproduction tests.

Three is acceptable as a workflow trigger precisely because the analysis is cheap. It is not evidence.

## B5. Does class-level analysis reduce recurrence more?

[speculative] No credible head-to-head study was found comparing class-level review with equally high-quality per-incident analysis. `[no source found]`

The available evidence is weaker:

- Google shows that structured aggregation finds cross-product themes and priorities, but provides no controlled comparison for aggregation itself.
- NTSB shows that multi-event safety studies generate systemic recommendations, not that the format outperforms individual investigations.
- A 2020 healthcare systematic review found only two of 21 RCA studies could establish safety improvement even “to some extent”; weak recommendations were a common failure. [Martin-Delgado et al., 15 May 2020](https://pubmed.ncbi.nlm.nih.gov/32417837/)
- A 43-study review found little evidence that incident-reporting systems produced improved outcomes or double-loop learning. [Stavropoulou et al., 2 December 2015](https://pmc.ncbi.nlm.nih.gov/articles/PMC4678941/)

So:

- Industry adoption of class review: [established].
- Ability to expose repeated mechanisms: [established] as a descriptive capability.
- Superior recurrence reduction: [speculative].
- Rationality of class review plus falsifiable corrective-action closure for this lab: [emerging].

---

# C. AI-agent and LLM operations, 2025-2026

## C1. Existing taxonomies

[emerging] The field is no longer empty, but it is young, fragmented and dominated by benchmarks and preprints.

| Work | Findings | Limitation |
|---|---|---|
| Hasan & Biswas, ASE 2026 | 16,586 GitHub issues screened; 547 manually confirmed coding-agent safety failures; 33 risks including fabrication, false assurance, contextual forgetting, authorization bypass, destructive actions and environment corruption | Failure-only GitHub sample; cannot estimate incidence rates. [single-source] [29 May 2026, revised 4 August](https://arxiv.org/abs/2605.30777) |
| Shah et al. | 385 faults from 13,602 issues/PRs; 37 fault types and 12 root-cause categories, including tool integration, dependencies, validation, runtime environment, tokens and authentication | Public OSS corpus, recent preprint. [single-source] [6 March 2026](https://arxiv.org/abs/2603.06847) |
| MAST | 1,642 multi-agent traces; 14 modes covering system design, inter-agent misalignment, verification and termination; human κ=.88 | Benchmark traces, not production. [single-source] [17 March 2025](https://arxiv.org/abs/2503.13657) |
| AgentRx | 115 failed trajectories; nine categories including plan adherence, invented information, invalid invocation, tool-output misinterpretation, intent/plan mismatch, guardrail and system failures; +23.6 points localization and +22.9 attribution over prompting baselines | Small benchmark; not a long-running fleet. [single-source] [Microsoft, 12 March 2026](https://www.microsoft.com/en-us/research/blog/systematic-debugging-for-ai-agents-introducing-the-agentrx-framework/) |
| OpenRCA | 335 real enterprise software failures and 68GB telemetry; best model plus RCA agent fully solved only 11.34% | Cloud software RCA, not agent self-analysis. [single-source] [ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d29b8d65716093994e1d245c023e49d2-Abstract-Conference.html) |
| CUADebug | 204 failed computer-use trajectories; failure families included reasoning/control, perception, grounding/interaction and external/system failures; joint subtype+step diagnosis reached 19.6% | Six-day-old preprint and benchmark. [single-source] [31 July 2026](https://arxiv.org/abs/2608.02643) |
| False-success study | 9,876 τ2 and 1,879 AppWorld trajectories; LLM judges reached at most AUROC .65 and .54, while simple TF-IDF detectors reached .83 and .95 | Very recent single-author preprint. [single-source] [1 June 2026](https://arxiv.org/abs/2606.09863) |

Mapping to your observed modes:

- silent no-op → false success, premature termination, plan-adherence failure;
- tool-call failure → invalid invocation, interface/input or system failure;
- context loss → contextual forgetting, token/memory management, synthesis collapse;
- hallucinated success → false assurance, invention of information, false success;
- stale credentials → token/authentication and session-state failure;
- green indicator/dead pipeline → false success plus missing external verification.

[speculative] No validated production taxonomy specifically covers long-running profile drift, cookie divergence, stale vendor sessions, re-login loops and cross-machine browser state as one family. `[no source found]`

Your local taxonomy would therefore be a legitimate extension, not an established standard.

## C2. Can an LLM analyze itself?

**Answer: not alone.**

[established] LLMs can usefully:

- normalize logs;
- extract events;
- retrieve analogous incidents;
- propose tags and causal hypotheses;
- draft postmortems;
- identify evidence gaps.

They cannot currently be trusted as the sole evidence judge, causal analyst and verifier.

Measured evidence:

- On Microsoft incidents, RCA agents achieved about 35-39% accuracy depending on strategy. Among incorrect answers, hallucination rates ranged from 6% to 49%; the lower-hallucination ReAct approach abstained much more often. [single-source] [Roy et al., 7 March 2024; FSE July 2024](https://arxiv.org/abs/2403.04123)
- RCACopilot reported root-cause-category accuracy up to .766, but this was constrained classification against historical handlers, not free-form discovery of causal truth. [single-source] [Microsoft/EuroSys, April 2024](https://www.microsoft.com/en-us/research/publication/automatic-root-cause-analysis-via-large-language-models-for-cloud-incidents/)
- A Microsoft study over more than 100,000 production incidents reported a 24.8% average relative improvement over fine-tuned GPT-3 and better human ratings, but its public report does not disclose the corresponding absolute accuracy. [single-source] [July 2024](https://www.microsoft.com/en-us/research/publication/automated-root-causing-of-cloud-incidents-using-in-context-learning-with-gpt-4/)
- OpenRCA’s best full-solve rate was only 11.34%, demonstrating how much performance falls when the model must reason over heterogeneous real telemetry. [single-source] [ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d29b8d65716093994e1d245c023e49d2-Abstract-Conference.html)

Specific failure modes:

1. [established] Models often repair an error when an external signal identifies its location, but struggle to locate their own reasoning error. [Tyen et al., August 2024](https://aclanthology.org/2024.findings-acl.826/)
2. [established] A critical survey found no convincing general success for prompted-only intrinsic self-correction, except unusually suitable tasks; reliable external feedback was the consistent enabler. [Kamoi et al., 2024](https://aclanthology.org/2024.tacl-1.78/)
3. [established] LLM evaluators can recognize and favor their own generations. [NeurIPS 2024](https://papers.nips.cc/paper_files/paper/2024/hash/7f1f0218e45f5414c79c0679633e47bc-Abstract-Conference.html)
4. [established] Agents can exhibit choice-supportive bias favouring decisions they made earlier, especially when they perceive themselves as in control. [AAAI, 11 April 2025](https://ojs.aaai.org/index.php/AAAI/article/view/34843)
5. [emerging] A fluent reasoning narrative is not a faithful causal trace; model explanations may omit influential signals. [single-source] [Anthropic, 3 April 2025](https://www.anthropic.com/research/reasoning-models-dont-say-think)

No direct study was found proving that an LLM investigator deliberately “lets itself off the hook.” `[no source found]` Self-preference, choice-supportive bias, sycophancy and mistake-location failure make the concern credible, but it must remain an inference.

## C3. Safeguards

Your requirement that every “because” is either evidenced or labelled a hypothesis is strongly consistent with the literature. It is necessary, but insufficient.

Add:

1. **Freeze evidence before interpretation:** raw transcript, tool inputs/outputs, timestamps, machine, model/version, context reductions, exit codes, diffs, browser URL/state, account/profile identity and downstream artifact freshness.
2. **Write a claim-evidence ledger first:** claim, evidence pointer, status, strongest rival, falsifying observation.
3. **Use a graph, not a prose chain:** initiating conditions, contributors, failed controls, detection/recovery conditions and feedback loops.
4. **Separate roles:** extractor → hypothesis generator → blind challenger → verifier. The investigated agent cannot be its final verifier.
5. **Force alternatives:** at least one agent/model explanation, one environment/interface explanation and one observability explanation where plausible.
6. **Permit “unresolved”:** forcing a root rewards fabricated coherence.
7. **Use negative controls and held-out cases.**
8. **Test the intervention:** remove/block the suspected condition and see whether the class-level symptom changes.
9. **Verify environment state, not the agent’s success statement.**
10. **Fix and test are owned separately:** the fixer must not weaken its own acceptance test.
11. **Count exposure:** three failures in three attempts are not equivalent to three in 30,000.
12. **Audit the taxonomy:** double-code a sample, preserve disagreement and revise categories.

[emerging] This combination is supported by the evidence-grounding, external-feedback and independent-evaluation findings above, but the exact procedure has not been evaluated as a package.

---

# D. What you should do

## D1. Point-by-point critique of your rule

| Current point | Verdict | Required correction |
|---|---|---|
| 1. Unit is the series, not the incident | **Partly supported, partly wrong.** [established] | The series is an additional level, not a replacement. High-quality incident records are the raw evidence from which the class is tested. |
| 2. Minimum three cases | **Arbitrary.** [established] | Use three only as a mandatory low-cost review trigger. Candidate record at two. One severe/novel/security/data-loss/monitoring failure overrides the count. |
| 3. Trigger is recurrence, not severity | **Contradicted.** [established] | Recurrence, severity, novelty, loss, monitoring failure and exposure-adjusted impact must run in parallel. Google and safety-critical practice do this. |
| 4. Fix in a separate dedicated session | **Reasonable governance; not evidence-based as a universal rule.** [emerging] | Contain the incident inline. Open a separate problem/fix session when the change crosses components, machines, ownership or requires independent testing. |
| 5. Every “because” proven or hypothesis | **Strongly supported.** [established] | Add evidence IDs, rival explanations, falsifying observations and `DISPROVEN/UNKNOWN`, not just proven/hypothesis. |
| 6. Fix closes the class via gate/test/rule | **Supported, but “rule” is weak.** [established] | Prefer elimination, automated constraint, invariant or direct observability. Training and written rules are lower-strength controls. Verify effectiveness per exposure. |
| 7. Propagate to all machines | **Unsupported and potentially dangerous.** [emerging] | Propagate only to compatible, exposed nodes. Canary first, then staged rollout with rollback. Fleet-wide simultaneous change creates common-mode risk. |

One additional flaw: the current one-sentence “root cause” requirement conflicts with the multi-cause reality of your system. Replace it with a one-sentence **problem mechanism summary**, followed by a causal graph.

## D2. One-page replacement procedure

### Series Problem Review, SPR-1

1. **Record each incident.** Save time, machine, task, symptom, impact, environment/account/profile, logs, tool results, changes, recovery and exposure opportunity.
2. **Contain it.** Restore service safely. Mark the workaround as temporary. Do not wait for the series review.
3. **Trigger the review.**
   - severe, novel, security, data-loss or monitoring failure: after one;
   - candidate recurring problem: after two;
   - mandatory SPR: after three;
   - additionally: when an outage class consumes a defined reliability budget.
4. **Freeze evidence.** Copy or hash raw traces before any analyst writes a story.
5. **Define the candidate class.** Name a suspected mechanism or failed control, not a symptom: “profile ownership lock not detected before browser launch,” not “browser broke.”
6. **Test membership.** Make a case × mechanism table. Use two blind coders. Include one same-looking negative case and one different-looking positive case where possible. Split the class if one mechanism does not predict every member.
7. **Build the causal map.** Record all material branches:
   - initiating conditions;
   - contributing/enabling conditions;
   - failed or missing controls;
   - detection and recovery gaps;
   - common-mode conditions.
   Every edge is `PROVEN`, `HYPOTHESIS`, `DISPROVEN` or `UNKNOWN`, with an evidence pointer.
8. **Challenge it.** Require the strongest rival explanation and one observation that distinguishes the two. Convenient conclusions receive an independent adversarial review.
9. **Choose the control.** Prefer, in order:
   - eliminate the condition;
   - enforce an automated invariant/gate;
   - detect and stop automatically;
   - improve direct observability;
   - workaround;
   - documentation/training only as last resort.
10. **Pre-register the test.** State before editing:
    - failure reproduced before fix;
    - expected pass after fix;
    - negative-control behavior;
    - rollback;
    - exposure count and effectiveness window.
    The fixer cannot weaken this test.
11. **Canary and propagate.** Test one relevant machine, then one different node type. Roll out only to nodes sharing the mechanism and verify each output.
12. **Close or split.** Close only when:
    - the causal hypothesis survived the rival/negative-control test;
    - the acceptance test failed before and passed after;
    - rollback is available;
    - all relevant nodes are verified;
    - the predeclared exposure target was reached without mechanism-specific recurrence.

**Explicit stop condition:** if evidence cannot distinguish the leading explanations, stop causal storytelling. Leave the record as **UNRESOLVED KNOWN PROBLEM**, retain the workaround, improve instrumentation, and wait for a discriminating observation. Never manufacture one root to complete the form.

## D3. Failure modes of this procedure

| Failure | Cheapest detector |
|---|---|
| Fake class from surface similarity | Same-looking negative case. If the rule includes it, split/reject the class. |
| Tidy retrospective story | Count unsupported causal edges; require evidence IDs and a rival explanation. |
| Analyst/agent self-exoneration | Blind second analyst receives evidence without the first conclusion. |
| Taxonomy drift | Recode a small fixed sample monthly and display disagreement. |
| Small-N overconfidence | Label N=3 as hypothesis generation; require reproduction or more exposure before rate claims. |
| “No recurrence” because nothing ran | Exposure counter per node/task; calendar silence does not count. |
| Fix verifies itself | Locked acceptance test owned by a different session or reviewer. |
| Paperwork with no operational change | Every review must have an owner, control, test or explicit parked decision. |
| Rule/training proliferation | Track control strength; require justification when automation or elimination was rejected. |
| Fleet rollout creates common-mode failure | Canary, compatibility matrix and staged rollout. |
| Endless investigation | Time-box hypothesis work; unresolved is allowed when no discriminating evidence exists. |
| One deep cause distracts from easy protection | Rank interventions by blocking power, testability, cost and blast radius, not causal “depth.” |

The cheapest overall fake-root alarm is:

> Can the proposed mechanism predict one held-out recurrence and exclude one surface-similar counterexample?

If not, it remains a hypothesis.

## D4. Options

### Option 1: Keep the current rule

Trade-off: cheapest and memorable, but preserves the three-case fiction, recurrence-only trigger, singular root and unsafe fleet propagation.

Risk: confident false classes and elegant but wrong one-line roots.

Confidence it is sufficient: [speculative], low.

### Option 2: “5 Whys++”

Keep the name but add branches, evidence states, rival explanations, class testing, exposure and canaries.

Trade-off: low migration cost, but the familiar name will keep pulling agents back toward a single chain and exactly five questions.

Risk: procedural drift back to the original method.

Confidence: [emerging].

### Option 3: Replace with FRACAS-lite / Series Problem Review

Use the D2 procedure for every qualifying recurrence.

Trade-off: slightly more work per review; much stronger classification, evidence and closure discipline.

Risk: paperwork unless the record remains one page and has a named owner.

Confidence: [emerging].

### Option 4: Tiered SPR + CAST escalation

- Default: FRACAS-lite Series Problem Review.
- Escalate to CAST-lite when the class crosses machines/controllers, involves feedback or monitoring failures, or carries security/data-loss risk.
- Use FTA/FMEA prospectively for critical automation and watchdogs.

Trade-off: best conceptual fit and strongest defences against fake roots; requires the team to recognize when escalation is justified.

Risk: CAST can become an expensive diagram ceremony.

## Recommendation

**Choose Option 4: tiered SPR + CAST escalation. [emerging]**

Use three cases only as the automatic cheap-review threshold, not as evidence. Preserve single-incident records. Run severity and recurrence triggers in parallel. Replace “one root cause” with an evidence-tagged causal map. Canary before targeted fleet propagation.

Main assumption: the lab can retain immutable traces and count meaningful exposure. Without reliable evidence and exposure, no method, human or LLM, can distinguish a common mechanism from a persuasive story.

---

## Source register

### 5 Whys, safety science and Toyota

1. [Card, “The problem with ‘5 whys’,” 2 September 2016 / 2017 issue](https://qualitysafety.bmj.com/content/26/8/671)
2. [Al-Rifai, industrial 5-Why report quality, 14 January 2025](https://doi.org/10.1108/IJLSS-04-2024-0077)
3. [IAEA-TECDOC-1756, 2015](https://www-pub.iaea.org/MTCD/Publications/PDF/TE-1756_web.pdf)
4. [Peerally et al., “The problem with root cause analysis,” 23 June 2016 / May 2017 issue](https://pmc.ncbi.nlm.nih.gov/articles/PMC5530340/)
5. [Nicolini, Waring & Mengis, policy and practice of RCA, May 2011](https://doi.org/10.1016/j.socscimed.2011.05.010)
6. [Smits et al., RCA inter-rater reliability, 19 June 2009](https://pubmed.ncbi.nlm.nih.gov/19542181/)
7. [Rasmussen et al., WHO incident taxonomy reliability, 2013](https://pmc.ncbi.nlm.nih.gov/articles/PMC3607357/)
8. [Dekker, *Field Guide to Human Error Investigations*, 2002](https://www.routledge.com/The-Field-Guide-to-Human-Error-Investigations/Dekker/p/book/3318536892099)
9. [Cook & Woods, “Operating at the Sharp End,” January 1994](https://psnet.ahrq.gov/issue/operating-sharp-end-complexity-human-error)
10. [Hollnagel et al., Safety-II white paper, September 2013](https://www.eurocontrol.int/sites/default/files/content/documents/nm/safety/safety_whitepaper_sept_2013-web.pdf)
11. [Leveson, STAMP accident model, 1 April 2004](https://www.sciencedirect.com/science/article/pii/S728585449704990X)
12. [Leveson, *Engineering a Safer World*, 2011](https://maritimesafetyinnovationlab.org/wp-content/uploads/2021/04/Engineering_a_Safer_WorldNancyLeveson.pdf)
13. [Leveson, CAST Handbook, 2019](https://psas.scripts.mit.edu$HOME?name=CAST_handbook.pdf)
14. [Ohno, *Toyota Production System*, 1978/1988; publisher edition 2019](https://www.oreilly.com/library/view/toyota-production-system/6242591910655/xhtml/11_Chapter02.xhtml)
15. [Toyota Annual Report, 2014](https://www.toyota-global.com/pages/contents/investors/ir_library/annual/pdf/2014/ar14_e.pdf)
16. [Toyota UK, Genchi Genbutsu, 25 July 2024; updated 6 July 2026](https://mag.toyota.co.uk/genchi-genbutsu/)
17. [Toyota global TPS page, accessed 6 August 2026](https://global.toyota/en/company/vision-and-philosophy/production-system/)
18. [IHI Five Whys guidance, date not shown; accessed 6 August 2026](https://www.ihi.org/library/tools/5-whys-finding-root-cause)

### Series analysis and review formats

19. [PeopleCert, ITIL 4 Problem Management, accessed 6 August 2026](https://www.peoplecert.org/browse-certifications/it-governance-and-service-management/ITIL-1/itil4-practices-problem-management-3688)
20. [Marrone et al., Proactive Management of IT Operations, 2017](https://www.redalyc.org/journal/2032/444639356884/)
21. [Google SRE, Postmortem Culture, 2016](https://sre.google/sre-book/postmortem-culture/)
22. [Google SRE Workbook, Postmortem Culture, 2018](https://sre.google/workbook/postmortem-culture/)
23. [Google SRE Workbook, Example Error Budget Policy, 19 February 2018](https://sre.google/workbook/error-budget-policy/)
24. [Allspaw, Markers of Progress in Incident Analysis, 20 November 2019](https://www.adaptivecapacitylabs.com/2019/11/20/markers-of-progress-incident-analysis/)
25. [Jones, incident playbook and themes, 16 November 2022](https://www.heavybit.com/library/video/your-incident-response-playbook)
26. [Shortridge, human error in security incidents, 27 July 2021](https://www.darkreading.com/cyber-risk/how-do-i-let-go-of-human-error-as-an-explanation-for-incidents-)
27. [J. Paul Reed, DevOpsDays, 2017](https://devopsdays.org/events/2017-atlanta/program/j-paul-reed)
28. [US DoD MIL-HDBK-2155 FRACAS, 11 December 1995; validated 19 July 2019](https://quicksearch.dla.mil/qsDocDetails.aspx?ident_number=207200)
29. [ASQ Eight Disciplines 8D, accessed 6 August 2026](https://asq.org/quality-resources/eight-disciplines-8d)
30. [ASQ cause-analysis tools, accessed 6 August 2026](https://asq.org/quality-resources/root-cause-analysis/tools)
31. [NIST/SEMATECH Weibull guidance, accessed 6 August 2026](https://www.itl.nist.gov/div898/handbook/apr/section3/apr312.htm)
32. [NASA ASRS overview, accessed 6 August 2026](https://asrs.arc.nasa.gov/overview/summary.html)
33. [ICAO ADREP taxonomy, accessed 6 August 2026](https://www.icao.int/safety/AIG/taxonomy)
34. [NTSB Part 135 safety study, 13 August 2024](https://www.ntsb.gov/news/press-releases/Pages/NR20240813.aspx)
35. [WHO incident reporting and learning guidance, 16 September 2020](https://www.who.int/publications-detail-redirect/4658754625926)
36. [NASA Fault Tree Handbook, August 2002](https://everyspec.com/NASA/NASA-General/NASA_FTA_1--1_68/)
37. [NASA GSFC-HDBK-8004 FMECA, 20 August 2024](https://standards.nasa.gov/node/12367)
38. [ASQ Fishbone guidance, reviewed October 2024](https://asq.org/quality-resources/fishbone)
39. [AHRQ Current Reality Tree technical review, January 2006](https://www.ncbi.nlm.nih.gov/books/NBK44036/)
40. [Microsoft LiDAR linked-incident research, November 2020](https://www.microsoft.com/en-us/research/publication/identifying-linked-incidents-in-large-scale-online-service-systems/)
41. [Microsoft AutoARTS, July 2023](https://www.usenix.org/conference/atc23/presentation/dogga)
42. [ASQ control-chart guidance, accessed 6 August 2026](https://asq.org/quality-resources/control-chart)
43. [Martin-Delgado et al., RCA systematic review, 15 May 2020](https://pubmed.ncbi.nlm.nih.gov/32417837/)
44. [Stavropoulou et al., incident-reporting systematic review, 2 December 2015](https://pmc.ncbi.nlm.nih.gov/articles/PMC4678941/)

### AI-agent and LLM RCA

45. [Hasan & Biswas, coding-agent risk taxonomy, 29 May 2026; revised 4 August 2026](https://arxiv.org/abs/2605.30777)
46. [Shah et al., agentic-AI faults, 6 March 2026](https://arxiv.org/abs/2603.06847)
47. [Cemri et al., MAST multi-agent failure taxonomy, 17 March 2025](https://arxiv.org/abs/2503.13657)
48. [Microsoft AgentRx, 12 March 2026](https://www.microsoft.com/en-us/research/blog/systematic-debugging-for-ai-agents-introducing-the-agentrx-framework/)
49. [OpenRCA, ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d29b8d65716093994e1d245c023e49d2-Abstract-Conference.html)
50. [CUADebug, 31 July 2026](https://arxiv.org/abs/2608.02643)
51. [False-success study, 1 June 2026](https://arxiv.org/abs/2606.09863)
52. [Roy et al., LLM RCA agents, 7 March 2024 / FSE July 2024](https://arxiv.org/abs/2403.04123)
53. [Microsoft RCACopilot, April 2024](https://www.microsoft.com/en-us/research/publication/automatic-root-cause-analysis-via-large-language-models-for-cloud-incidents/)
54. [Microsoft GPT-4 in-context RCA, July 2024](https://www.microsoft.com/en-us/research/publication/automated-root-causing-of-cloud-incidents-using-in-context-learning-with-gpt-4/)
55. [Tyen et al., mistake-location study, August 2024](https://aclanthology.org/2024.findings-acl.826/)
56. [Kamoi et al., self-correction survey, 2024](https://aclanthology.org/2024.tacl-1.78/)
57. [LLM evaluators favour their own generations, NeurIPS 2024](https://papers.nips.cc/paper_files/paper/2024/hash/7f1f0218e45f5414c79c0679633e47bc-Abstract-Conference.html)
58. [Zhuang et al., choice-supportive bias, 11 April 2025](https://ojs.aaai.org/index.php/AAAI/article/view/34843)
59. [Anthropic, reasoning faithfulness, 3 April 2025](https://www.anthropic.com/research/reasoning-models-dont-say-think)

> 🧒 **Simply:** Keep the good idea: repeated problems deserve their own investigation. Remove the dangerous shortcuts. Three cases should ring a bell, not prove a root. Draw several possible roads, pin evidence to every road, test the best explanation, try it on one machine first, and let a different reviewer decide whether it really worked.

06.08.2026, 15:24 UK
