# Weekly Tracker Health Check

**Cadence:** Every Monday at 8:00 AM local time
**Platform:** Copilot Studio + Power Automate
**Output channel:** Microsoft Teams channel post

---

## What This Does

Performs a hygiene audit across your SharePoint List initiative tracker and Planner boards every Monday morning. It catches the items that silently rot — stale rows no one has touched in two weeks, initiatives missing owners, overdue tasks, and Planner tasks not linked to any initiative in the tracker.

Without this check, trackers degrade over time and program health becomes invisible until something breaks at review time.

## Sample Output

The following message appears in your Teams channel on Monday morning:

---

**Tracker Health Check — Week of March 16**

⚠️ **3 items need attention**

🔴 **Overdue (past target date)**
- Partner brief — Build 2026 → @Erica — 5 days overdue
- Applied Skills dashboard Phase 2 → @Amy — 12 days overdue

🟡 **Stale (14+ days no update)**
- TOGO 2.0 coach deployment → @Sweta — last updated March 1

❌ **Missing Owner**
- Innovation Studio localization — no owner assigned

✅ **No orphaned tasks detected**

---

## How to Build This

### Step 1: Power Automate — Recurrence Trigger

- **Trigger:** Recurrence
- **Interval:** 1 week
- **Frequency:** Week
- **Days:** Monday
- **Start time:** 8:00 AM (team timezone)

### Step 2: Run Hygiene Checks

**Overdue items (SharePoint List):**
```
Filter: Target_Date < today AND Status != "Complete"
Output: Title, Owner, Target_Date, (today - Target_Date) as days_overdue
Sort: days_overdue descending
```

**Stale items (SharePoint List):**
```
Filter: Modified < (today - 14 days) AND Status != "Complete"
Output: Title, Owner, Modified as last_updated
Sort: last_updated ascending (oldest first)
```

**Missing owners (SharePoint List):**
```
Filter: Owner = null OR Owner = "" AND Status != "Complete"
Output: Title, Status, Target_Date
```

**Missing owners (Planner):**
```
Query: Tasks with no assignee, incomplete
Output: Task title, Plan name, Due date
```

**Orphaned tasks (Planner vs. SharePoint List):**
```
For each open Planner task:
  Check if task title substring-matches any active SharePoint List item title
  If no match: flag as orphaned
Output: Task title, Assignee, Due date
```

### Step 3: Compose Teams Message (HTML)

> ⚠️ **Formatting gotcha:** Use HTML formatting. Teams collapses plain-text line breaks in Power Automate posts.

```html
<b>Tracker Health Check — Week of [Date]</b><br><br>
[If issues found:]
⚠️ <b>[count] items need attention</b><br><br>
[If overdue items:]
🔴 <b>Overdue (past target date)</b><br>
[Loop: - Title → @Owner — X days overdue]<br><br>
[If stale items:]
🟡 <b>Stale (14+ days no update)</b><br>
[Loop: - Title → @Owner — last updated [date]]<br><br>
[If missing owners:]
❌ <b>Missing Owner</b><br>
[Loop: - Title — no owner assigned]<br><br>
[If orphaned tasks:]
🔗 <b>Orphaned Tasks (not linked to any initiative)</b><br>
[Loop: - Task title → @Assignee]<br><br>
[If no issues:]
✅ <b>Tracker is clean — no issues detected</b>
```

### Step 4: Post to Teams Channel

- **Action:** Post message in a chat or channel (HTML)
- **Channel:** Team channel (standup, general, or ops channel)

---

## Copilot Studio Variant

If using Copilot Studio:

1. Create a Monday 8:00 AM scheduled trigger
2. Invoke the `issue-triage` skill (now called "Tracker Triage")
3. Skill runs checks against SharePoint List and Planner via Graph MCP
4. Post result to Teams channel via agent flow

---

## Data Source Configuration

| Setting | Value |
|---------|-------|
| SharePoint site URL | [Your team's SharePoint site] |
| Initiative list name | [Your tracker list — e.g., "Initiative Tracker"] |
| Planner plan ID(s) | [All active Planner plans for the team] |
| Stale threshold | 14 days (adjust per team preference) |
| Teams channel | [Health check output channel] |

---

## Gotchas

1. **Orphan detection is fuzzy.** There's no native parent-child link between Planner tasks and SharePoint List items in most M365 setups. Orphan detection works on name-matching — it improves if your team uses consistent naming conventions.
2. **SharePoint List grounding caveat.** If using Copilot Studio to ground the agent on SharePoint List data: SharePoint List as a Copilot Studio knowledge source is a staged rollout feature and may not be available in all tenants. Use the Power Automate path to generate a Teams post from list data if list-grounding isn't available yet.
3. **"Stale" is relative to team update cadence.** 14 days is a reasonable default, but adjust to match your team's actual update rhythm. If standups happen daily, 7 days may be more appropriate.
4. **Modified date includes system-triggered changes.** A SharePoint rule triggering a column update will update the Modified field even if no human touched the item. Add a custom "Last Human Updated" date column if this is a concern.
5. **Don't auto-close items.** The health check surfaces problems — owners close or reassign items. Never auto-delete or auto-status-change from this flow.
