# Founder Intelligence Kit

Two Claude Code skills that compound over time. The more you use them, the sharper your thinking gets.

**[/mentor](#mentor-skill)** — A world-class business advisor in your terminal. Socratic, research-grounded, anti-sycophantic. Challenges your assumptions before helping you decide.

**[/memory-update](#memory-update-skill)** — Captures what you learned each session so the next one starts with full context, not a blank slate.

Used together, they create a loop: sharper thinking in the present, accumulated wisdom over time.

---

## Why these two skills

Most AI sessions evaporate. You think clearly in the moment, reach a decision, close the tab — and the next session starts cold. You re-explain the context, re-derive the reasoning, and often reach slightly different conclusions because the framing shifted.

This kit solves that with two primitives:

| Without the kit | With the kit |
|---|---|
| AI gives generic startup advice | AI knows your company, your stage, your history |
| Every session starts from zero | Context accumulates across sessions |
| Good ideas get lost | Decisions are logged with reasoning |
| You repeat the same analysis | Patterns and mistakes are captured |

The skills are designed to work together. `/mentor` does the thinking. `/memory-update` makes sure that thinking persists.

---

## What's in this repo

```
founder-intelligence-kit/
├── skills/
│   ├── mentor/
│   │   ├── SKILL.md        ← the mentor skill
│   │   └── feedback.log    ← autoimprovement log (starts empty)
│   └── memory-update/
│       ├── SKILL.md        ← the memory-update skill
│       └── feedback.log    ← autoimprovement log (starts empty)
└── templates/
    ├── MEMORY.md           ← starter memory index for your project
    └── session-log.md      ← session log template
```

---

## Quick install

**1. Install Claude Code** if you haven't: [claude.ai/code](https://claude.ai/code)

**2. Copy the skills into your user skills directory:**

```bash
# Mac / Linux
cp -r skills/mentor ~/.claude/skills/
cp -r skills/memory-update ~/.claude/skills/

# Windows (PowerShell)
Copy-Item -Recurse skills/mentor $env:USERPROFILE\.claude\skills\
Copy-Item -Recurse skills/memory-update $env:USERPROFILE\.claude\skills\
```

**3. Set up the memory system** for each project you want to track:

```bash
mkdir -p ~/.claude/projects/<your-project-name>/memory
cp templates/MEMORY.md ~/.claude/projects/<your-project-name>/memory/
```

**4. (Optional) Configure Obsidian** — see [Obsidian integration](#obsidian-integration) below.

**5. Test the install** — open a Claude Code session and type `/mentor` or `/memory-update`.

---

## The memory architecture

The memory system is a folder of small Markdown files, indexed by a single `MEMORY.md` file. Claude loads the index every session and reads individual files only when relevant.

```
~/.claude/projects/<project>/memory/
├── MEMORY.md                       ← index (always loaded, under 200 lines)
├── user_profile.md                 ← who you are, your background, preferences
├── feedback_communication_style.md ← how Claude should talk to you
├── project_<name>_status.md        ← current state of a project
├── session_2026-06-18.md           ← what happened in today's session
└── ...
```

### Four memory types

| Type | What to store | When it helps |
|---|---|---|
| `user_*` | Your role, goals, expertise level, preferences | Claude adapts its depth and framing to you |
| `feedback_*` | Corrections, validated approaches, what NOT to do | You never have to repeat the same guidance twice |
| `project_*` | Status, deadlines, decisions made, goals | Context loads instantly without re-explaining |
| `session_*` | What happened, decisions, next steps, self-reflection | Each session builds on the last |

### Why this design

**Token efficiency.** `MEMORY.md` is always in context but stays small (under 200 lines). Full memory files load only when relevant — so you pay for context proportional to what you actually need.

**Separation of concerns.** Each topic gets its own file. When a project changes, you update one file, not a monolith. When a preference changes, you update the feedback file, not hunt for it in a session log.

**Point-in-time integrity.** Memory files are snapshots. If a fact changes, update the file — don't append history. The session logs capture the timeline; the status files capture truth now.

**Self-correcting.** The `/memory-update` skill includes a self-reflection phase where Claude logs what slowed it down and what it would do differently. Over time, this surfaces patterns: recurring mistakes, approaches that work, context that's always needed.

---

## Obsidian integration

Both skills can write to an Obsidian vault. This is optional but powerful: your Obsidian vault becomes a searchable, linked knowledge base of every important decision and session.

### What gets written where

| Content | Destination |
|---|---|
| Session summary | `08-daily/YYYY-MM-DD.md` (daily note) |
| Significant decisions | `06-decisions/YYYY-MM-DD-slug.md` |
| Running log | `00-dashboard/log.md` (append-only) |

### Setup

**1.** Create the folder structure in your vault (or point to an existing one):

```
your-vault/
├── 00-dashboard/
│   └── log.md
├── 06-decisions/
└── 08-daily/
```

**2.** Open `skills/memory-update/SKILL.md` and set `YOUR_VAULT_PATH` to your vault's absolute path:

```yaml
# Near the top of the skill file, under ## Configuration:
VAULT_PATH: /Users/yourname/Documents/my-vault
```

**3.** From that point, every `/memory-update` call writes a daily note and appends to the vault log automatically.

### How Obsidian and Claude memory work together

The Claude memory system (`~/.claude/projects/`) and Obsidian serve different purposes:

| Claude memory (`~/.claude/`) | Obsidian vault |
|---|---|
| Fast in-session retrieval | Long-term knowledge graph |
| Loaded by Claude automatically | Browsed by you directly |
| Structured for AI consumption | Structured for human review |
| Project-scoped | Cross-project, searchable |
| Ephemeral when outdated | Archival, never deleted |

Think of Claude memory as the working memory (RAM) and Obsidian as the long-term record (disk). The `/memory-update` skill writes to both at the end of each session.

---

## Using the skills together

The compound loop works in three stages:

```
START SESSION
  └── Claude loads MEMORY.md + relevant project files
      └── Full context without re-explaining anything

DURING SESSION: /mentor
  └── Socratic inquiry → structured analysis → mentor's view
  └── Research-grounded, challenges your assumptions
  └── Weighted decision matrix if needed

END SESSION: /memory-update
  └── Logs decisions, what worked, what didn't
  └── Writes Obsidian daily note (if configured)
  └── Self-reflects so next session starts smarter
```

### Example: A co-founder conflict

**Session 1:**
- You describe the conflict to `/mentor`
- Mentor runs Phase 1 (loads any context it has), asks Socratic questions, builds a negotiation prep matrix
- You reach a decision on how to approach the conversation
- `/memory-update` logs the decision, the reasoning, the alternatives rejected, and the emotional state you were in

**Session 2 (one week later):**
- Claude loads memory — it already knows the conflict history, your decision, and your BATNA
- You type: "the conversation happened, here's what they said"
- `/mentor` immediately builds on stored context instead of starting from scratch
- It notices patterns: "In your session last week you flagged X as a risk. It happened. Here's what that tells us..."

**Session 3:**
- You're making a bigger strategic decision in a completely different area
- `/mentor` cross-references the conflict resolution experience — "this is similar to how you handled the co-founder negotiation: you optimized for X over Y. Is that still your priority ordering?"

This is the compounding effect. Without `/memory-update`, Session 2 and 3 start cold. With it, context accumulates.

---

## Mentor skill

### What it does

A 9-phase structured mentoring framework that:
- Loads your context first (memory + any company data you've configured)
- Uses Socratic questioning before giving any advice
- Grounds every recommendation in peer-reviewed research with cited sources
- Builds weighted decision matrices for multi-option choices
- Prepares full negotiation frameworks for deals and conflicts
- Applies cross-cultural lens for international situations
- Gives its own direct view — labeled as opinion, not fact

### Trigger phrases

```
/mentor
"advise me on..."
"help me decide..."
"negotiate this..."
"strategic decision..."
"think this through with me..."
"weighted decision..."
"decision matrix..."
"business advice..."
```

### What it doesn't do

- It does not validate your existing decision — it challenges it first
- It does not give generic startup advice — it grounds everything in your specific context
- It does not pretend certainty — all claims carry a confidence label
- It does not say "great question" — it engages with the substance

### Configuration

At the top of `skills/mentor/SKILL.md`, set:

```yaml
MEMORY_PATH: ~/.claude/projects/<your-project>/memory/
VAULT_PATH: /path/to/your/obsidian/vault    # optional
COMPANY_DATA_PATH: /path/to/company/notes   # optional
```

---

## Memory update skill

### What it does

A 8-phase session sync that runs at the end of a conversation:
1. Locates your memory system
2. Reviews everything that happened in the session
3. Extracts decisions, tasks, blockers, feedback, next steps
4. Self-reflects (what slowed Claude down, what it would do differently)
5. Writes or updates the relevant memory files
6. Updates the MEMORY.md index
7. Appends to Obsidian vault log (if configured)
8. Writes an Obsidian daily note (if configured)
9. Confirms to you what was saved

### Trigger phrases

```
/memory-update
"update memory"
"save session"
"end of session"
"wrap up"
"save progress"
"memory update"
```

### Configuration

At the top of `skills/memory-update/SKILL.md`, set:

```yaml
MEMORY_BASE_PATH: ~/.claude/projects/
VAULT_PATH: /path/to/your/obsidian/vault    # optional — leave blank to skip Obsidian steps
```

---

## Use cases

### Fundraising decision

You're deciding between two term sheets. Open `/mentor`:
- It loads your budget constraints, cap table, and any past fundraising notes from memory
- Asks Socratic questions to surface what you actually care about (control? money? network?)
- Builds a weighted decision matrix with your criteria
- Researches comparable pre-seed terms (citing sources)
- Gives its direct view with reasoning
- You run `/memory-update` → the decision is logged with rationale → next session starts with full deal history

### Hiring your first employee

You're choosing between a contract and a full-time hire for a key role:
- `/mentor` loads your burn rate and stage from project memory
- Applies Noam Wasserman's research on founder dilemmas (it cites the page)
- Asks what outcome you're actually optimizing for — speed, quality, cost, or flexibility?
- Flags the cognitive biases at play (you may be anchoring on salary cost, not total cost)
- `/memory-update` captures the decision so if the hire doesn't work out, you have the original reasoning

### Preparing for a difficult conversation

A co-founder, investor, or client conversation you've been avoiding:
- `/mentor` runs the full negotiation prep matrix (your BATNA, their BATNA, ZOPA, cultural context)
- Uses Voss's tactical empathy method to script your opening moves
- Flags if you're deciding from fear vs. clarity (emotional state check)
- Gives you calibrated questions to ask in the room
- `/memory-update` captures the prep, the conversation outcome, and what you'd do differently

### Weekly strategic review

End of each week, run `/mentor` + `/memory-update` as a ritual:
- Ask the mentor: "What am I not seeing this week?"
- It reviews recent session logs and project memory
- Identifies patterns: decisions you keep deferring, assumptions you keep making
- `/memory-update` writes the weekly reflection to Obsidian
- Over time, you build a searchable log of your strategic reasoning as a founder

---

## Compliance and best practices

These skills are built to the [Anthropic skill design guidelines](https://docs.anthropic.com/en/docs/claude-code/tutorials--creating-a-claude-skill) and the Chainlink developer agent skill principles:

- **Progressive disclosure:** The frontmatter description is kept under 1024 characters and loads every session. The full skill body loads only when triggered.
- **Explicit triggers:** Both natural language and `/slash-command` triggers are specified.
- **Autoimprovement:** Each skill includes a `feedback.log` that is read at session start and appended during the session.
- **Guardrails:** Both skills have explicit guardrails for irreversible actions (writing to external systems, committing decisions).
- **Source quality standards:** The mentor skill specifies Tier 1/2/3 source classifications and never cites unverified content.
- **Anti-hallucination:** Both skills instruct Claude to flag when it cannot verify information rather than guessing.

---

## Contributing

Issues and PRs welcome. If you've built on top of these skills or adapted them for a different use case, open a PR with your variation.

This kit is maintained by [Stephany Biello](https://github.com/stephaniebiello) and the [Lyra AI](https://justlyra.com) team.

---

## License

MIT. Use it, fork it, ship it.
