# Daily Morning Pulse

**Cadence:** Every weekday at 9:00 AM local time
**Platform:** Copilot Studio + Power Automate
**Output channel:** Microsoft Teams channel post

---

## What This Does

Delivers a concise morning briefing to your team's Teams channel so everyone starts the day knowing what is due, what is stuck, and what moved yesterday. Without this, team leads spend 15–20 minutes manually scanning Planner boards and SharePoint list trackers each morning to assemble the same picture. This automation eliminates that overhead and ensures nothing slips through overnight.

## Sample Output

The following message appears in your Teams channel at 9:00 AM:

---

**Daily Pulse — Wednesday, March 18**

📋 **Due Today**
- NVIDIA content brief → @Erica (P1)
- Partner co-design review → @Mindy (P0 — overdue 1 day)

🔄 **In Progress (updated yesterday)**
- Build 2026 lab track outline — 60% → @Aaron
- Applied Skills attribution model — Phase 2 spec → @Amy

⛔ **Blocked**
- Innovation Studio registration flow → waiting on UX review from @Kai

📅 **Coming Up (next 48 hrs)**
- March 18: NVIDIA sync call
- March 19: Applied Skills weekly review

---

## How to Build This

### Step 1: Power Automate — Recurrence Trigger

Create a scheduled flow:
- **Trigger:** Recurrence
- **Interval:** 1 day
- **Frequency:** Day
- **Start time:** 9:00 AM (team timezone)
- **Days:** Monday, Tuesday, Wednesday, Thursday, Friday

### Step 2: Pull Data from M365 Sources

**From Planner (via Graph API or Planner connector):**
- Tasks due today: `dueDateTime le [today 11:59 PM] AND completedDateTime eq null`
- Tasks completed yesterday: `completedDateTime ge [yesterday midnight] AND completedDateTime le [yesterday 11:59 PM]`
- Tasks with no recent activity: `lastModifiedDateTime le [3 days ago] AND completedDateTime eq null`
- Tasks marked blocked (if using bucket or label convention)

**From SharePoint List / MS Lists (via SharePoint connector):**
- Items with `Target Date = today`
- Items with `Status = "Blocked"` or `Obstacles` field non-empty
- Items modified yesterday (`Modified >= yesterday midnight`)

### Step 3: Compose the Teams Message

> ⚠️ **Formatting gotcha:** Teams collapses plain-text line breaks in Power Automate messages. Use HTML-formatted content via the "Post a message in a chat or channel" action with `Content-Type: HTML`. Structure with `<b>` for headers and `<br>` for line breaks.

```html
<b>Daily Pulse — [Date]</b><br><br>
<b>📋 Due Today</b><br>
[Loop through due-today items: - Title → @Owner (Priority)]<br><br>
<b>🔄 In Progress (updated yesterday)</b><br>
[Loop through updated-yesterday items: - Title — Progress → @Owner]<br><br>
<b>⛔ Blocked</b><br>
[Loop through blocked items: - Title → waiting on...]<br><br>
<b>📅 Coming Up (next 48 hrs)</b><br>
[Loop through next-48h items: - Date: Title]
```

### Step 4: Post to Teams Channel

- **Action:** Post message in a chat or channel
- **Post as:** Flow bot (or Copilot Studio agent)
- **Post in:** Channel
- **Team:** [Your team name]
- **Channel:** [Your standup/pulse channel]
- **Message:** HTML-formatted content from Step 3

### Step 5: Handle Empty Sections

If a section has no items, post a placeholder rather than an empty header:

```html
<b>⛔ Blocked</b><br>
No blocked items today ✅
```

---

## Copilot Studio Variant

If using Copilot Studio instead of Power Automate directly:

1. Create a scheduled trigger in Copilot Studio (9:00 AM weekdays)
2. Invoke the `standup` skill
3. The skill pulls from Planner and SharePoint using Graph MCP connection
4. Post the formatted output to Teams via the "Post to Teams Channel" agent flow

The key advantage of the Copilot Studio path: the standup skill handles data-gap messaging gracefully ("No activity detected for [Name]") rather than requiring custom error handling in Power Automate.

---

## Data Source Configuration

Configure these before deploying:

| Setting | Value |
|---------|-------|
| Planner plan ID | [Your team's Planner plan ID — find in Planner URL] |
| SharePoint site URL | [Your team's SharePoint site] |
| SharePoint list name | [Your initiative tracker list name] |
| Teams channel | [Your pulse/standup channel] |
| Timezone | [Team timezone for 9 AM trigger] |
| Skip weekends | Yes (built into Recurrence trigger) |
| Skip holidays | Add condition: check against a holiday list or skip manually |

---

## Gotchas

1. **Teams HTML formatting.** Plain text loses line breaks. Always send as HTML. Test in the Power Automate "Test" pane before deploying.
2. **Planner task visibility.** The Power Automate Planner connector can only see tasks in plans you specify. If the team has multiple plans, you'll need to query each one or use Graph API with a broader scope.
3. **SharePoint List column names are case-sensitive** in Power Automate connectors. Confirm exact column names from your list settings.
4. **"Updated yesterday" includes minor edits.** A status-column change from "In Progress" to "In Progress" (same value) may not appear. Normalize status update discipline.
5. **No standup if nothing moved.** Consider sending a brief "All quiet — no updates detected" message rather than skipping the post entirely, so the team knows the automation ran.
