---
name: 1on1-prep
description: Generates 1:1 meeting prep for any Solutions Marketing team member. Follows the running-deck / OneNote pattern used by Microsoft Solutions Marketing managers. Pulls from Planner, SharePoint Lists, Loop trackers, or prior standup/status history. Supports upward mode for meetings with your own manager.
allowed-tools: [Bash, Read, Grep, Glob]
---

# 1:1 Prep Generator

**Type:** Meeting Preparation
**Speed:** ~30 seconds
**Output:** Loop/OneNote-ready prep notes + Teams message

## What This Skill Does

Generates a 1:1 prep document for any direct report, or for an upward meeting with your manager. Follows the format used by Microsoft Solutions Marketing leads — a running deck/page anchored to outcomes, blockers, stakeholders, and coaching rather than a raw task list.

Sections:
1. **Wins** — Completions and outcomes since last 1:1
2. **Priorities** — This week / next week focus
3. **Blockers** — What they need from you (decisions, air cover, unblock)
4. **Stakeholders & Comms** — Who they're aligning with, relationship status
5. **Growth / Feedback** — Skills, scope, wellbeing signal

Plus: **FIRE prompts** (Feedback, Ideas, Requests, Expectations) for upward mode.

## Triggers

```
/1on1-prep [name]       # Prep for 1:1 with a direct report
/1on1-prep              # Prompts for which person
/1on1-prep --upward     # Prep for meeting with YOUR manager
/1on1-prep --notes      # Output as OneNote/Loop page format
```

## Data Sources

| Source | What It Contains | Use |
|--------|-----------------|-----|
| **Planner / Tasks** | Open tasks, completed tasks, overdue/blocked items | Wins + blockers |
| **SharePoint Lists** | Initiative status, obstacles field, owner updates | Priorities + blockers |
| **Loop table** | Priority / Progress / Obstacles fields | Priorities + blockers |
| **Prior standup posts** | Teams channel history for this person | Wins + context |
| **Prior weekly status** | Most recent `/weekly-status` output | Full context |
| **Calendar** | Shared meetings with this person in the past week | Stakeholder context |
| **Manual input** | User provides context directly | Fallback |

## Implementation

### Step 1: Identify Person and Date Range

```
PERSON = [direct report name or "upward"]
SINCE  = date of last 1:1 with this person (default: 7 days ago)
```

### Step 2: Gather Per-Section Data

**Wins (since last 1:1):**
- Planner: tasks completed in the date range, assigned to this person
- SharePoint List: items moved to "Complete" in the date range, owned by this person
- Standup history: "Yesterday ✅" items from their posts

**Priorities (this week / next week):**
- Planner: open tasks due this week or next week
- SharePoint List: items with Status = "In Progress"
- Their most recent standup "Today 🎯" items

**Blockers (what they need from you):**
- Planner: blocked tasks or overdue items with no recent activity
- SharePoint List: items with content in the "Obstacles" column
- Their most recent standup "Blockers ⛔" items
- Anything overdue by 5+ days with no completion signal

**Stakeholders & Comms:**
- Shared calendar events with cross-team or leadership attendance
- Any peer/leadership names that appear in their status posts

**Growth / Feedback:**
- No automated source — prompt manager to add a note
- Flag if this person has had no 1:1 in 14+ days (wellbeing signal)

### Step 3: Format Output

**Standard prep (Teams message or Loop/OneNote page):**

```markdown
## 1:1 Prep — [Name] — [Date]
*Since last 1:1: [prior date]*

### 🏆 Wins
- [Outcome 1 — specific and outcome-framed]
- [Outcome 2]
- [Or: No completed items detected — confirm with [Name] before meeting]

### 🎯 Priorities
**This week:**
- [Priority 1] — [Status: On Track / At Risk]
- [Priority 2]

**Next week:**
- [Upcoming milestone or focus area]

### 🔓 Blockers (what they need from you)
- [Blocker] — Decision needed: [what and by when]
- [Blocker] — Air cover needed: [context]
- [Or: No blockers detected]

### 🤝 Stakeholders & Comms
- Aligning with: [peer/team names from calendar/status]
- Cross-team dependencies: [any flagged items]

### 💬 Growth / Feedback
*Add your note here before the meeting*
- [Coaching observation, scope discussion, wellbeing check-in]
```

**Upward mode (`--upward`):**

Swap the sections to prep for a meeting with your own manager. Adds FIRE framework:

```markdown
## Upward 1:1 Prep — [Your Manager] — [Date]

### What's Landing (Wins + Progress)
- [Outcome 1]
- [Outcome 2]

### What's Coming (Priorities)
- [This week / next week focus]

### What I Need (Blockers + Asks)
- [Decision needed from manager]
- [Air cover or escalation needed]

### FIRE
**Feedback:** [Feedback you want to give or receive]
**Ideas:** [Ideas you want to surface]
**Requests:** [Requests you're making]
**Expectations:** [Alignment check on expectations]
```

## Gotchas

- **Wins should be outcome-framed, not activity-framed.** "Delivered partner brief to Erica" beats "Worked on partner brief."
- **Blockers are the most valuable section.** A direct report who isn't raising blockers in their data either has none or isn't updating their tracker. Prompt them explicitly.
- **Growth/feedback has no automated source by design.** This section requires manager judgment. The template makes it easy to add a note before the meeting, not generate it automatically.
- **Upward mode is different in tone.** FIRE framework questions are prompts for the manager, not a report. Keep it conversational.
- **Don't fabricate status.** If no data exists for a person, say so explicitly and prompt for manual input before the meeting.

## Related Skills

- `/standup` — Daily signals that feed into 1:1 context
- `/weekly-status` — Weekly rollup that feeds into prep
- `/escalation-tracker` — Blockers that need upward escalation
