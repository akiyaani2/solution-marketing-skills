---
name: event-countdown
description: Days until key events with readiness checklist status. Three urgency tiers — imminent, coming soon, on horizon — with per-event readiness percentage and color coding.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/event-countdown"
  - "/countdown"
  - "/countdown [event]"
  - "what events are coming"
  - "days until [event]"
  - "event readiness"
---

# Event Countdown

Shows countdown to key events with readiness scoring. Groups events into three urgency tiers and calculates per-event readiness from sub-issue completion in the event's parent epic.

## Usage

```
/event-countdown           # All upcoming events across all tiers
/countdown [event]         # Single event deep dive (e.g., /countdown build)
/countdown --week          # Events in next 7 days only
/countdown --month         # Events in next 30 days only
/countdown --quarter       # Events in next 90 days
```

## Three Urgency Tiers

| Tier | Window | Action Required |
|------|--------|----------------|
| 🚨 **Imminent** | < 7 days | Final checks only. Anything not done is a risk. |
| ⏰ **Coming Soon** | 7-30 days | Active execution. Blockers must be resolved now. |
| 📅 **On Horizon** | 30-90 days | Planning and preparation. Dependencies should be mapped. |

Events beyond 90 days are listed in a "Future" section without readiness scoring.

## Readiness Scoring

Readiness is calculated from the event's parent epic (`type:initiative` with `event:` or `func:events` label):

```
Readiness % = (completed_tasks / total_tasks) * 100

Where:
- completed_tasks = count of "- [x]" items in epic body + closed linked issues
- total_tasks     = count of all "- [ ]" and "- [x]" items + all linked issues
```

### Readiness Color Coding

The color depends on BOTH readiness percentage AND time remaining:

| Scenario | Color | Meaning |
|----------|-------|---------|
| >= 90% readiness AND < 7 days out | 🟢 Green | Ready for launch |
| >= 75% readiness AND > 7 days out | 🟢 Green | On track |
| 50-89% readiness AND < 7 days out | 🟡 Yellow | At risk — gaps this close to event |
| 50-74% readiness AND 7-30 days out | 🟡 Yellow | Needs acceleration |
| < 50% readiness AND < 7 days out | 🔴 Red | Critical — event in danger |
| < 50% readiness AND 7-30 days out | 🔴 Red | Behind schedule |
| < 50% readiness AND > 30 days out | 🟡 Yellow | Early — time to recover |

**Key insight:** 60% readiness means different things at 3 days out (red) versus 60 days out (yellow). The scoring adjusts for time proximity.

## Implementation Steps

### 1. Find all event-related epics

```bash
# Get issues with event-related labels
gh issue list --repo OWNER/REPO \
  --state open \
  --label "type:initiative" \
  --json number,title,body,labels,updatedAt,assignees \
  --limit 100
```

Filter for issues that have:
- An `event:` label (e.g., `event:build-2026`, `event:ignite-2026`)
- OR a `func:events` or `func:hackathon` label
- OR "event" / "hack" / "conference" in the title

### 2. Extract event dates

Event dates can come from multiple sources (check in order):
1. **Custom field "Event Date"** on the project board (if available via GraphQL)
2. **Date in the issue body** — look for patterns like `Date: YYYY-MM-DD` or `Event: Month DD, YYYY`
3. **Label convention** — e.g., `event:build-2026` can map to a known date table
4. **Milestone due date** — if the event is a milestone

```bash
# Check issue body for date references
gh issue view [NUMBER] --repo OWNER/REPO --json body \
  | jq -r '.body' \
  | grep -iE '(date|when|event date|target date).*\d{4}'
```

### 3. Calculate days remaining

```bash
# macOS
EVENT_DATE="2026-06-01"
TODAY=$(date +%Y-%m-%d)
DAYS=$(( ( $(date -j -f "%Y-%m-%d" "$EVENT_DATE" +%s) - $(date -j -f "%Y-%m-%d" "$TODAY" +%s) ) / 86400 ))

# Linux
DAYS=$(( ( $(date -d "$EVENT_DATE" +%s) - $(date -d "$TODAY" +%s) ) / 86400 ))
```

### 4. Calculate readiness per event

```bash
# Get epic body for task list parsing
gh issue view [EPIC_NUMBER] --repo OWNER/REPO --json body

# Count completed vs total tasks in the body
# "- [x]" or "- [X]" = completed
# "- [ ]" = incomplete
```

Also check for sub-issues with `status:blocked` or `status:at-risk`:

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "event:[event-name],status:at-risk" \
  --json number,title
```

### 5. Assemble output by tier

Sort events by date, group into tiers, apply readiness color coding.

## Output Format

### All Events View

```markdown
# Event Countdown — [Date]

## 🚨 Imminent (< 7 days)

| Event | Date | Days | Ready | Owner | Risk |
|-------|------|------|-------|-------|------|
| [Event Name] | Mar 16 | 3 | 🟡 85% | [name] | Swag delivery delayed |

---

## ⏰ Coming Soon (7-30 days)

| Event | Date | Days | Ready | Owner | Risk |
|-------|------|------|-------|-------|------|
| [Event Name] | Apr 5 | 22 | 🟢 78% | [name] | — |
| [Event Name] | Apr 12 | 29 | 🟡 55% | [name] | Content not locked |

---

## 📅 On Horizon (30-90 days)

| Event | Date | Days | Ready | Owner | Risk |
|-------|------|------|-------|-------|------|
| [Event Name] | Jun 1 | 76 | 🟢 70% | [name] | — |
| [Event Name] | Jun 15 | 90 | 🔴 30% | [name] | No partner confirmed |

---

## 📆 Future (90+ days)

| Event | Date | Days | Owner |
|-------|------|------|-------|
| [Event Name] | Nov 2026 | 240 | [name] |

---

## Key Dates Timeline

| Date | Event | Days |
|------|-------|------|
| Mar 16 | [Event] | 3 |
| Apr 5 | [Event] | 22 |
| Jun 1 | [Event] | 76 |
| Jun 30 | Fiscal Year End | 105 |
```

### Single Event Deep Dive (`/countdown [event]`)

```markdown
# [Event Name] — [Date] ([N] days)

**Owner:** [name] | **Epic:** #NN | **Readiness:** 78% 🟢

## Task Checklist

| Task | Status | Due | Notes |
|------|--------|-----|-------|
| Venue/platform confirmed | ✅ Done | Feb 15 | |
| Speaker lineup locked | ✅ Done | Mar 1 | |
| Content/sessions finalized | 🔄 In Progress | Apr 15 | 3 of 8 sessions drafted |
| Registration live | ⏳ Not Started | May 1 | Depends on content lock |
| Marketing materials | ⏳ Not Started | May 15 | |
| Logistics/swag | ⚠️ At Risk | May 20 | Vendor PO not signed |

## Blockers

| # | Title | Owner | Days Blocked |
|---|-------|-------|-------------|
| #NN | [Title] | [name] | 5 |

## Milestones

| Milestone | Date | Status |
|-----------|------|--------|
| Content lock | Apr 15 | 🔄 In progress |
| Registration opens | May 1 | ⏳ Waiting |
| Event day | Jun 1 | 📅 76 days |

## Risk Assessment

- **Overall:** 🟢 On track
- **Key risk:** Vendor PO for swag not signed — escalate if not resolved by Apr 1
```

## Gotchas

- **Event dates are not always in GitHub.** Many teams track event dates in external calendars, SharePoint, or team wikis. If your repo uses custom project fields for "Event Date," you will need the GitHub Projects GraphQL API to read them — `gh issue view` alone will not surface project-level custom fields.
- **Readiness at 0% early is normal.** An event 90 days out at 10% readiness is not alarming — most prep happens in the final 30 days. The color coding accounts for this, but do not over-alarm on low readiness for distant events.
- **"Target Date" vs "Event Date" are different.** Target Date is when prep work should complete (e.g., content lock May 15). Event Date is the event itself (e.g., conference June 1). Some epics track both. Use Event Date for countdown, Target Date for readiness milestones.
- **Past events still in the list.** If an event epic is not closed after the event, it will show negative days. Filter these out or flag as "event completed — close epic."
- **Date parsing is fragile.** Issue bodies use inconsistent date formats. Look for `YYYY-MM-DD`, `Month DD, YYYY`, `MM/DD/YYYY`, and relative references like "Q3 FY26."
- **Date arithmetic differs between macOS and Linux.** macOS uses `date -j -f`, Linux uses `date -d`. Include both variants or detect the OS first.
- **Hackathons and multi-day events have start and end dates.** Use the start date for countdown. A 5-day event starting in 3 days is "3 days out," not "8 days out."

## Related Skills

- `/epic-health` — Detailed health scoring for event epics
- `/event-ops` — Full event lifecycle management (workback, vendors, logistics)
- `/standup` — Daily view surfaces urgent event tasks
- `/triage` — Identifies blocked event tasks that threaten readiness
