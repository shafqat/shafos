# ShafOS

An open-source kit to build your own AI Chief of Staff with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

Not a chatbot — a **system**. Persistent context, email triage, meeting prep, project tracking, and daily briefings that make sure nothing falls through the cracks.

## How it works

Read the full story: **[bubs-ai.vercel.app](https://bubs-ai.vercel.app)**

## Quick start

### Prerequisites
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Ideally, at least one MCP integration set up (Outlook, Gmail, Google Calendar, Slack, etc.)
  - The system works without MCPs (you can paste info manually), but it shines when it can pull your email and calendar directly

### Setup (20 minutes)

1. **Clone this repo:**
   ```bash
   git clone https://github.com/shafqat/shafos.git
   cd shafos
   ```

2. **Create a new project directory** and copy the templates:
   ```bash
   mkdir ~/my-cos && cp -r templates ~/my-cos/
   cd ~/my-cos
   ```

3. **Open Claude Code** in your new project directory:
   ```bash
   claude
   ```

4. **Paste the bootstrap prompt** from [`bootstrap.md`](bootstrap.md) — everything below the `---` line. Claude will interview you and generate your personalized system.

5. **Start using it.** In your next session, try: "Run my morning brief" or "What's on my calendar today and what should I know?"

### Optimizely employees

There's an internal Optimizely-specific version that pre-wires the exact same tech stack (Outlook/Teams, Salesforce, Coda, ClickUp, Optimizely Analytics). Ask Shafqat for access to the private repo.

## What's in the box

```
shafos/
├── bootstrap.md              # The prompt that builds your system (generic)
├── templates/
│   ├── CLAUDE.md             # Master config — personality, principles, integrations
│   ├── context/
│   │   ├── about-me.md       # Who you are — bio, values, communication style
│   │   ├── professional.md   # Role, company, key initiatives
│   │   ├── contacts.md       # People who matter and why
│   │   ├── calendar.md       # Meeting rhythms and protected time
│   │   └── personal.md       # Family, dates, personal context (optional)
│   ├── systems/
│   │   ├── email-triage.md   # Priority tiers, search methodology, filtering rules
│   │   ├── morning-brief.md  # Quick daily refresh — overnight catch-up
│   │   ├── evening-brief.md  # Heavy prep — tomorrow's calendar, loose ends, prep
│   │   ├── weekly-review.md  # Sunday bridge — past week, next week, priorities
│   │   └── meeting-prep.md   # Prep levels by meeting type, cross-referencing rules
│   └── active/
│       ├── priorities.md     # Top 3-5 things that matter right now
│       ├── projects.md       # Active project tracker with status and action items
│       └── prep-queue.md     # "Don't forget to bring this up" meeting items
└── README.md
```

## Key ideas

**Context is everything.** The system works because every session starts with deep context about who you are, what you're working on, and who matters. Without that, it's just a fancy chatbot.

**The triage engine is the killer feature.** Raw email inboxes are 80% noise. The tiered priority system with sender-based rules, elevation signals, and mandatory search patterns means nothing critical gets buried.

**The prep queue bridges conversations.** Action items from Tuesday's meeting need to surface in Thursday's 1:1. Without the prep queue pattern, these fall through the cracks.

**Codify everything.** When your assistant makes a mistake and you correct it, that correction should become a rule in a system file — not just a one-time fix. The system gets smarter over time because lessons are encoded, not just remembered.

**Start lean, grow organically.** You don't need every file on day one. Start with CLAUDE.md + context files + email triage. Add systems and automation as you find what works.

## Automation (optional)

The bootstrap sets up the interactive system — you talk to your assistant in Claude Code sessions. For automated daily briefs (morning, evening, weekly) sent to your email on a schedule, you'll need:

- A Gmail account for sending (SMTP with app password)
- A scheduling mechanism (launchd on Mac, cron on Linux, Task Scheduler on Windows)
- Headless Claude Code (`claude -p`) for unattended runs

This is documented on the [ShafOS website](https://bubs-ai.vercel.app) under the Automation section.

## Built by

[Shafqat Islam](https://www.linkedin.com/in/shafqatislam/) — President, Optimizely. Built originally as "Bubs," a personal AI Chief of Staff. ShafOS is the open-source kit for building your own.

Powered by [Claude Code](https://docs.anthropic.com/en/docs/claude-code) from Anthropic.
