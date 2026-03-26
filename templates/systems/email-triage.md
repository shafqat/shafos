# Email & Messaging Triage System

<!-- This is the most important system file. It determines what gets surfaced
     and what gets filtered. Refine it aggressively based on feedback —
     every "you should have shown me that" or "stop showing me those" is
     a rule to add here. -->

## Priority Tiers

### Tier 1 — Respond Now
Emails that need immediate attention or same-day response.

**Senders:**
<!-- List people whose emails are ALWAYS urgent.
     Example:
- CEO
- Board members
- Customers or prospects (external emails related to active deals) -->

**Signals that auto-elevate to Tier 1 (regardless of sender):**
- Emails marked HIGH importance — the sender thinks it's urgent, respect that
- Sent directly TO {{USER_NAME}} with an explicit action request (not just CC'd)
- Internal sender asking {{USER_NAME}} to send a customer-facing email — a customer is effectively waiting
- Escalation language — scan for: "refusing to pay," "lost trust," "threatening to leave," "churn risk," "cancel," "legal," "security incident," "urgent escalation"

**Topics (regardless of sender):**
- Customer escalations or complaints
- Deal approvals or pricing decisions that are blocking
- Anything where someone external is waiting on {{USER_NAME}}

### Tier 2 — Respond Today
Important but not time-critical. Can be batched.

**Senders:**
<!-- Direct reports and key leaders who regularly need input -->

**Topics:**
- Internal asks with a deadline this week
- Meeting prep materials for upcoming meetings
- Key initiative updates

### Tier 3 — Batch Process
Low urgency, handle when convenient.

- FYI threads where {{USER_NAME}} is CC'd but not directly asked for input
- Internal announcements
- Meeting notes or recaps

### Tier 4 — Ignore / Archive
Skip entirely unless specifically asked.

- Marketing emails, newsletters, vendor outreach
- Automated notifications
- Mass distribution emails with no action required

## Search Methodology

<!-- This section codifies HOW to search email. It prevents the common failure
     mode of keyword search missing things from unexpected senders. -->

**Mandatory search pattern for every triage session:**

1. **Inbox folder pull (PRIMARY)** — Pull the raw inbox chronologically. This catches everything regardless of sender or subject. Always do this FIRST.
2. **Extend coverage** — If the first pull doesn't reach back to the last triage point (morning brief to evening, evening to morning), run additional searches with broad queries until you have full coverage.
3. **Targeted sender searches (SAFETY NET)** — After full inbox coverage, run sender-filtered searches for Tier 1 senders. This catches emails that may have landed in non-Inbox folders.
4. **Sent Items check (BEFORE flagging loose ends)** — Before telling {{USER_NAME}} they haven't replied to something, check Sent Items. People respond from different devices, and the inbox thread may not show it. This prevents embarrassing false alarms.

<!-- The inbox-first approach was learned the hard way: keyword and sender searches
     will always miss emails from unexpected senders. The raw inbox pull is the
     only reliable foundation. -->

## Messaging Triage (Teams / Slack)

<!-- If you use Teams or Slack, add similar triage rules here.
     Key principles:
     - DMs take priority over group chats (someone chose to reach you specifically)
     - List priority channels/chats by name
     - A broad sweep catches things keyword search misses -->

### Priority Chats
<!-- List the DMs and group chats that always get surfaced.
     Example:
- CEO DM — always Tier 1
- Leadership Team channel — always Tier 2
- [Project] channel — Tier 2 when active -->

## Filtering Guidance

- **External senders with company domains** — likely customers/prospects, check and elevate
- **Distribution lists** — the sender and subject determine priority, not the list name
- **Promotional/marketing senders** — almost always Tier 4
- **When in doubt, err toward surfacing it** — better to show something unnecessary than to bury something critical

## Triage Output Format

For each email surfaced, include:
- **Sender** and their context (role, company, relationship)
- **Subject** and **one-line summary**
- **Suggested action**: reply, delegate to [person], schedule time, or just FYI
- **Urgency signal**: why it's in this tier

Group by tier. Within each tier, order by time-sensitivity.
