---
name: weekly-status
description: Generates a weekly status summary for any Solutions Marketing team member. Follows the Key Focus / Update / Next Step / Risk format from Microsoft's Skilling-Weekly-Status-Deck template. Pulls from Planner, SharePoint Lists, Loop trackers, or manual input.
allowed-tools: [Bash, Read, Grep, Glob]
---

# Weekly Status Generator

**Type:** Weekly Operations
**Speed:** ~20 seconds
**Output:** Teams post + deck-ready bullets (matches Skilling Weekly Status Deck format)

## What This Skill Does

Generates a weekly status summary for any team member or the full team, using the format observed in Microsoft Solutions & Partner Marketing:

1. **Key Focus** — What this person/team is driving this week
2. **Update** — What shipped or moved forward
3. **Next Step** — What's coming next week
4. **Risks / Flags** — What needs attention or escalation

Also produces:
- **Completed This Week** — Deliverables finished, grouped by owner
- **In Progress** — Active workstreams with RAG status
- **Blocked / At-Risk** — Items needing escalation

## Triggers

```
/weekly-status              # Prompts for which person (or shows all)
/weekly-status [name]       # Specific person's status
/weekly-status --deck       # Format for weekly status deck (bullet-only)
/weekly-status --all        # Full team rollup
```

## Data Sources

| Source | What It Contains | Priority |
|--------|-----------------|----------|
| **SharePoint Lists / MS Lists** | Initiative status, owner, progress %, target date, obstacles | Primary |
| **Planner / Tasks** | Tasks completed/in-progress/overdue by person this week | Primary |
| **Loop tables** | Initiative fields: Priority / Motion / Owner / Progress / End date / Obstacles | Secondary |
| **Weekly status deck (running)** | Prior week's update for continuity | Context only |
| **Manual input** | User pastes their own update | Fallback |

> **Note:** If the team uses GitHub Issues, substitute `gh issue list` queries filtered by `closed:>=WEEK_START` and `owner:[name]` labels.

## Implementation

### Step 1: Set Date Window

```
WEEK_START = Monday of current week (or prior week if run Monday)
WEEK_END   = Friday of current week
NEXT_WEEK  = Following Monday
```

### Step 2: Gather Data Per Section

**Completed This Week:**
- Planner: tasks with completion date within the week window, grouped by assignee
- SharePoint List: items moved to "Complete" status with date in window
- Loop table: rows where Progress = 100% updated this week

**In Progress (with RAG):**
- Planner: open tasks due this week or next week
- SharePoint List: items with Status = "In Progress"
- Assign RAG:
  - 🟢 **Green** — On track, no blockers
  - 🟡 **Yellow** — At risk, flagged obstacle or slipping date
  - 🔴 **Red** — Blocked or overdue

**Blocked / At-Risk:**
- Planner: tasks with blocker flag or overdue by 3+ days
- SharePoint List: items with content in "Obstacles" column
- Loop: rows with non-empty Obstacles field

**Next Week Preview:**
- Planner: tasks due next week
- SharePoint List: items with target date in the following week
- Top 3 per person

### Step 3: Format Output

**Standard format (Teams post):**

```markdown
## Weekly Status — [Name] — Week of [Date]

**Key Focus:** [What they're driving this week — 1 sentence]

**Update:**
- ✅ [Completed item 1 — outcome, not activity]
- ✅ [Completed item 2]
- 🔄 [In-progress item — current status]

**Next Step:**
- [Priority 1 next week]
- [Priority 2 next week]

**Risks / Flags:**
- 🔴 [Blocked item] — needs [decision/asset] from @[person] by [date]
- 🟡 [At-risk item] — [brief reason]
```

**Deck format (`--deck`):**

Bullet-only, no markdown headers. Designed for direct paste into the Skilling-Weekly-Status-Deck template:

```
Key Focus: [1 sentence]
Update: [bullet] / [bullet] / [bullet]
Next Step: [bullet] / [bullet]
Risks: [bullet or "None"]
```

**Full team rollup (`--all`):**

Table format — one row per person, 4-column (Key Focus / Update / Next Step / Risk).

### Step 4: Handle Missing Data

If no connected source is available:
```
⚠️ No data source connected for [Name].

To generate status, please provide:
1. What did you complete this week?
2. What are you working on now?
3. What's coming next week?
4. Any blockers or risks to flag?
```

## Gotchas

- **RAG status requires team discipline.** If SharePoint list Status fields aren't updated, RAG assessment defaults to Yellow for anything past target date.
- **"Completed" vs "done done."** Distinguish between "draft delivered" and "published/launched." The update should reflect outcome-level completions, not intermediate steps.
- **Keep Key Focus to one sentence.** The weekly status deck format has limited space — one sentence per section is the norm.
- **The deck format and Teams format serve different audiences.** Deck goes to leadership; Teams post goes to the team. Tone and level of detail should differ.

## Related Skills

- `/standup` — Daily version of the same signals
- `/1on1-prep` — Uses weekly status as the prep context
- `/mbr` — Monthly rollup that draws on weekly status data
- `/peer-digest` — Cross-team version (Friday)
