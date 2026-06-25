# Hermes Deck — Design System Prompt

> A reusable specification + generation prompt for the **Hermes Agent** slide deck.
> Theme: **Dark + Hermes Gold**. Canvas: **1280 × 720** (16:9). Source of truth: `dark.css`.
> Use this document to (a) read the talk-track for the master "End to End" slide, and
> (b) paste the **Generation Prompt** (bottom) into any LLM / design tool to produce a
> new slide that matches the deck exactly.

---

## 1. Speaker notes — Slide 00 · "One Orchestrator, End to End" (master)

**Role of the slide:** the visual table-of-contents. Everything in slides 01–07 is one
zone on this map. Open and close the talk here.

**Talk track (~75 sec):**

> "Most 'agents' are a prompt wrapped around a model. Hermes is different: it's **one
> orchestrator** — a single `AIAgent` object that runs the *same loop* whether it's
> triggered from a chat app, the CLI, a cron job, or an IDE.
>
> Read it top to bottom. **Interfaces** come in at the top — Telegram, Discord, Slack,
> CLI, IDE — all funnel into the one agent. The glowing core is **Hermes itself**, and it
> coordinates four primitives: a **Kanban** board to claim and assign work, **delegation**
> to spawn sub-agents, **cron** to schedule and chain tasks, and **guardrails** that scope
> each worker's toolset and approvals.
>
> Below it are the **workers** — internal sub-agents plus external coding CLIs like Codex,
> OpenCode, and Claude Code — all driven through the same primitives. They run on real
> **execution backends**: local, Docker, SSH, Modal, Daytona — mapped down to actual
> **hardware**, from a \$5 VPS to a GPU cluster.
>
> On the left is **state** — memory and skills on disk — the learning loop that *compounds
> every session*. On the right, **any model**, no lock-in. The whole rest of this deck just
> zooms into one of these boxes."

**Closing callback (last slide):** "Every box you saw zoomed in — interfaces, primitives,
workers, execution, state — is still just **one agent**, one loop."

**Speaker-note convention for every slide:** 1 framing sentence → 2–3 sentences walking the
diagram left→right / top→bottom → 1 takeaway that names the next slide. Bold the single
word the audience should remember (it matches the gold `<b>` accent on the slide).

---

## 2. Design language

| Aspect | Rule |
|---|---|
| **Mood** | Premium, dark, editorial-technical. One hero per slide. Generous negative space. |
| **Accent discipline** | **Gold is the only brand accent.** Tonal gold/amber/bronze for hierarchy. |
| **Semantic color** | Only two semantic exceptions: **green = keep/positive**, **coral = prune/negative**. Never introduce a 3rd hue. |
| **Depth** | Flat panels + soft shadow. Emphasis = *glow*, not a brighter fill. |
| **Icons** | Line icons only, 1.7 stroke, round caps/joins, drawn in gold/amber. No filled or multicolor icons. |
| **Motion of the eye** | Solid connector = fixed flow; dashed connector = dynamic/delegated relationship. |

---

## 3. Design tokens (exact — from `dark.css`)

**Color**
```
--gold   #FFD700      brand accent, emphasis, <b> text, step coins
--amber  #f5b942      secondary accent, line icons
--amber2 #e8965a      tertiary warm accent
--bronze #b8923c      muted captions / zone labels
--bg     #0a0a0c      slide background (under gradients)
--paper  #16140d      panel fill (warm dark)
--paper2 #121117      cool dark panel
--ink    #f4efe1      primary text
--ink2   #a89e83      secondary text / subtitle
--ink3   #76705a      dim text / footnotes
--line   rgba(255,255,255,.09)   hairline borders
SEMANTIC:  green-d #7fd08a (keep)   coral-d #ee9077 (prune)
```

**Background** (every `.slide`)
```
radial-gradient(760px 440px at 50% -8%, rgba(245,170,40,.15), transparent 62%),
radial-gradient(900px 600px at 92% 110%, rgba(255,215,0,.05), transparent 60%),
#0a0a0c
+ faint 46px gold grid, radially masked from top-center
```

**Typography**
```
Display / headings  Space Grotesk  700  (h1 36px, letter-spacing -.6px)
Body / labels       Space Grotesk  600/500
UI / logo / tags    Inter          700
Code / mono values  JetBrains Mono 400/500   (model ids, file paths, CLI)
Subtitle .sb 15.5px ink2 · card title .t 16px · card body .d 12.5px ink2
```

**Spacing & grid**
```
Slide 1280×720, padding 40px 52px 0
Header (.hd) top · Stage (.stage, z2, margin-top 26px) middle · Footer bar (.fbar) bottom
Panel radius 14px · pill/coin radius 50% · chip radius 11px
```

---

## 4. Component inventory

- **`.box`** — base panel (radius 14, hairline border, paper gradient, soft shadow).
  `.t` title + `.d` description; `.d b` renders gold. Modifier `.center` centers content.
- **Tonal variants** — `.peach .yellow .blue .purple` collapse to *gold-tinted* panels
  (border = accent, fill stays dark). `.green` / `.coral` keep their semantic hue.
  `.slate` = neutral cool panel. `.plain` = default.
- **`.glow`** — the hero treatment: gold border + amber outer glow + inner warm glow.
  **Exactly one `.glow` per slide** (the orchestrator, the loop, the key idea).
- **`.step`** — 30px gold "coin" badge for numbered sequences.
- **`.chip`** — 42px (or `.sm` 34px) rounded icon tile holding a 24px line icon.
- **`.wires`** — absolute SVG layer for connectors. `path/line` = solid gold .34 alpha;
  `.dash` = coral dashed (dynamic). Arrowheads via a shared marker.
- **`.elabel`** — small pill label sitting on a connector.
- **`.colhd` + `.divider`** — column caption + dashed-gold vertical divider for compare layouts.
- **`.fbar`** — full-width footer takeaway band (gold-bordered top, `<b>` = gold).
  `.fnote` — bottom-right meta caption.
- **`.tg`** — tag chips: `.g` green · `.y` amber · `.b` gold · `.p` bronze · `.c` coral.
- **Zone label** — `Inter 700, 11px, letter-spacing 2px, color bronze` for grouping captions
  (STATE / MODELS / WORKERS).

---

## 5. Layout composition rules

1. **Header** every slide: `h1` (ink, with one gold-gradient keyword) + `.sb` subtitle +
   `datasciencedojo` logo top-right. Title sentence-case, ≤ 6 words of payload.
2. **One hero.** Largest element, centered or center-spine, gets `.glow`. Everything else
   is tonal/quiet so the hero pops.
3. **Group with zones.** Related boxes live inside a labeled wrapper panel (thin border,
   faint fill) with a bronze zone-label — not loose on the canvas.
4. **Connectors carry meaning.** Solid = fixed pipeline; dashed = delegation/optional.
   Arrowheads always point in flow direction; add an `.elabel` when the relation needs a word.
5. **Footer states the takeaway**, with the one memorable word in gold `<b>`. `.fnote`
   bottom-right may cross-reference ("map of slides 01–07").
6. **Mono for machine things** only (model ids, paths, commands). Never mono for prose.
7. **Whitespace budget:** keep ≥ ~22px gutters between zones; never fill the canvas edge-to-edge.

---

## 6. Deck narrative map (composition of the set)

| # | Title | One-line role | Hero / layout |
|---|---|---|---|
| 00 | **One Orchestrator, End to End** | Visual TOC; one agent over all zones | Glowing central hub + intake/workers/exec/hw bands + state/model rails |
| 01 | Stateless vs. Self-Learning Agents | The *why*: memory = agent vs chatbot | Two-column compare (`.colhd` + `.divider`) |
| 02 | The Hermes Learning Loop | 3 systems that compound | Cyclic loop, 3 nodes, `.step` coins |
| 03 | How Hermes Manages Memory | extract → store → retrieve | Left→right pipeline, `.step` 1·2·3 |
| 04 | How Hermes Creates & Curates Skills | auto-write then improve; keep/prune | Branch w/ `.green` keep · `.coral` prune |
| 05 | Autonomous Coding Agent Team | claim · scope · run | Kanban + ownership map |
| 06 | Hermes on Real Hardware | each part → a physical resource | Mapping rows, agent layer → hw layer |
| 07 | Plugging In Codex / OpenCode / Claude Code | same primitives, new workers | Orchestrator → external CLI workers |

**Through-line:** 00 frames the whole; 01 motivates; 02–04 = the **state/learning** rail;
05+07 = the **workers** rail; 06 = the **hardware** band. Each slide zooms one zone of 00,
so 00's layout doubles as the deck's information architecture.

---

## 7. Generation Prompt (paste this to produce a matching slide)

```
You are designing one 1280×720 slide for the "Hermes Agent" deck.
THEME: dark + Hermes gold. Output: a single self-contained HTML file that links dark.css
(or inlines equivalent tokens).

NON-NEGOTIABLE STYLE:
- Background: near-black #0a0a0c with a soft amber radial glow at top-center, a faint
  gold glow bottom-right, and a 46px gold grid radially masked from the top.
- Gold #FFD700 is the ONLY brand accent. Use tonal gold/amber #f5b942/bronze #b8923c for
  hierarchy. The ONLY other hues allowed are green #7fd08a (keep/positive) and
  coral #ee9077 (prune/negative) — never a third color.
- Type: Space Grotesk for headings/labels, Inter for UI/logo/tags, JetBrains Mono ONLY for
  model ids / file paths / CLI. h1 = 36px, 700, letter-spacing -.6px, ink #f4efe1 with
  exactly ONE gold-gradient keyword.
- Components from the system: .box panels (radius 14, hairline border, dark paper gradient,
  soft shadow), tonal variants that stay dark with an accent border, .glow for the hero,
  .step gold coins for numbered steps, .chip tiles holding 24px LINE icons (1.7 stroke,
  round caps, gold/amber, never filled/multicolor), .wires SVG connectors (solid gold =
  fixed flow, coral .dash = dynamic/delegated), .elabel pills on connectors, .fbar footer
  takeaway with the key word in gold <b>, .tg tag chips.

LAYOUT RULES:
1. Header: h1 + .sb subtitle + "datasciencedojo" logo top-right.
2. Exactly ONE hero element, centered/dominant, with .glow; everything else stays quiet.
3. Group related boxes inside labeled zone wrappers (bronze 11px letter-spaced 2px caption).
4. Solid arrows = fixed pipeline; dashed = delegation/optional; arrowheads in flow direction.
5. Footer .fbar states the takeaway with one gold-bold word; optional .fnote bottom-right.
6. Generous whitespace (~22px+ gutters); never edge-to-edge clutter.

SPEAKER NOTES: also output a ~60–75s talk track: 1 framing sentence → 2–3 sentences walking
the diagram top→bottom / left→right → 1 takeaway that names the next slide. Bold the single
word that matches the slide's gold <b> accent.

DECK CONTEXT: This deck tells one story — Hermes is ONE orchestrator (a single AIAgent loop)
spanning interfaces → primitives (kanban/delegation/cron/guardrails) → workers (internal +
external coding CLIs) → execution backends → hardware, with a state/learning rail (memory +
skills that compound) and any-model-no-lock-in. Slide 00 is the map; every other slide zooms
ONE zone of it. Keep this slide visually consistent with that map.

SLIDE TO PRODUCE: <title> — <one-line role> — <hero/layout from the deck map>.
```
