# Executive Summary

Non-technical users rarely stick with complex PKM/AI starter kits without strong hand-holding. Industry benchmarks show steep drop-offs (software retains only ~39% of users after 1 month), and Obsidian users report abandoning manual setups within a week. Academic evidence confirms that strong PKM habits are needed for sustained tool use. Established MIT-licensed engines (e.g. *claude-obsidian* with 8.4k★) offer a turnkey foundation. The recommended strategy is to **fork-and-extend** such an engine rather than rebuild, adding a thin layer of role-specific vault templates and guided onboarding. High-touch “concierge” setup for each friend will be critical; resources like structured folder schemes, sample prompts, and identity files (e.g. `CLAUDE.md` or `USER.md`) can make the system usable. Distribution should remain private (friends/family channel) to limit support load. Rigid version-pin and testing of upstream updates is essential to avoid breaking users’ setups. In summary, leverage proven open-source PKM code, curate it per role, and invest in personal onboarding to maximize adoption and retention.

# Key Findings

- **Software retention is poor without value**. Across industries, on average only ~39% of new users remain after 1 month and ~30% after 3 months. Productivity apps fall off even faster. Anecdotally, developers note “Manual Obsidian lasts about a week. Then it gets abandoned” without an AI agent taking over. Unassisted PKM systems often fail to deliver immediate payoff, causing churn.

- **PKM skill levels matter**. In a study of digital workplace tools, individuals with stronger personal knowledge management practices showed significantly higher tool usage (path coefficient β=0.398, p<0.05). This suggests non-technical users without PKM habits are at high risk of abandoning a new knowledge tool unless its workflows align with their comfort zone.

- **Existing engines are mature but complex**. Three active MIT-licensed “AI second brain” projects exist: *claude-obsidian* (8.4k★, 969 forks, with extensive CLI skills), *obsidian-second-brain* (2.9k★, 341 forks), and *obsidian-skills* (38.7k★, by Obsidian’s CEO) with broad feature coverage. These projects have built robust feature sets (multi-model agents, vault linting, scheduled agents, PARA/LYT organization modes, etc.). Their popularity shows demand (e.g. 1,000+ practitioners use Sebastien Dubois’s paid Starter Kit). However, this complexity can overwhelm new users.

- **Human maintenance burden kills wikis**. As Karpathy notes, “Humans abandon wikis because the maintenance burden grows faster than the value”. Without an AI automating upkeep, a shared or personal vault will stagnate. Relying on an AI-agent engine (which auto-updates links and summaries) is key to sustaining engagement. Conversely, users stressed that the simplest solution is best – “Take notes. That’s it” – highlighting that over-engineering invites abandonment.

- **Concierge onboarding boosts early success**. Interviews with SaaS founders emphasize high-touch onboarding for initial users. For example, one founder advises treating first installs as “done-with-you” or “premium” services, documenting every step for future automation. Early manually-guided setup yields invaluable feedback and deep user commitment, whereas pure self-serve installers risk confusion and drop-off.

- **Fork-vs-build trade-offs**. Industry experience shows every approach has costs. Forking a mature open-source engine is much faster than building from scratch, but inherits upstream complexity and migration work. Building fresh gives custom control but requires huge R&D effort. The heavy-hit projects cited above demonstrate that stand-alone success took months of dev. For a small, private audience, leveraging an MIT codebase plus minimal custom “role packs” will save effort and reduce bugs, as long as one can manage upstream drift.

- **Failure and breakage risks**. Even well-intentioned updates can disrupt non-technical users. One Obsidian user reported: “The 1.7.4 update has really messed things up…I’ll continue to stick with v1.6.7… Don’t update to 1.7.4!”. Complex plugin sets or CSS can break with each version bump. Without careful version-pin and testing, friends’ machines could be left unusable. (In practice, that means disabling auto-update for the plugin, or baking a known-good release into each vault.)

# Contradictions & Debates

- **“Just note-taking” vs “feature-rich system”**. Some users dismiss PKM frameworks as overkill – e.g. “Obsidian just isn’t that magical… If you have no need to write down notes, then don’t bother”. Others invest heavily in elaborate structures (PARA, Zettelkasten, AI workflows). In reality, non-technical users benefit most from simplicity. The strategy here is to hide complexity: provide a curated vault with sensible defaults, templates and minimal required actions. Advanced features should exist but be optional, so novices can “just take notes” comfortably while having growth paths available.

- **Fork vs Build**. High-scale projects (e.g. Reddit’s Kubernetes platform) acknowledge a classic dilemma: “Will I contribute changes upstream?… build something new?… fork and maintain?”. Forking saves time initially but means absorbing future complexity and updates. Building from scratch is even heavier. Weighing trade-offs, the friend-oriented toolkit should favor *forking* a proven base. This avoids reinventing the wheel and lets you piggyback on others’ ongoing improvements, accepting the need for periodic maintenance of your fork (and perhaps contributing fixes upstream when needed).

- **Concierge vs Self-Serve**. If the project were a commercial product, one might debate self-service documentation vs guided setup. For private friends, evidence strongly favors white-glove assistance at first. Founders in SaaS emphasize that for early adopters, “manual isn’t the product, it’s the research lab”. In other words, treat initial installs as a learning phase. In-house, you can do the heavy lifting (or split screen zoom sessions). This contrasts with many open-source communities that expect tech-savvy users; our “customers” are non-technical, so high-touch is needed.

- **Public vs Private Distribution**. Publicizing a polished starter kit might build a community, but it invites support requests and potential forks from random users. In this case, the owner’s advantage (moat) is curating content and support, not code per se. Keeping the distribution limited (private GitHub repos or invite-only plugin marketplace entries) reduces noise. As one SaaS builder put it, concierge onboarding “is dangerous as a permanent identity” if offered widely, but perfect for an initial pilot.

# Best Practices

- **Base on a proven engine**. Leverage an existing MIT-licensed project (e.g. *AgriciDaniel/claude-obsidian*) to avoid reimplementing core AI-vault logic. These projects come with tested CLI commands, templates, and even automated tests. Fork it so you can customize plugin settings, but accept most of the upstream code as-is.

- **Use structured vault templates**. Provide each friend with a pre-built vault (template) tailored to their role. Include an opinionated folder structure (e.g. PARA-inspired inbox/projects/areas/resources/archive), daily note boilerplate, and a root `CLAUDE.md` or `AGENTS.md` instructing the agent how the vault is organized. Add a `USER.md` identity file describing their job or style, which enhances agent relevance. This “opinionated” setup spares them from thinking up a structure.

- **Document workflows clearly**. Each vault should have a short “**Getting Started**” guide (Markdown). Spell out the *minimum* steps: how to install the plugin (`/plugin marketplace add …; /plugin install …`), how to open the vault in Obsidian, and the first commands to try (e.g. `/ingest`, `/save`). Avoid long tutorials; just three to five sentences per step, with screenshots or diagrams if possible. The blog [17] shows the value of clear instructions: it provides CLAUDE.md examples and folder conventions outright.

- **Keep it simple for users**. Non-technical friends will not appreciate manual plugin install commands or JSON fiddling. Use the CLI (e.g. `claude plugin install owner/repo`) as the user-facing action, but you do it together. If possible, prepare a script (like `setup-vault.sh` in claude-obsidian) that pre-configures the vault (graph view, CSS, etc.) so the user only needs to “open this folder” in Obsidian.

- **Limit scope initially**. Don’t overload the vault with every plugin or AI trick. Start with core skills: note ingestion (`/obsidian-save`), daily notes, and one semantic query (e.g. `/obsidian-find`). Advanced features (like multisource web research, vision, canvas creation) can be taught later if the user sticks with it. This “vanilla” approach avoids overwhelming first-timers.

- **Train on one use-case at a time**. During onboarding, walk through a realistic task (e.g. saving meeting notes to relevant pages, or ingesting an article). Encourage them to see quick wins (e.g. auto-linking concepts). Initial success builds habit. Remind them “just take notes” – use Obsidian for something already familiar (like journaling) so they hit “value” immediately.

- **Provide ongoing support**. Schedule a quick follow-up (in person or call) after 1–2 weeks. Answer questions and apply updates manually if needed. Non-tech users may abandon silently if stuck. In active communities this is called “next-day check-in” or “in-app nudges”. Here it’s a personal courtesy to cement their use.

- **Version and test updates**. Treat the plugin/vault like production software. Pin to a known working release. Before suggesting any upstream update, test it yourself: set up the vault in a fresh environment (or use a VM/container) and run all core commands (ingest, query, backup) to ensure nothing breaks. Only then apply the update to friends. Communicate any compatibility issues promptly (e.g. “skip this update – use v1.6.7 for now” as one user did).

- **Emphasize data ownership and privacy**. Highlight that all notes remain plain Markdown on their machine (no cloud lock-in). This builds trust and eases fears. If any remote syncing or backup is involved, make it transparent and optional.

# Failure Modes

- **Overcomplex kit → immediate churn**. Packing too many plugins or workflows kills adoption. A user lamented needing “60 plugins” and abandons upgrades. Non-tech users should not have to configure or understand more than a few tools. 

- **Abandoned vault**. If the system requires frequent manual updates (e.g. linking pages, summarizing logs) and the user isn’t motivated, it will “rot” and be abandoned. Karpathy warned: *“Humans abandon wikis because the maintenance burden grows faster than the value.”*.

- **Version breakage**. An upstream change (Obsidian core or one of the skills) may break the vault. As seen above, even Obsidian itself can introduce incompatibilities. Failure to control updates means a friend’s vault might stop working overnight.

- **Loss of motivation**. If early tasks yield no “aha” moment, users lose interest. For example, if they ingest notes but never retrieve anything useful, they may “just use Google/ChatGPT directly” instead. This is why structured onboarding and showing retrieval (e.g. asking a question and getting an answer with citations) is critical.

- **Fork fragmentation**. If each friend modifies their vault independently (adding notes differently or tweaking commands), sharing a common base becomes hard. Differences could make it impossible to provide a single update path, leading to multiple outdated forks.

- **Security/privacy slip-ups**. If the system requires API keys (e.g. for web research), improper configuration could expose personal data. Always treat the local vault as private (e.g. no accidental sync to a public repo).

# Strategic Recommendations

1. **Fork an MIT-licensed base (e.g. *claude-obsidian*).** This gives you a stable engine with minimal licensing risk. It already includes tests, a plugin manifest, and CLI commands. Working from a fork means you inherit upstream improvements (AI models, language support) while adding only what you need.

2. **Add thin “role packs” on top.** For each friend’s role (e.g. student, manager, researcher), prepare a vault template with tailored structure and example content. These packs might include specialized tags or dashboards, but share the common core (e.g. the  PARA or Zettelkasten foundation from [17]). The differences should be mainly content and presets, **not code**, so you’re not maintaining separate engines per role.

3. **Craft guided onboarding documentation.** Write concise, step-by-step instructions (3–5 short paragraphs, bullet lists) for installation and daily use. Format as a single-page “README” or in-vault start guide. Use code blocks or numbered steps for clarity. Provide a link for them to copy-paste install commands, but be ready to do it together. Emphasize *immediate small tasks* (like adding today’s note) to build habits.

4. **Use concierge (hands-on) setup for each friend.** Schedule a one-hour session per person. Help them install Claude Code CLI and the plugin (`/plugin marketplace add …; /plugin install …`), open the vault, and run a sample workflow (e.g. ingest a PDF or site). Verify it works, adjust any path issues, and celebrate the first success. Offer this for free as part of the experiment; it’s an investment in retention.

5. **Choose private distribution**. Instead of going public, share the fork via private GitHub repos or invite-only links. This fits the “friends-and-family” scope and keeps user count manageable. It also lets you iterate with minimal noise. If needed, the plugin marketplace supports private catalogs (the existing projects suggest adding via `/plugin marketplace add owner/repo`).

6. **Define an update governance plan.** Don’t auto-update the vault. Set a schedule (e.g. quarterly) to check upstream changes. Run your full suite of tests or workflows in a sandbox vault before rolling out. Communicate upcoming changes to friends (“I’m going to upgrade your agent next week; we’ll check it together”). Provide an easy rollback by backing up their vault beforehand.

7. **Monitor and adapt.** After 1–2 weeks, solicit feedback: what’s confusing, what’s useful. Be ready to simplify further (e.g. remove an unnecessary plugin) or adjust documentation. Use their feedback to tighten the system. Remember, your “real moat” is curation and support, not the code itself.

# Action Plan

1. **Audit and select engine.** Clone the latest *AgriciDaniel/claude-obsidian* repo. Review its features and remove any skills or default content not needed for your friends’ roles. Ensure the plugin.json and marketplace.json are set for MIT distribution.

2. **Fork and configure.** Fork the repo under your control. Adjust the `plugin.json` metadata (name, slug) if you want a custom identity. Commit a `bin/setup-vault.sh` if needed to auto-configure the vault upon first open (colors, graph filters, etc.). Tag this as your v1.0.

3. **Develop vault templates.** For each target role, create a copy of the repo and customize the seeded vault: add example notes (e.g. “Daily note template”, “Meeting notes template”), domain-specific resources folders, and relevant tags. Populate CLAUDE.md with a brief vault guide (see [17†L279-L288] for example language). Place a sample USER.md with a persona blurb.

4. **Write onboarding docs.** In each vault, include a “Start Here” note: step-by-step how to install the plugin (GitHub CLI + `claude plugin` commands), how to capture a first note, and how to query it. Keep the language simple (“copy this command”), and add screenshots if possible. Also prepare an external quick-start cheat sheet in Markdown or PDF.

5. **Pilot internally.** Test the process on your own machine or with a technical friend. Execute every step in the docs. Fix typos or broken steps. Run through an entire day’s workflow (ingest some text, ask a question, wrap up). Ensure plugin commands like `/wiki-ingest`, `/wiki-query`, `/obsidian-lint` work and produce expected results.

6. **Concierge rollout.** For each friend, schedule a setup session. Remotely or in person, clone the appropriate vault template, run `bin/setup-vault.sh`, open Obsidian, and add the plugin via CLI. Demonstrate a simple task (e.g. “Let’s save this conversation into a note”). Observe them trying the basic commands. Answer questions.

7. **Follow-up and simplify.** After a week, check who’s still using it. If someone has stalled, reach out to offer help. Tweak the system based on their feedback (maybe reduce a step or add a visual cue in the vault). Encourage using it at least daily (e.g. journal or to-do capture).

8. **Manage updates carefully.** Keep an eye on the original project’s releases (subscribe to GitHub notifications). When a new release appears, pull it into a test vault and verify key functions. If safe, update the fork and coordinate a mini session to apply the upgrade. If not, postpone or skip until issues are resolved upstream.

9. **Plan for future scaling**. If one friend becomes a power user, they might desire more autonomy or features. At that point, consider whether to widen distribution or remain private. Most likely, maintain it as a curated personal tool – your core advantage is *trust and guidance*, not broad adoption.

# Sources

- Pendo (2025) retention benchmarks: average software retains ~39% of users after 1 month.  
- PKM adoption study (2025): PKM capability strongly predicted tool use (β=0.398, p<0.05).  
- BuildMVPFast blog (Apr 2026): “Manual Obsidian lasts about a week. Then it gets abandoned…”.  
- AgriciDaniel/claude-obsidian GitHub (2026): 8.4k stars, 969 forks; MIT license.  
- eugeniughelbur/obsidian-second-brain GitHub (2026): 2.9k stars, 341 forks; MIT license.  
- Dubois (2025) Obsidian Starter Kit blog: “used by 1,000+ professionals”.  
- Karpathy (2023) LLM-wiki gist: *“Humans abandon wikis because the maintenance burden grows faster than the value.”*.  
- SaaS forum (2024): “Concierge onboarding can be a great wedge for the first 10–20 customers… frame it as ‘done-with-you setup’”.  
- Reddit (2022): “Stop building ‘The Second Brain’… Obsidian just isn’t that magical… just take boring notes.”.  
- User forum (Oct 2024): “The 1.7.4 update has really messed things up… Don’t update to 1.7.4!” (64 plugins).  
- Heavybit podcast (2023): “Will I contribute upstream? Build new? Or fork and maintain? All these options have trade-offs.”.