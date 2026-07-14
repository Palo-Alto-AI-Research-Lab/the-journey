# 🌱 Second Brain Starter — seed for your Claude Code

**What this is.** Paste the block below into your Claude Code (one message). It bootstraps a
"second brain + AI cofounder" on YOUR machine: a persistent memory that survives sessions, an
Obsidian-style knowledge vault, and a small set of standing rules that make your Claude act like
a cofounder (holds focus, catches procrastination, ships) instead of autocomplete.

**No data from anyone else.** This is a clean scaffold. It creates only empty structure + rules
on your own disk. Nothing is sent anywhere. You own all of it.

**After it runs you'll have:**
- `~/second-brain/` — an Obsidian vault (open it in Obsidian) with a sane folder structure.
- `~/.claude/CLAUDE.md` — the standing rules your Claude loads every session.
- `~/.claude/memory/MEMORY.md` — a one-line-per-fact memory index that persists across sessions.
- A working loop: your Claude writes durable facts to memory, and reads them back next time.

---

## ⬇️ PASTE THIS INTO YOUR CLAUDE CODE (one message)

```
You are setting up a "second brain + AI cofounder" for me on this machine. Do all of the
following, then show me a summary. Ask me nothing until it's done — pick sane defaults.

SAFETY FIRST — this must be safe to re-run and safe on a machine that already uses Claude
Code: never overwrite or delete an existing file. If ~/.claude/CLAUDE.md or
~/.claude/memory/MEMORY.md already exists, APPEND the new sections/lines (skipping any that
are already there) instead of replacing the file, and say so in the summary. If a vault
folder already exists, leave its contents untouched.

1. Create an Obsidian vault at ~/second-brain/ with these folders:
   00-Inbox/ 01-Notes/ 02-Decisions/ 03-People/ 04-Projects/ 05-Concepts/
   06-Templates/ _originals/ _drafts/
   Add a README.md at the root explaining what each folder is for (one line each).

2. Create a memory system at ~/.claude/memory/ :
   - MEMORY.md — an index file. Header "# Memory Index", then one line per memory:
     "- [Title](file.md) — one-line hook". Keep it under 135 lines / 20KB (it loads every
     session, so it must stay small — details live in the individual files, not here).
   - Each memory = its own file with YAML frontmatter:
       ---
       name: <kebab-slug>
       description: <one-line summary used to judge relevance on recall>
       metadata: { type: user | feedback | project | reference }
       ---
       <the fact. for feedback/project add **Why:** and **How to apply:** lines.>
   - Rule for you (the assistant): whenever we establish something durable — a preference, a
     decision and its reason, an ongoing project constraint, a useful reference — WRITE it as a
     memory file and add its index line. Before saving, check MEMORY.md for an existing file
     that already covers it and update that instead of duplicating. Don't save things the code
     or git history already records.

3. Create ~/.claude/CLAUDE.md with these standing rules (keep them, they're the point):

   ## Memory is real — prove it, don't fake it
   At the start of a session, read MEMORY.md. When you use a remembered fact, cite it (source +
   date). Never pretend to remember — if it's not in memory, say so. End a substantive session
   by writing what's worth keeping to memory.

   ## Ship-gate — one thing OUT before anything else
   Priority always goes to work that pushes toward an external result (a shipped feature, a user,
   a demo, a decision made real). Each working session: (1) name today's ONE ship; (2) freeze —
   don't open new research / new tooling / endless polish until the ship is done; (3) work that
   doesn't push forward and where I tend to hide, I don't start on my own. Carve-out: real
   blockers (a broken build, a backup, a security issue, or something the ship literally needs)
   get done anyway — but if it doesn't block today's ship, it waits.

   ## Fix the root cause, not the symptom
   Trace a bug to its first cause (5 whys) and fix the class, not the one case. A patch on the
   symptom is temporary — mark it and open a task for the root. Before touching safety-critical
   code (locks, auth, retries, schedulers, data pipelines), READ the existing implementation and
   prove behavior with a tiny test before proposing a change.

   ## Test after you build, before you say "done"
   After building or editing anything executable: run it on real input, try to break it (empty
   input, missing dependency), confirm you can SEE it worked (a counter/log), then give a verdict
   with evidence. Only a passed test = "done".

   ## Keep it simple and repairable (AK-47)
   Default to the simplest thing that works and that I could understand and fix myself. Flag any
   real complexity (a new dependency, service, abstraction) with what pain it solves and the
   cheaper alternative. One source of truth per rule — link, don't copy.

4. Verify with counters, not vibes: list the vault tree (expect 9 folders + README.md), print
   MEMORY.md, and confirm ~/.claude/CLAUDE.md contains all 5 rules (count the "## " headers).
   If anything is missing, fix it before summarizing. Write one starter memory (type: user)
   capturing my name (take it from `git config user.name` or the OS username — I'll correct it
   later) and what I'm building (from context, or a placeholder), so the loop is proven
   end-to-end.

5. Summarize what you created and tell me the ONE next thing I should save to my second brain.
```

---

## Notes for you (the human)

- **Open `~/second-brain/` in Obsidian** to browse/edit your notes visually.
- The magic isn't the folders — it's the **habit**: let your Claude write durable facts to memory
  and it stops re-learning you every session.
- Tune `CLAUDE.md` to your own voice and workflow over time. It's yours.
- Questions / stuck? Ping me — happy to jump on it. 📱 WhatsApp +1 341 222 9178

_— built by the Palo Alto AI Research Lab. Free. Use it, break it, tell us what's missing._
