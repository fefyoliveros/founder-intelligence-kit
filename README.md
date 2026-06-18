# Founder Intelligence Kit

> Two Claude Code skills that compound over time. The more you use them, the sharper your thinking gets.

---

## The skills

| Skill | What it does | Install path |
|---|---|---|
| [/mentor](skills/mentor/SKILL.md) | Socratic business advisor. Asks questions, challenges assumptions, gives structured analysis only when you ask for it. | `~/.claude/skills/mentor/` |
| [/memory-update](skills/memory-update/SKILL.md) | End-of-session memory sync. Captures decisions, learnings, and next steps so every session starts with full context. | `~/.claude/skills/memory-update/` |

**Read the skill files** to understand exactly what each one does and how to configure it.

---

## Why these two skills

Most AI sessions evaporate. You think clearly in the moment, close the tab, and the next session starts cold. You re-explain the context, re-derive the reasoning, and often land somewhere different because the framing shifted.

This kit fixes that with two primitives:

| Without the kit | With the kit |
|---|---|
| AI gives generic startup advice | AI knows your stage, history, and constraints |
| Every session starts from zero | Context accumulates across sessions |
| Good insight gets lost | Decisions are logged with reasoning |
| You repeat the same analysis | Patterns and mistakes are captured |

The compound loop works like this:

```
START SESSION
  Claude loads MEMORY.md + relevant project files
  Full context without re-explaining anything

DURING SESSION: /mentor
  Socratic mode by default — asks questions, reflects back
  Advice mode when you ask: "give me your advice"
  Research-grounded, challenges your assumptions
  Weighted decision matrix if needed

END SESSION: /memory-update
  Logs decisions, what worked, what didn't
  Writes Obsidian daily note (if configured)
  Self-reflects so next session starts smarter
```

---

## Install in 4 steps

**Step 1. Install Claude Code** if you haven't: [claude.ai/code](https://claude.ai/code)

**Step 2. Copy the skills into your user skills directory:**

```bash
# Mac / Linux
cp -r skills/mentor ~/.claude/skills/
cp -r skills/memory-update ~/.claude/skills/

# Windows (PowerShell — run from this repo's folder)
Copy-Item -Recurse skills\mentor $env:USERPROFILE\.claude\skills\
Copy-Item -Recurse skills\memory-update $env:USERPROFILE\.claude\skills\
```

**Step 3. Set up the memory system** for each project you want to track:

```bash
# Replace <your-project> with a short name for your project
mkdir ~/.claude/projects/<your-project>/memory
cp templates/MEMORY.md ~/.claude/projects/<your-project>/memory/
```

**Step 4. Configure each skill.** Open the SKILL.md file for each skill and fill in the `## CONFIGURATION` block at the top:

```
MEMORY_PATH: ~/.claude/projects/<your-project>/memory/
VAULT_PATH:  /path/to/your/obsidian/vault   # optional
```

Test it: open a Claude Code session and type `/mentor` or `/memory-update`.

---

## How /mentor works

### Two modes

**Socratic mode (default)** — activated by `/mentor` with no other instruction, or phrases like "help me think through", "I'm struggling with", "reflect on this with me".

In Socratic mode, the mentor:
- Reflects back what it hears before asking anything
- Asks exactly one question per response
- Works through six layers: clarification, assumptions, evidence, implications, self, integration
- Never gives the answer — guides you to find it yourself
- Offers a synthesis every 4-6 exchanges: "Here's what I'm hearing..."

**Advice mode** — activated when you explicitly say: "give me your advice", "what would you do", "just tell me".

In Advice mode, the mentor:
- Runs a structured analysis (weighted decision matrix, negotiation prep, or strategic options)
- Grounds every recommendation in peer-reviewed research with cited sources
- Labels opinion vs evidence clearly: "My view is..." vs "The evidence suggests..."
- Returns to Socratic mode after delivering its view

You can switch between modes at any point in the conversation.

### What the mentor draws from

40+ years of experience, 6 continents. Academic grounding in Kahneman (cognitive bias), Voss (negotiation), Erin Meyer (cross-cultural), Wasserman (founder dilemmas), Taleb (uncertainty), and 10+ others. Case studies from Google X, Pixar, Toyota, Mercado Libre, Safaricom, and more. Cross-cultural intelligence from Japanese *nemawashi* to African *Ubuntu* to Nordic *lagom*.

Full bibliography is in [skills/mentor/SKILL.md](skills/mentor/SKILL.md).

---

## How /memory-update works

Run it at the end of any significant session. It takes 60-90 seconds and does 8 things:

1. Locates your memory directory
2. Reviews everything that happened in the session
3. Extracts: tasks completed, decisions made, blockers, user feedback, next steps
4. Writes a self-reflection (what slowed Claude down, what to do differently next time)
5. Creates or updates the right memory files (session log, project status, feedback rules)
6. Updates MEMORY.md index
7. Appends to Obsidian vault log (if configured)
8. Writes an Obsidian daily note (if configured)

The self-reflection in step 4 is what makes this compound. Over time, Claude gets better at working with you specifically — not because it's smarter, but because it's tracked what works and what doesn't.

---

## The memory architecture

```
~/.claude/projects/<project>/memory/
├── MEMORY.md                       ← index (always loaded, max 200 lines)
├── user_profile.md                 ← who you are, expertise, preferences
├── feedback_communication.md       ← how Claude should work with you
├── project_<name>_status.md        ← current state, goals, decisions
├── session_2026-06-18.md           ← what happened today
└── ...
```

### Four memory types

| File prefix | What to store | Why it helps |
|---|---|---|
| `user_*` | Your role, background, expertise level, preferences | Claude adapts depth and framing to you specifically |
| `feedback_*` | Corrections, validated approaches, what NOT to do | You never have to repeat the same guidance twice |
| `project_*` | Status, deadlines, goals, key decisions | Context loads instantly without re-explaining |
| `session_*` | What happened, what was decided, next steps | Each session builds on the last |

### Why this design

**Token efficiency.** `MEMORY.md` stays under 200 lines and loads every session. Full files load only when relevant. You pay for context proportional to what you actually need.

**Point-in-time integrity.** When facts change, update the file — don't append. Session logs capture the timeline. Status files capture what's true right now.

**Self-correcting.** The session log includes a self-reflection: what slowed Claude down, what it would do differently. Over time, this surfaces patterns and recurring mistakes.

---

## Obsidian integration

Both skills can write to an Obsidian vault. This is optional but creates a powerful second layer: a searchable, linked knowledge base of every decision and session, browsable outside of Claude.

### What gets written

| Content | Location in vault |
|---|---|
| Running chronological log | `00-dashboard/log.md` (append-only) |
| Significant decisions | `06-decisions/YYYY-MM-DD-slug.md` |
| Session summaries | `08-daily/YYYY-MM-DD.md` (daily note) |

### Setup

Create these folders in your vault (or point to existing ones):

```
your-vault/
├── 00-dashboard/
│   └── log.md       ← running log, created automatically if it doesn't exist
├── 06-decisions/    ← one file per significant decision
└── 08-daily/        ← daily notes, one per day
```

Set `VAULT_PATH` in both skill SKILL.md files, and the rest is automatic.

### Claude memory vs Obsidian

| Claude memory (`~/.claude/`) | Obsidian vault |
|---|---|
| Fast in-session retrieval | Long-term knowledge graph |
| Loaded by Claude automatically | Browsed by you directly |
| Structured for AI consumption | Structured for human review |
| Project-scoped | Cross-project, searchable |
| Updated when it changes | Append-only archive |

Think of Claude memory as RAM and Obsidian as disk. `/memory-update` writes to both.

---

## Use cases

### Fundraising

You're choosing between two term sheets. Run `/mentor`:
- It loads your burn rate, cap table, and past fundraising notes from memory
- Asks what you're actually optimizing for: control, capital, or network?
- Surfaces the cognitive biases at play (anchoring on headline valuation?)
- In advice mode: builds a weighted decision matrix with your criteria and comparable term data
- Run `/memory-update` — the decision is logged with rationale so if the deal falls through you have the original reasoning

### Co-founder conflict

A conversation you've been avoiding:
- `/mentor` runs the full negotiation prep: your BATNA, their BATNA, ZOPA, cultural context
- Uses Voss's tactical empathy method to script your opening moves
- Flags if you're deciding from fear vs. clarity
- `/memory-update` captures the prep, the outcome, what you'd do differently

### Hiring your first employee

Contract vs. full-time for a key role:
- `/mentor` loads your burn rate from memory
- Applies Wasserman's research on founder dilemmas (cites the source)
- Asks what you're optimizing for: speed, quality, cost, or flexibility?
- Flags the hidden costs you haven't counted yet

### Weekly strategic review

A 15-minute end-of-week ritual:
- Ask `/mentor`: "What am I not seeing this week?"
- It reviews recent session logs and project memory, identifies patterns
- `/memory-update` writes the reflection to Obsidian
- Over time you build a searchable log of your strategic reasoning

---

## Templates

| Template | What it's for |
|---|---|
| [templates/MEMORY.md](templates/MEMORY.md) | Starter memory index — copy to your project's memory folder |
| [templates/session-log.md](templates/session-log.md) | Session log structure — used automatically by /memory-update |

---

## Compliance with Anthropic skill design guidelines

Both skills are built to the [Anthropic skill design guidelines](https://docs.anthropic.com/en/docs/claude-code/tutorials--creating-a-claude-skill):

- Frontmatter descriptions are under 1024 characters and loaded every session
- Full skill body loads only when triggered (progressive disclosure)
- Both natural language and `/slash-command` triggers are specified
- Each skill includes a `feedback.log` for autoimprovement (read at session start, appended during sessions)
- Explicit guardrails for irreversible actions
- Anti-hallucination: both skills instruct Claude to flag unverifiable information

---

## License

MIT. Use it, fork it, ship it.

Maintained by [@stephaniebiello](https://github.com/stephaniebiello) and the [Lyra AI](https://justlyra.com) team.
