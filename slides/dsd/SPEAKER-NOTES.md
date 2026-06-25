# Hermes Deck — Speaker Notes (all slides)

Format per slide (same convention as the design system): **1 framing sentence → 2–3
sentences walking the diagram → 1 takeaway that names the next slide.** The **bold** word
matches the gold `<b>` accent on the slide. Target ~60–75 s each; the deck runs ~9–10 min.

---

## 00 · One Orchestrator, End to End  *(master / map)*

> "Most 'agents' are a prompt wrapped around a model. Hermes is **one orchestrator** — a
> single `AIAgent` object that runs the *same loop* whether it's triggered from a chat app,
> the CLI, a cron job, or an IDE.
>
> Read it top to bottom. **Interfaces** funnel in at the top. The glowing core is Hermes,
> coordinating four primitives — **Kanban** to claim and assign work, **delegation** to
> spawn sub-agents, **cron** to schedule, and **guardrails** that scope each worker. Below
> are the **workers** — internal sub-agents plus external coding CLIs — running on real
> **execution backends** down to hardware, from a \$5 VPS to a GPU cluster. Left is **state**
> — memory and skills that compound; right is **any model**, no lock-in.
>
> The rest of this deck just zooms into one of these boxes."

**→ Next:** "Same system, one more way to see it — as a stack, top to bottom."

---

## 00b · The Hermes Stack — Top to Bottom  *(layered view)*

> "If the last slide was the map, this is the **stack** — the same system read top-down, the
> way a request actually travels.
>
> A request enters at the top through an **interface** — chat, CLI, or IDE. It drops into the
> one **orchestrator**, the single AIAgent loop that routes everything. From there it uses
> the **primitives** to coordinate, hands off to **workers** to do the job, leans on the
> **state** layer so it keeps getting smarter, runs on an **execution** backend, and finally
> resolves all the way down to **hardware**. Off to the side, **models** are cross-cutting —
> any provider, no lock-in, feeding the orchestrator and workers. Results bubble back up the
> same stack.
>
> The whole point: it's **one vertical path**, and every layer reuses the same primitives."

**→ Next:** "Now zoom in — and start with *why* the state layer matters at all."

---

## 01 · Stateless vs. Self-Learning Agents

> "Here's the single design decision that separates an agent from a chatbot: **memory**.
>
> On the left, the typical agent — you send a message, it responds, the session ends, and
> **everything is forgotten**. Next time you start over from zero. On the right, Hermes runs
> the same exchange, but at session end it writes facts, preferences, and decisions to a
> **memory store**. So the next session opens with context *already loaded* — and each turn
> through the loop it gets a little smarter.
>
> An agent that forgets is just a chatbot with a fancier UI. Hermes **remembers, and
> compounds**."

**→ Next:** "So how does that compounding actually work? Three systems."

---

## 02 · The Hermes Learning Loop

> "Compounding isn't one feature — it's **three systems** that update continuously, shown
> here as a loop around the agent.
>
> One: **Persistent Memory** — after each session Hermes extracts what mattered and injects
> it into every future session automatically. Two: **Skill Creation** — complex tasks
> produce reusable skill files, and a Curator grades them by outcome and prunes on a 7-day
> cycle. Three: **User Modeling** — a persistent picture of who you are and how you work,
> built across sessions with Honcho dialectic modeling.
>
> All three run every session, so the longer you use Hermes, the **better it knows you**.
> The next three slides open each system in turn."

**→ Next:** "First system — memory. Let's trace one fact through it."

---

## 03 · How Hermes Manages Memory

> "Memory here is concrete and local — **extract, store, retrieve**, every session.
>
> **Phase 1**, the conversation just runs normally. **Phase 2**, at session end, an
> extraction pass writes what matters to plain files in `~/.hermes/` — `user_profile.md`,
> `projects.md`, `preferences.md`, session logs. **Phase 3**, next session, those files are
> indexed in **SQLite with FTS5**; the best-matching memories are lifted into the prompt
> during context assembly, and Hermes replies with full context. Then the loop repeats.
>
> It's full-text-searchable memory **on disk — no vendor lock-in** — and it reloads itself
> before every reply."

**→ Next:** "Memory stores facts. Skills store *procedures* — same idea, applied to work."

---

## 04 · How Hermes Creates & Curates Skills

> "Skills are **procedural memory** — Hermes doesn't just remember facts, it remembers *how
> to do things*, written from real work.
>
> The top row is the lifecycle: a complex task finishes, a skill gets written as portable
> markdown in `~/.hermes/skills/`, it's loaded automatically the next time a similar task
> appears, and every 7 days a **Curator** grades skills by outcome. That grade branches:
> good skills are **kept** and reinforced, mixed ones are **revised** with better logic, and
> poor ones are **pruned** from the library.
>
> So the skill library is self-cleaning — **kept, rewritten, or pruned on outcomes** — and
> it's compatible with agentskills.io."

**→ Next:** "Now point all of this at one job: an autonomous coding team."

---

## 05 · Building an Autonomous Coding Agent Team

> "This is where the primitives combine into real **feature-level control** — who claims
> work, what each agent may touch, and where it runs.
>
> Work enters through the gateway or an IDE *(1)*. The heart is the **Kanban board** in
> SQLite *(2)* — atomic claims, heartbeats, a dispatcher that assigns *one task to one
> worker* and reaps crashes. From there it **delegates** to isolated parallel subagents,
> each with a restricted toolset *(3)*; **guardrails** set exactly which tools each may call
> and auto-deny dangerous commands *(4)*; and each runs in its own **isolated backend** —
> local, Docker, SSH, Modal, and more *(5)*. Underneath, shared Memory and Skills mean every
> agent inherits what the fleet already learned, and cron chaining turns runs into
> unattended pipelines.
>
> What you actually control is **which feature owns each layer** — flat delegation today,
> full pipelines via cron chaining."

**→ Next:** "All of this rides on physical hardware. Let's map it."

---

## 06 · Hermes on Real Hardware

> "Every part of the agent maps to a **physical resource** — and that mapping is the whole
> performance story.
>
> On the processor side: the **CPU** is the always-on control plane — agent loop, gateway,
> Kanban dispatcher, cron — light enough for a \$5 VPS. The **GPU** does heavy inference,
> usually provider-side, or self-hosted for privacy; an optional **NPU** handles on-device
> models. On the storage side it's a hierarchy: **DRAM** is the volatile working context —
> the reason compression exists; the **SSD** is hot persistent state — the SQLite memory DB,
> skills, kanban.db, queried before every reply; and the **HDD** is cold storage for
> archived trajectories and datasets.
>
> The key idea: Hermes' memory model *is* a **hardware hierarchy** — context = DRAM, memory
> and skills = SSD, trajectories = HDD."

**→ Next:** "Last piece — the workers don't have to be ours. Plug in external coding agents."

---

## 07 · Plugging In Codex, OpenCode & Claude Code

> "The workers can be **external coding agents** — and the punchline is that they reuse the
> *exact same primitives* you've already seen.
>
> Hermes delegates a coding task by loading a skill, then driving the CLI through its
> **terminal backend** — `terminal()` to launch, a sandbox to run in, `process()` to poll
> long jobs. The same wrapper drives all three: **Codex** for features and PR reviews,
> **OpenCode** for provider-agnostic parallel work in worktrees, and **Claude Code** with a
> tool allowlist and turn caps. And it's **reversible** — Hermes is itself an ACP server
> editors can drive, and Codex can even serve as a model backend.
>
> So an external coding agent is just **another worker** — same Delegation, Skills, and
> Terminal backends from slide 5."

**→ Close:** "Which brings us back to the map: interfaces, primitives, workers, execution,
state — still **one agent, one loop**." *(return to slide 00)*

---

### Presenter cheat-sheet (bold keyword per slide)

`00` one orchestrator · `00b` one vertical path · `01` remembers, and compounds · `02` better it knows you ·
`03` on disk — no lock-in · `04` kept, rewritten, or pruned · `05` which feature owns each
layer · `06` hardware hierarchy · `07` another worker.
