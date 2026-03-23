# Weekly Friday Digest

**Cadence:** Every Friday at 2:00 PM local time
**Platform:** Copilot Studio + Power Automate
**Output:** Teams channel post + email to distribution list

---

## What This Does

Sends a week-in-review digest to the team channel and via email every Friday afternoon. Follows the format used by Microsoft Solutions & Partner Marketing teams — the "Weekly Wrap-up" pattern: what shipped, what's coming, what's blocked, links for fast navigation.

It replaces the manual end-of-week recap that someone would otherwise have to write by hand, and gives the team a shared sense of momentum going into the weekend.

Dual output matches your org's actual pattern: Teams channel post for the team, email for broader distribution and leadership visibility.

## Sample Output

**Team channel post:**

---

**Week in Review — March 16–20**

✅ **What Shipped**
- NVIDIA content brief v1 delivered to Red Thread — @Erica
- Build 2026 lab track outline locked — @Mindy
- Applied Skills attribution model spec sent to Nextant — @Amy

📅 **Coming Next Week**
- March 23: NVIDIA partner sync
- March 24: Applied Skills weekly review
- March 25: Innovation Studio UX walkthrough

⛔ **Blockers / Asks**
- Build 2026 DevRel alignment — needs @Mark Winters decision by March 25
- Innovation Studio registration flow — UX fix ETA TBD

🔗 **Links**
- [Initiative tracker](link)
- [NVIDIA content brief](link)
- [Build 2026 lab outline](link)

---

**Email version:** Same content, sent to team DL + leadership as needed.

---

## How to Build This

### Step 1: Power Automate — Recurrence Trigger

- **Trigger:** Recurrence
- **Interval:** 1 week
- **Frequency:** Week
- **Days:** Friday
- **Start time:** 2:00 PM (team timezone)

### Step 2: Pull Week's Data from M365 Sources

**What shipped this week (SharePoint List):**
```
Filter: Status = "Complete" AND Modified >= [Monday of this week]
Output: Title, Owner, Modified date
Sort: Modified descending
Limit: Top 5 (surface the most recent)
```

**What shipped this week (Planner):**
```
Filter: completedDateTime >= [Monday of this week]
Output: Task title, Assignee, completedDateTime
```

**Blockers / At-risk (SharePoint List):**
```
Filter: Status = "Blocked" OR Obstacles != null AND Status != "Complete"
Output: Title, Owner, Obstacles, Target_Date
```

**Coming next week (SharePoint List):**
```
Filter: Target_Date >= [next Monday] AND Target_Date <= [next Friday] AND Status != "Complete"
Output: Title, Owner, Target_Date
Sort: Target_Date ascending
Limit: Top 5
```

**Links:** Pull from a "key documents" list or use fixed SharePoint links to the initiative tracker and recent deliverables.

### Step 3: Compose Teams Message (HTML)

> ⚠️ **Formatting gotcha:** Use HTML. Teams collapses plain-text newlines in Power Automate posts.

```html
<b>Week in Review — [Monday Date]–[Friday Date]</b><br><br>
✅ <b>What Shipped</b><br>
[Loop: - Title — @Owner]<br><br>
📅 <b>Coming Next Week</b><br>
[Loop: - Date: Title]<br><br>
⛔ <b>Blockers / Asks</b><br>
[Loop: - Title — needs [ask] from @[person] by [date]]<br>
[If none: No blockers this week ✅]<br><br>
🔗 <b>Links</b><br>
[Initiative tracker] | [Key deliverable 1] | [Key deliverable 2]
```

### Step 4: Post to Teams Channel

- **Action:** Post message in a chat or channel (HTML)
- **Channel:** Your team's general or comms channel

### Step 5: Send Email Version

- **Action:** Send an email (V2) via Office 365 Outlook connector
- **To:** Team DL (e.g., teamalias@microsoft.com)
- **CC:** Leadership if configured
- **Subject:** `[Team Name] Weekly Wrap-up — [Month DD]`
- **Body:** Same content as Teams post, formatted as plain HTML email

---

## Copilot Studio Variant

1. Create a Friday 2:00 PM scheduled trigger
2. Invoke the `peer-digest` skill (which already produces dual Teams+email output)
3. Skill pulls from SharePoint List and Planner via Graph MCP
4. Post to Teams via agent flow + send email via Outlook connector

---

## Data Source Configuration

| Setting | Value |
|---------|-------|
| SharePoint site URL | [Your team's SharePoint site] |
| Initiative list name | [Your tracker list name] |
| Planner plan ID(s) | [Active Planner plans] |
| Teams channel | [Week-in-review channel] |
| Email DL | [Team distribution list] |
| Leadership CC | [Optional — leaders to CC] |
| Max items per section | 5 (adjust per team size) |

---

## Gotchas

1. **"What shipped" vs. "what got marked complete."** Some items get marked complete in batches at end of week. Others may have shipped but weren't marked complete. The digest reflects tracker state, not reality — set that expectation with the team.
2. **Email formatting differs from Teams.** The Teams HTML post and the email body use the same content but may render differently. Test both before deploying.
3. **Links to SharePoint documents require correct permissions.** If the email goes to a broad DL, confirm that linked documents are accessible to all recipients.
4. **Don't flood.** If the team has 20+ completions in a week, cap the digest at 5 highlights with a "see full tracker" link rather than listing everything.
5. **Dual-publish is the right pattern.** Teams post for the team (scannable, fast), email for leadership + discoverability. Both should link back to the live initiative tracker as the source of truth.
