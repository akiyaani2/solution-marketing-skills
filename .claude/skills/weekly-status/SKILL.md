---
name: weekly-status
description: Generate a weekly status summary for any team member from their GitHub project board. Shows completed, in-progress, blocked, and upcoming items.
---

# Weekly Status Generator

**Type:** Weekly Operations
**Speed:** ~20 seconds

## What This Skill Does

Generates a weekly status summary for any team member based on their GitHub issues:
1. **Completed This Week** -- Issues closed in the last 7 days
2. **In Progress** -- Open issues actively being worked
3. **Blocked / At-Risk** -- Items that need attention or escalation
4. **Coming Up Next Week** -- Priority items and upcoming deadlines
5. **Discussion Points** -- Auto-suggested talking points for 1:1s

## Triggers

```
/weekly-status              # Prompts for which person (or shows all)
/weekly-status [name]       # Specific person's status
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use `owner:[name]` labels to assign ownership
- Priority labels (e.g., `p0`, `p1`, `p2`) for sorting
- Status labels (e.g., `status:blocked`, `status:at-risk`, `status:in-progress`)
- Repository name set as `REPO` variable

## Implementation

### Step 1: Set Variables

```bash
REPO="[org]/[repo]"
OWNER="[name]"   # From the command argument
WEEK_AGO=$(date -v-7d +%Y-%m-%d 2>/dev/null || date -d "7 days ago" +%Y-%m-%d)
```

### Step 2: Query Completed Issues (Last 7 Days)

```bash
gh issue list --repo "$REPO" \
  --state closed \
  --label "owner:${OWNER}" \
  --search "closed:>=${WEEK_AGO}" \
  --json number,title,closedAt \
  --limit 20
```

### Step 3: Query In-Progress Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "owner:${OWNER}" \
  --json number,title,labels,updatedAt \
  --limit 30
```

Filter for items with `status:in-progress`, `p0`, or `p1` labels to surface active work. Everything else goes into the backlog count.

### Step 4: Query Blocked / At-Risk Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "owner:${OWNER}" \
  --label "status:blocked" \
  --json number,title,labels,updatedAt \
  --limit 10

gh issue list --repo "$REPO" \
  --state open \
  --label "owner:${OWNER}" \
  --label "status:at-risk" \
  --json number,title,labels,updatedAt \
  --limit 10
```

### Step 5: Identify Upcoming Deadlines

Scan open issues for due dates in the next 7-14 days. Check issue bodies, milestones, or project custom date fields.

### Step 6: Generate Discussion Points

Auto-suggest based on patterns:
- Any blocked items that need escalation
- Items waiting on the team lead's decision (e.g., `status:waiting-approval`)
- Cross-team coordination needs (e.g., `sp:cross-solution` or similar labels)
- Stale items (no update in 14+ days)

## Output Format

```markdown
## Weekly Status -- [Name] | Week of [Date]

---

### Completed This Week
- **#123** Issue title
- **#124** Another completed item
- (or "No items closed this week")

### In Progress
- **#125** Active work item [P0]
- **#126** Another active item [P1]
- **#127** Third active item

### Blocked / At-Risk
- **#128** Blocked item -- reason/waiting on X
- (or "All clear -- no blockers")

### Coming Up Next Week
- [Priority items for next week based on P0/P1 labels]
- [Any upcoming deadlines or events]

### Discussion Points for 1:1
- [Auto-suggest: blocked items needing escalation]
- [Items waiting on lead's decision]
- [Cross-team coordination needed]
- [Stale items to review]

---

*Generated [timestamp] from GitHub Issues*
```

## Team Lead Roll-Up

When invoked without a name (or with the team lead's name), aggregate all team members:

```markdown
## Weekly Status -- Team Roll-Up | Week of [Date]

### Summary
| Owner | Closed | In Progress | Blocked |
|-------|--------|-------------|---------|
| [Person A] | 3 | 5 | 0 |
| [Person B] | 1 | 4 | 1 |
| [Person C] | 2 | 6 | 0 |
| **Total** | **6** | **15** | **1** |

### Highlights
- [Top 3 completions across the team]

### Needs Attention
- [Blocked items, stale items, at-risk items]
```

## Gotchas

- **Lead with wins, then problems.** Always show "Completed" before "Blocked" -- this is for morale and reflects well in status meetings.
- **Keep each section to 5 items max.** If there are more, summarize (e.g., "and 4 more items"). Long lists lose the reader.
- **P0s should be visually prominent.** Bold them or tag them so they stand out in the In Progress section.
- **Don't editorialize.** This report should be factual data pulled from GitHub. Add context only in Discussion Points.
- **Stale items (14+ days no update) are a signal.** Surface them in Discussion Points -- they often indicate work is stuck but not labeled as blocked.
- **Weekend timing matters.** If run on Monday, the "last 7 days" window should cover the full prior work week (Monday to Friday), not Saturday to Monday.
- **Some team members have project boards; some don't.** The `owner:` label query works regardless of whether they have a dedicated project board.
- **Always include all 5 sections** even if empty ("All clear" or "No items"). Consistent structure makes it scannable.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Lookback window | 7 days | Adjust for sprints or pay periods |
| Owner label prefix | `owner:` | Change to match your label scheme |
| Priority labels | `p0`, `p1`, `p2` | Map to your priority scheme |
| Stale threshold | 14 days | Adjust based on team cadence |
| Max items per section | 5 | Increase for detailed reviews |

## Related Skills

- `/standup` -- Daily version (24h window)
- `/1on1-prep` -- Deep prep including FIRE framework
- `/mbr` -- Monthly roll-up for leadership
- `/peer-digest` -- Cross-team Friday update
