---
name: escalation-tracker
description: Tracks items escalated to leadership with SLA monitoring. Dashboard of pending, responded, and resolved escalations. Creates new escalations with options and recommendations. Stores escalations in SharePoint List with Teams notifications.
allowed-tools: [Bash, Read, Grep, Glob]
disable-model-invocation: true
---

# Escalation Tracker

**Type:** Governance & Accountability
**Speed:** ~20 seconds
**Storage:** SharePoint List (escalation log) + Teams notification

## What This Skill Does

Tracks all upward escalations to leadership with SLA awareness. Ensures items sent up the chain get responses and outcomes recorded. Nothing escalated should ever get lost.

1. **Dashboard** — Pending, responded, resolved escalations at a glance
2. **SLA Monitoring** — Configurable expected response times by leadership tier
3. **Create Escalation** — Structured template with options and recommendation
4. **Aging Alerts** — Flag items past their expected response window

## Triggers

```
/escalations                # Full escalation dashboard
/escalations --pending      # Only pending items (awaiting response)
/escalations --overdue      # Only items past SLA
/escalations --create       # Create a new escalation
/escalations --to [name]    # Filter by escalation target
```

## Data Source

Escalations are tracked in a dedicated **SharePoint List** (recommended name: "Escalation Tracker"). This list is the single source of truth — the skill reads from it and writes to it via Graph API or SharePoint connector.

**Required columns:**

| Column | Type | Description |
|--------|------|-------------|
| Title | Text | Brief escalation summary |
| Escalation_To | Person | Who it was escalated to |
| Raised_By | Person | Who raised the escalation |
| Date_Raised | Date | When it was escalated |
| Severity | Choice | High / Medium / Low |
| Status | Choice | Pending / Responded / Resolved / Closed |
| Description | Multi-line text | Full context and ask |
| Recommended_Action | Multi-line text | What you're asking for |
| Options | Multi-line text | 2–3 options considered |
| Response | Multi-line text | Leadership's response (filled in after) |
| Date_Resolved | Date | When resolved (filled in after) |
| SLA_Days | Number | Expected response window in days |
| Related_Initiative | Lookup | Link to initiative in your main tracker |

> **Note:** If your team uses GitHub Issues, you can supplement this with GitHub issues tagged `status:waiting-approval` and `stakeholder:[name]`. But the SharePoint List is the primary store — it works without GitHub.

## Prerequisites

- SharePoint List "Escalation Tracker" created with the schema above
- Microsoft Graph MCP connection (for read/write)
- Teams channel for escalation notifications
- Configured SLA thresholds per leadership tier (see below)

## Implementation

### SLA Configuration

```
Tier 1 (Direct manager):        2 business days
Tier 2 (Skip-level / Director): 3 business days
Tier 3 (VP / CVP):              5 business days
Default (unclassified):         3 business days
```

### `/escalations` — Full Dashboard

Query the SharePoint List:

```
Filter: Status != "Closed"
Group by: Status (Pending / Responded / Resolved)
Compute: days_open = (today - Date_Raised)
Flag: days_open > SLA_Days → Overdue
```

Output format:

```markdown
# Escalation Dashboard — [Date]

## Summary
| Status | Count | Overdue |
|--------|-------|---------|
| Pending | X | X |
| Responded | X | — |
| Resolved | X | — |

## 🔴 Overdue — Needs Follow-Up

| Item | Escalated To | Raised | Days Open | SLA |
|------|-------------|--------|-----------|-----|
| [Title] | [Name] | [Date] | X | X |

## 🟡 Pending — Awaiting Response

| Item | Escalated To | Raised | Days Open |
|------|-------------|--------|-----------|
| [Title] | [Name] | [Date] | X |

## ✅ Responded — Action Taken

| Item | Escalated To | Response Summary |
|------|-------------|-----------------|
| [Title] | [Name] | [Summary] |
```

### `/escalations --create` — New Escalation

Prompt the user for:

```
Title: [Brief summary — what are you escalating?]
Escalated to: [Who needs to decide or unblock this?]
Severity: High / Medium / Low
Context: [What's the situation? What's at stake?]
Options considered: [2–3 options you've evaluated]
Recommendation: [What you're asking for]
SLA needed: [When do you need a response?]
```

Then:
1. Create a new item in the SharePoint List with Status = "Pending"
2. Post a Teams notification to the escalation target (if configured)
3. Return a confirmation with the escalation link

**Teams notification to escalation target (HTML):**

```html
🔴 <b>Escalation: [Title]</b><br><br>
Raised by: @[Raised_By]<br>
Severity: [High/Medium/Low]<br>
Response needed by: [Date based on SLA]<br><br>
<b>Context:</b> [Description]<br><br>
<b>Recommendation:</b> [Recommended_Action]<br><br>
<a href="[SharePoint item URL]">View escalation →</a>
```

### Aging Alerts

Run automatically as part of the Monday `weekly-board-health` flow, or on-demand via `/escalations --overdue`. Items past SLA get surfaced for follow-up.

## Gotchas

- **Escalations live in SharePoint, not chat.** Don't use Teams messages as the escalation record — they get buried. The SharePoint List is the audit trail.
- **"Responded" ≠ "Resolved."** Update the Status and Response fields when the escalation target responds. "Resolved" means the action was taken or the decision was made.
- **SLA clocks run on business days.** If you're computing SLA in Power Automate, use a business-day calculation or a holiday-aware function — not just calendar days.
- **Close escalations when done.** Stale "Responded" escalations that should be closed clutter the dashboard. Add a weekly prompt to close items that have been responded to.

## Related Skills

- `/weekly-status` — Surfaces blockers that may need escalation
- `/1on1-prep` — Pulls open escalations for manager prep
- `/mbr` — Monthly rollup includes escalation section
