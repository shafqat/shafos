# ShafOS Bootstrap — Optimizely Edition

This is the Optimizely-specific version of the ShafOS bootstrap. It assumes you work at Optimizely and share the same tech stack: Outlook/Teams (O365), Salesforce, Coda, ClickUp, and Optimizely Analytics. It will walk you through connecting to each system and set up an AI Chief of Staff with the same capabilities as Bubs — email triage, Teams monitoring, meeting prep with transcript history, account deep dives, pipeline tracking, and daily briefings.

Copy everything below the line and paste it into Claude Code in an empty project directory that contains the `templates/` folder from this repo.

---

You are helping me set up a personalized AI Chief of Staff system. I work at Optimizely and share the same tech stack as the rest of the leadership team. Your job is to:

1. Interview me to understand my role and priorities
2. Walk me through connecting to our shared tools (O365, Salesforce, Coda, ClickUp, Optimizely Analytics)
3. Generate a fully personalized set of system files — modeled on the same architecture that powers Bubs (Shafqat Islam's AI Chief of Staff)
4. Verify everything works end to end

The goal: after setup, I should be able to run a morning brief, get my email and Teams triaged, prep for meetings, investigate any account on demand, and track pipeline — all from Claude Code.

## Part 1: Interview (10 minutes)

Ask me these questions conversationally, 3-5 at a time. Don't proceed to the next round until I've answered.

### Round 1: Who are you?
- What's your name and what do you want to name your assistant?
- What's your role at Optimizely? (title, what you oversee, how many people)
- Who are your direct reports? (names and roles)
- Who do you report to?

### Round 2: Your world
- What are your top 2-3 priorities right now?
- How do you prefer information delivered? (brief vs. detailed, narrative vs. bullets, lead with the point vs. build context first)
- What are your pet peeves in how people communicate with you?
- What's your schedule like? (office days, remote days, preferred meeting hours, timezone)

### Round 3: Your people
- Beyond your directs, who are the 5-10 people whose emails/Teams messages should ALWAYS be surfaced urgently? (e.g., Alex, board members, key customers)
- Who are the people you meet with weekly? (name, role, day/time)
- Any key external relationships? (customers, board members, partners, analysts)
- Which Teams DMs and group chats are most important to you? (list them — we'll set up priority monitoring)

### Round 4: What do you want?
Based on what you've told me, present the full set of capabilities and let them prioritize:

**Core (recommended for everyone):**
- Email triage (Outlook inbox prioritization with tiered sender rules)
- Teams monitoring (DMs + priority group chats)
- Meeting prep (scaled by meeting type, with transcript history)
- Account deep dives (Salesforce-powered investigations on demand)
- Project tracking (active projects + prep queue)

**Daily briefs (recommended for leaders):**
- Morning brief (overnight email + Teams catch-up, today's calendar)
- Evening brief (next-day prep, loose ends, advance meeting radar)
- Weekly review (Sunday bridge — past week recap, next week preview, priorities check)

**Data access (set up based on role):**
- Pipeline & bookings metrics (Optimizely Analytics)
- Onboarding status (ClickUp)
- Opal strategy docs (Coda) — only if they work on Opal

Recommend a starting set based on their role.

## Part 2: Tool Setup (10 minutes)

Walk through each integration one at a time. For each tool, explain what it does, guide them through setup, and verify it works with a real test. **Do not move to the next tool until the current one is verified.**

### 2a. Microsoft 365 (Outlook + Teams + Calendar) — MOST IMPORTANT

O365 is the foundation — email triage, Teams monitoring, calendar access, and meeting transcripts all depend on it.

**Setup steps:**
1. Check if the O365 MCP is already configured — look in Claude Code settings for "Office 365" or "O365"
2. If not configured:
   - In Claude Code, go to settings and add the `claude.ai/Office 365` MCP integration
   - Authenticate with their Optimizely Microsoft account when prompted
3. **Verify email works** — pull their inbox:
   Use `read_resource` with URI `mail:///folders/Inbox`. This should return their most recent ~50 emails.
4. **Verify calendar works** — pull today's calendar:
   Use `outlook_calendar_search` with today's date range.
5. **Verify Teams works** — search for a recent message:
   Use `chat_message_search` with `query: "*"` and `afterDateTime` set to yesterday. This should return recent Teams messages.

**Tell them the key O365 patterns:**
- Inbox folder pull (`read_resource` on `mail:///folders/Inbox`) returns the most recent ~50 messages chronologically — this is the PRIMARY triage method, not keyword search
- For broader email coverage, use `outlook_email_search` with `query: "*"` and `afterDateTime` — always use `*` for broad sweeps (empty string returns nothing)
- For Teams, wildcard sweep (`chat_message_search` with `query: "*"`) discovers ALL active chats. Paginate with `offset` until `moreResults` is false
- Teams chat type is deterministic from the ID: `@unq.gbl.spaces` = DM, `@thread.v2` = group chat
- Always check Sent Items before flagging an email as "unanswered" — people reply from different devices
- Always convert UTC timestamps to Eastern Time before presenting

### 2b. Salesforce CLI

Salesforce provides deal data, email activity logged by AEs/CSMs, meeting transcripts (via Zoom Revenue Accelerator), support cases, and account team rosters. This is what powers account deep dives.

**Setup steps:**
1. Check if `sf` CLI is already installed: `sf --version`
2. If not installed: `brew install sf` (Mac) or `npm install -g @salesforce/cli`
3. Authenticate to the Optimizely org:
   ```
   sf org login web --alias optimizely --instance-url https://optimizely.my.salesforce.com
   ```
   This opens a browser — log in with Optimizely SSO credentials.
4. **Verify it works** — run a test query:
   ```
   sf data query --query "SELECT Id, Name FROM Account WHERE Name LIKE '%Dell%' LIMIT 3" --target-org optimizely
   ```

**Tell them the key Salesforce patterns:**
- **Always report ARR in USD, never TCV.** Formula: `TotalPrice / Currency_Rate__c / (Subscription_Term__c / 12)`. Opportunity-level shortcut: `Recurring_Software_Amount__c`. Multi-currency org — raw amounts can be in GBP, EUR, SEK, etc.
- **Closed Won stages:** Both `'Closed Won'` AND `'Closed Won - Finance'` (majority of booked deals use the latter)
- **Email activity** is in the `Task` object. Three systems sync: Clari (`[Clari -` prefix), Outreach (`[Outreach]` prefix), and Groove (boolean field). When you query Tasks for an account, you get ALL email activity.
- **Meeting transcripts** via Zoom Revenue Accelerator: `ZVC__Zoom_ZRA_Analysis__c` — covers BOTH Zoom and Teams meetings (Zoom bot joins Teams calls). Has AI summaries, action items, competitor mentions, sentiment scores, engagement scores.
- **Historical transcripts** in Gong: `Gong__Gong_Call__c` — Oct 2020 to Oct 2024 (no longer syncing). Use for historical lookups only.

### 2c. ClickUp (Optional — for onboarding/CS visibility)

ClickUp tracks all professional services and onboarding projects. Space: "Professional Services Customers", organized by account folders.

**Setup steps:**
1. Check if ClickUp MCP is configured in Claude Code settings
2. If not, add the ClickUp MCP and authenticate
3. **Verify it works** — search for a known account:
   Use `clickup_search` with an account name + "onboarding"

### 2d. Coda (Optional — for Opal team members)

Coda has the Opal Platform doc with roadmaps, weekly meeting notes, sprint planning, and strategy docs.

**Setup steps:**
1. Check if Coda MCP is configured in Claude Code settings
2. If not, add it and authenticate
3. **Verify it works** — list pages in the Opal Platform doc (doc ID: `0f_sY3MSh-`)

**Only set this up if they work on Opal.**

### 2e. Optimizely Analytics (Optional — for pipeline/metrics tracking)

Provides access to pipeline, bookings, adoption, and usage dashboards.

**Setup steps:**
1. Check if the Optimizely Analytics MCP is configured
2. If not, add it and authenticate
3. **Verify it works** — use `oa_list_apps` to confirm access. The main app is "Production Analytics (Opti on Opti)" — appid: `8659eda3-d769-4af3-b93b-87bc0d6c8238`. Must pass `appid` on every call.

**Useful dashboard:** Opal ELT Summary Dashboard (id: `434141130_8659eda3-d769-4af3-b93b-87bc0d6c8238`) has 54 explorations covering pipeline, bookings, ARR, adoption, and attach rates.

## Part 3: Generate the System Files

Now read every relevant template in `templates/` and generate the personalized system. Use templates for structure, personalize content based on the interview.

**Optimizely-specific content to bake into the generated files:**

### CLAUDE.md — Add these sections:

**Salesforce Access:**
```
## Salesforce Access
- Authenticated to Optimizely org (alias: `optimizely`)
- Use `sf data query --query "SOQL" --target-org optimizely` for queries
- Multi-currency org — always convert to USD
- Always report ARR, not TCV: ARR = TotalPrice / Currency_Rate__c / (Subscription_Term__c / 12)
- Opp-level shortcut: Recurring_Software_Amount__c
- Closed Won stages: both 'Closed Won' AND 'Closed Won - Finance'
```

**Account Deep Dive methodology:**
```
## Account Deep Dive

When asked to investigate an account, pull these data sources in parallel:

1. **Deal data** — Opportunities with ARR, products, stage, close dates
2. **Account team** — AE, CSM, SDR, Exec Sponsor from AccountTeamMember
3. **Email activity (last 180 days)** — Tasks with [Clari/[Outreach] prefixes (captures CSM/AE emails)
4. **Meeting transcripts (last 90 days)** — ZVC__Zoom_ZRA_Analysis__c (covers Zoom AND Teams calls) — summaries, action items, competitor mentions, sentiment/engagement scores
5. **Support cases** — Open/recent cases from Case object
6. **Contact roles** — OpportunityContactRole for key stakeholders
7. **Onboarding status** — ClickUp search if available

Analysis framework:
1. Account overview — ARR, products, deal history, key contacts
2. Relationship health — email frequency trends, sentiment from transcripts, engagement scores
3. Risk signals — competitor mentions, objections, support escalations, communication gaps
4. Onboarding/adoption status — are they using what they bought?
5. Timeline reconstruction — key events from deal close to present
6. Recommended actions — specific next steps with suggested owners
```

**O365 Usage Patterns:**
```
## O365 Email & Teams Patterns
- Primary inbox pull: `read_resource` with URI `mail:///folders/Inbox` (most recent ~50 messages)
- Broad email search: `outlook_email_search` with `query: "*"` (never empty string)
- Teams wildcard sweep: `chat_message_search` with `query: "*"` — paginate until `moreResults` is false
- Chat type from ID: `@unq.gbl.spaces` = DM, `@thread.v2` = group, `meeting_` prefix = meeting chat
- Always check Sent Items before flagging loose ends
- Always convert UTC to Eastern Time
```

**Meeting Transcript Retrieval (from O365):**
```
## Meeting Transcripts (O365)
- URI format: `meeting-transcript:///events/{URL-encoded joinWebUrl}`
- Method: Read calendar event → extract Teams join URL from body → URL-encode → pull transcript
- The join URL must include the `?context=` block with Tid and Oid
- Optimizely Tenant ID: 3ec00d79-021a-42d4-aac8-dcb35973dff2
- Transcripts are large (~180K chars for 50 min) — use subagents to process
```

### email-triage.md — Optimizely-specific tiers:

Build the triage system with these Optimizely-universal patterns:
- **Alex Atzberger (CEO)** = always Tier 1
- **ELT members** = Tier 1-2 depending on topic (Myles Johnson, Rupali Jain, Aniel Sud, Tara Corey, Sebastiaan de Jong, Allison Skidmore, Christopher Bayliss, Laura Thiele, Peter Yeung)
- **Board / Insight Partners** (Adam Berger) = always Tier 1
- **Field team escalating a customer issue** = Tier 1 regardless of sender's title or reporting line
- **HIGH importance flag** = auto-elevate to at least Tier 2
- **Escalation keywords** in subject/body = auto-elevate ("refusing to pay," "lost trust," "threatening to leave," "churn risk," "cancel," "competitive risk")
- **Internal sender asking user to email a customer** = Tier 1 (customer is effectively waiting)

Then layer the user's personal Tier 1/2 senders on top (from the interview).

### professional.md — Include Optimizely company context:

Add this so the assistant has company context from day one:
- Optimizely is a digital experience platform (DXP) for marketing and product teams
- Core products: CMS (SaaS + PaaS), Experimentation (A/B testing), CMP (Content Marketing Platform), Feature Flagging, ODP, Analytics, DAM
- Customers: mid-market to enterprise
- Formed from merger of Optimizely and Episerver; backed by Insight Partners
- "Optimizely One" is the unified platform vision
- Opal is the AI orchestration platform — the single most important strategic initiative
- CEO is Alex Atzberger
- ~800+ employees across Product, Engineering, Marketing, Sales, CS

## Part 4: Verification

Run through these tests to prove the system is working. Do them in order — each one exercises a different integration.

### Test 1: Email Triage
Pull the inbox and triage the last 20 emails using the tier system. Present results grouped by tier. Ask: "Does this prioritization match your intuition? Anyone missing from Tier 1?"

### Test 2: Teams Check
Run a Teams wildcard sweep for the last 24 hours. Surface all DMs and any messages from priority group chats. Ask: "Did we catch everything important?"

### Test 3: Calendar + Meeting Prep
Pull tomorrow's calendar. For the most important meeting, run the meeting prep framework — who's in the room, purpose, recent email threads with attendees, what's needed from the user.

### Test 4: Account Deep Dive
Ask them to name an account they know well. Run the full deep dive — Salesforce deal data, email activity, meeting transcripts, support cases, account team. Synthesize into the analysis framework. Ask: "Does this match your understanding? What did it get right? What did it miss?"

### Test 5: Pipeline Check (if Analytics MCP is set up)
Pull the key Opal pipeline numbers from the ELT dashboard — pipe gen this quarter and open pipe with next quarter close date. Present with context against targets.

**For each test that fails**, troubleshoot the underlying integration before moving on. A broken integration is worse than no integration.

**After all tests pass**, tell them:
1. Try "morning brief" tomorrow morning — it'll catch up on overnight email and Teams, show today's calendar, and flag what needs attention
2. Try "prep me for [meeting name]" before any meeting — it'll pull attendee context, recent threads, and suggested talking points
3. Try "deep dive on [account]" anytime — works for any of Optimizely's thousands of accounts
4. The system gets smarter over time — whenever it gets something wrong, tell it. It will ask if the correction should become a permanent rule.
5. Fill in projects and priorities during the first week — the more context the system has, the better it gets

## Important

- Walk through tool setup ONE AT A TIME — verify each before moving to the next
- If a tool setup fails, troubleshoot before continuing
- The verification tests are not optional — they prove the system works and give the user confidence
- Ask questions conversationally, 3-5 at a time max
- The goal is a fully working system in 20-30 minutes
- Every Optimizely user should end up with at least: email triage, Teams monitoring, meeting prep, and account deep dives working
