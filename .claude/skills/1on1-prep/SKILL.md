---
name: 1on1-prep
description: Generate 1:1 meeting prep for any team member. Pulls open issues, recent completions, blockers, and FIRE framework prompts. Supports upward mode for meetings with your own manager.
---

# 1:1 Prep Generator

**Type:** Meeting Preparation
**Speed:** ~30 seconds

## What This Skill Does

Generates a 1:1 prep document for any team member or for an upward meeting with your manager:
1. **Their open issues** grouped by priority
2. **Blocked items** needing escalation or discussion
3. **Recent completions** to acknowledge
4. **Upcoming deadlines** to align on
5. **FIRE prompts** (Feedback, Ideas, Requests, Expectations)
6. **Context notes** from their profile and activity patterns

## Triggers

```
/1on1-prep [name]       # Prep for 1:1 with a direct report
/1on1-prep              # Prompts for which person
/1on1-prep --upward     # Prep for meeting with YOUR manager
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use `owner:[name]` labels for ownership
- Priority labels (`p0`, `p1`, `p2`) for sorting
- Status labels (`status:blocked`, `status:waiting-approval`)
- Optional: Team member profiles at a known path (e.g., `team/[name].md`)
- Repository name set as `REPO` variable

## Implementation

### Step 1: Identify the Person

Map the argument to an owner label. If no argument, prompt for a name.

```bash
REPO="[org]/[repo]"
OWNER="[name]"  # From command argument
TWO_WEEKS_AGO=$(date -v-14d +%Y-%m-%d 2>/dev/null || date -d "14 days ago" +%Y-%m-%d)
```

### Step 2: Query Their Open Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "owner:${OWNER}" \
  --json number,title,labels,assignees,createdAt,updatedAt \
  --limit 50
```

### Step 3: Query Their Recent Completions (14 Days)

```bash
gh issue list --repo "$REPO" \
  --state closed \
  --label "owner:${OWNER}" \
  --search "closed:>=${TWO_WEEKS_AGO}" \
  --json number,title,closedAt \
  --limit 20
```

### Step 4: Query Their Blocked Items

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "owner:${OWNER}" \
  --label "status:blocked" \
  --json number,title,labels,updatedAt \
  --limit 10
```

### Step 5: Read Their Profile (Optional)

If team member profiles exist:

```bash
# Read profile for communication preferences, focus areas, coordination partners
cat team/[name].md 2>/dev/null
```

### Step 6: Generate FIRE Prompts

Based on the data patterns, generate contextual prompts for each FIRE category.

## Output Format (Direct Report)

```markdown
# 1:1 Prep: [Team Lead] <> [Name]
**Date:** [Today's date]
**Last 1:1:** [Date if known, or "Unknown"]

---

## Their Dashboard

### Open Issues ([X] total)

#### P0 -- Critical
| # | Issue | Status | Due |
|---|-------|--------|-----|
| XX | [Title] | In Progress | [Date] |

#### P1 -- Important
| # | Issue | Status | Due |
|---|-------|--------|-----|
| XX | [Title] | In Progress | [Date] |

#### P2 -- Backlog
[X items in backlog -- review if time permits]

### Blocked Items
| # | Issue | Blocker | Can You Help? |
|---|-------|---------|---------------|
| XX | [Title] | [What's blocking] | Yes -- [suggested action] |

### Completed Since Last 1:1
- #XX -- [Title] (closed [date])
- #XX -- [Title] (closed [date])

### Upcoming Deadlines (Next 14 Days)
| Date | Issue | Notes |
|------|-------|-------|
| [Date] | #XX -- [Title] | [Context] |

---

## FIRE Framework Prompts

### Feedback (for them)
- [Specific positive feedback based on recent completions]
- [Growth area to discuss based on observed patterns]

### Ideas (from them)
- "What's one thing we should start doing differently?"
- "What's blocking you that I don't know about?"
- "Any process that's slowing you down?"

### Requests (from them)
- "What do you need from me this week?"
- "Any decisions you're waiting on from me?"
- "Where do you need more support or air cover?"

### Expectations (alignment)
- "Are priorities clear for the next two weeks?"
- "Anything shifting that I should know about?"
- "How's your workload -- sustainable or stretched?"

---

## Context Notes

### Activity Patterns
- [X] issues closed in last 14 days (above/below their average)
- [X] items currently blocked
- [X] days since last update on oldest active issue
- [Longest open issue: #XX, opened X days ago]

### Your Notes
[Space for the team lead to add talking points before the meeting]

---
*Generated [timestamp] from GitHub Issues*
```

## Output Format (Upward -- Meeting with Your Manager)

When `--upward` is used, the format shifts from "reviewing a report" to "preparing for your own review":

```markdown
# 1:1 Prep: [Team Lead] <> [Manager]
**Date:** [Today's date]

---

## Items Needing Manager Input

### Decisions Needed
| # | Issue | Decision | Urgency |
|---|-------|----------|---------|
| XX | [Title] | [What you need decided] | High/Med/Low |

### Escalations
| # | Issue | Why Escalating | Ask |
|---|-------|----------------|-----|
| XX | [Title] | [Trigger] | [What you need] |

### FYIs (No Action Needed)
- [Update 1 -- brief context]
- [Update 2 -- brief context]

---

## Team Health Snapshot

| Metric | Value | Trend |
|--------|-------|-------|
| Open issues | XX | Stable/Rising/Falling |
| Blocked items | XX | Stable/Rising/Falling |
| Closed this week | XX | Above/Below average |
| Team capacity | Green/Yellow/Red | [Context] |

---

## Strategic Topics to Raise
- [Topic 1 -- what you want to align on]
- [Topic 2 -- what you want to align on]

## Career / Development
- [Optional -- include if there are career topics to discuss]

---
*Generated [timestamp] from GitHub Issues*
```

## FIRE Framework Deep Dive

The FIRE framework ensures 1:1s cover all dimensions, not just status updates:

| Letter | Category | Direction | Purpose |
|--------|----------|-----------|---------|
| **F** | Feedback | Lead to report | Acknowledge wins, coach on growth areas |
| **I** | Ideas | Report to lead | Surface innovation, process improvements |
| **R** | Requests | Report to lead | Unblock them, get resources, make decisions |
| **E** | Expectations | Bidirectional | Align on priorities, workload, timelines |

**Tip:** Rotate emphasis each week. Week 1: heavy on Feedback. Week 2: heavy on Ideas. This prevents 1:1s from becoming pure status meetings.

## Gotchas

- **Don't make the 1:1 a GitHub issue review session.** The prep is background context. The conversation should be about the person, not the tickets.
- **Positive feedback first.** Always lead the Feedback section with something specific and genuine. If you can't find a completion to highlight, note a pattern of consistency or improvement.
- **"Can You Help?" column is the action trigger.** If multiple blocked items say "Yes," that is the 1:1 agenda right there.
- **Stale issues are a conversation, not an accusation.** If an issue has had no update in 14+ days, ask "Is this still the right priority?" not "Why hasn't this moved?"
- **Upward mode requires different framing.** You are presenting to your manager, not reviewing a report. Lead with decisions needed and strategic alignment, not task lists.
- **The 14-day lookback for completions aligns with biweekly 1:1 cadence.** If your 1:1s are weekly, reduce to 7 days.
- **Profiles are optional but valuable.** If you maintain `team/[name].md` files with communication preferences and focus areas, the prep becomes much richer.
- **Activity patterns can be misleading.** Low issue-close counts might mean deep work on one large item, not lack of productivity. Don't jump to conclusions.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Lookback for completions | 14 days | Match your 1:1 cadence |
| Owner label prefix | `owner:` | Match your label scheme |
| Profile path | `team/[name].md` | Wherever you keep team docs |
| FIRE emphasis | Rotate weekly | Fix based on team need |
| Upward format | Decisions + escalations | Match your manager's preferences |

## Related Skills

- `/weekly-status` -- Weekly data that feeds into 1:1 context
- `/standup` -- Daily view for quick check-ins
- `/escalation-tracker` -- Escalation items for upward 1:1s
- `/mbr` -- Monthly package (shares some data sources)
