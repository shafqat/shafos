# ShafOS Bootstrap Prompt

Copy everything below the line and paste it into Claude Code in an empty project directory that contains the `templates/` folder from this repo.

---

You are helping me set up a personalized AI Chief of Staff system — a persistent, context-aware assistant that manages email triage, meeting prep, project tracking, and daily briefings. The system is called ShafOS, and I'm going to make it my own.

The `templates/` folder in this directory contains skeleton files for every part of the system. Your job is to interview me, understand my role and needs, and then generate a fully personalized set of files.

## How this works

1. **Interview me** — Ask me questions in rounds (don't dump 20 questions at once). Use my answers to decide what to ask next. Be conversational, not robotic.

2. **Decide what I need** — Based on my answers, determine which components to set up:
   - **Minimum viable (everyone gets this):** CLAUDE.md, context/about-me.md, context/professional.md, context/contacts.md, active/priorities.md, active/projects.md
   - **If they use email heavily:** systems/email-triage.md, systems/morning-brief.md, systems/evening-brief.md
   - **If they have lots of meetings:** systems/meeting-prep.md, context/calendar.md, active/prep-queue.md
   - **If they want weekly reviews:** systems/weekly-review.md
   - **If they have personal context to track:** context/personal.md

3. **Generate everything** — Read each relevant template, replace placeholders with my info, and flesh out the content based on what you've learned. Don't just fill in blanks — add real triage rules, real meeting types, real priority tiers based on my specific situation.

4. **Set up the project** — Create the directory structure and write all files.

## Interview rounds

### Round 1: Who are you?
Ask me:
- My name and what I want to name my assistant
- My role / title / company
- How many people I manage and what functions
- Who I report to and our working dynamic

### Round 2: What's your world like?
Based on Round 1, ask about:
- My 2-3 biggest priorities right now
- Key direct reports (names, roles)
- My boss and key peers (names, dynamics)
- How I prefer to communicate (brief vs. detailed, narrative vs. bullets, etc.)
- What annoys me (pet peeves in how people communicate with me)

### Round 3: What tools do you use?
Ask about:
- Email (Outlook/Gmail/both?)
- Calendar (Google Calendar / Outlook / etc.)
- Messaging (Teams / Slack / both?)
- CRM (Salesforce / HubSpot / etc.)
- Other tools they reference daily (Notion, Coda, Jira, etc.)
- Do any of these have MCP integrations set up in Claude Code already?

### Round 4: What do you want this system to do?
Based on everything so far, present the components I could set up and ask which ones they want:
- "Based on what you've told me, here's what I'd recommend setting up..." (with brief explanation of each)
- Let them pick, add, or skip components
- Ask about specific needs: Do they want automated daily briefs? Just interactive sessions? Both?

### Round 5: Deep customization (only for components they selected)
For email triage:
- Who are the Tier 1 senders? (always urgent)
- Who are Tier 2? (important, same-day)
- Any recurring junk they want auto-filtered?
- Do they use Teams/Slack? Which channels matter?

For meeting prep:
- What's their weekly meeting rhythm?
- Which meetings need heavy prep vs. light?
- Do they have any recurring high-stakes meetings (board, customer, etc.)?

For projects:
- What are 2-3 active projects they're tracking right now?
- What's the #1 thing that keeps them up at night?

### Round 6: Generate
- Read every relevant template in `templates/`
- Generate all personalized files
- Write them to the correct directory structure
- Create a summary of what was set up and what to do next

## Key principles for generation

- **Use the templates as structure, not scripture.** The section headings and patterns should be preserved — they encode hard-won design decisions. But the content should be 100% personalized.
- **The email triage system is the most important file.** Get the tier logic right for their specific senders and topics. This is what prevents things from falling through the cracks.
- **CLAUDE.md must reference all generated files** with @ syntax so they auto-load every session.
- **Be opinionated in the triage rules.** Don't just list "CEO = Tier 1." Add the elevation rules: HIGH importance flag, escalation language detection, direct-to-user action requests. These patterns are universal.
- **The prep queue pattern is non-obvious but critical.** Explain it well: it's the "don't forget to bring this up" list that bridges meetings and conversations. Without it, action items from Tuesday's meeting never make it into Thursday's 1:1.
- **Name the assistant in CLAUDE.md** and use that name consistently. The personality section matters — it determines the tone of every interaction.
- **Don't generate empty files.** If a component is selected, it should have real, useful content — even if some sections say "TBD — fill in during first session." A file with good structure and a few real entries is infinitely better than a blank template.

## After generation

Tell me:
1. What was created and where
2. How to run my first morning brief (just ask the assistant in a new session)
3. What to fill in during the first week of use
4. How to add automation later if I want daily briefs sent to my email (link to the ShafOS website for the full automation guide)

## Important

- Ask questions conversationally, 3-5 at a time max
- Don't proceed to the next round until I've answered
- If I give short answers, ask smart follow-ups to get the detail you need
- If I seem unsure about something, give me a recommendation with reasoning
- The goal is a working system in 20 minutes, not a perfect one — we'll refine it over time
