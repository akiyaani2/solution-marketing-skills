---
name: issue-triage
description: Tracker hygiene audit for M365-native teams. Scans SharePoint Lists, Planner boards, and Loop tables for stale items, missing owners, overdue tasks, and orphaned work. The M365 equivalent of GitHub issue triage — keeps your initiative and task trackers clean and actionable.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/triage"
  - "/tracker-triage"
  - "/triage --stale"
  - "/triage --orphans"
  - "/triage --overdue"
  - "/triage --missing-owners"
  - "triage the tracker"
  - "what needs attention"
  - "tracker hygiene"
---

# Tracker Triage

The M365-native equivalent of GitHub issue triage. Systematically scans your initiative and task trackers for hygiene problems that silently degrade team visibility.

## Usage

```
/triage                    # Full triage — all checks
/triage --stale            # Items with no updates in 14+ days
/triage --orphans          # Tasks not linked to any initiative
/triage --overdue          # Items past target date without completion
/triage --missing-owners   # Items with no assigned owner
/triage --p0-only          # Priority 1 items only
```

## Data Sources

| Source | What Gets Scanned |
|--------|------------------|
| **SharePoint Lists / MS Lists** | Initiative rows — Status, Owner, Target Date, Last Updated, Obstacles |
| **Planner / Tasks** | Tasks — Assignee, Due Date, Completion, Bucket/Plan |
| **Loop tables** | Initiative fields — Progress, Owner, End Date, Obstacles |

> **Note:** If the team uses GitHub Issues alongside M365 sources, append GitHub checks using `gh issue list` queries. GitHub is supplemental, not required.

## What This Skill Checks

### 1. Stale Items (`/triage --stale`)

Items with no update signal in 14+ days:

| Threshold | Signal |
|-----------|--------|
| 14–21 days | 🟡 Yellow — prompt owner to update |
| 21+ days | 🔴 Red — flag for manager review |
| 30+ days | 🔴🔴 Critical — escalate or close |

**Detection:** `today - last_modified_date > 14`

### 2. Missing Owners (`/triage --missing-owners`)

Items where the Owner/Assignee field is empty:
- Planner tasks with no assignee
- SharePoint List rows with empty Owner field
- Loop table rows with empty Owner field

**Rule:** Every active item (Status ≠ "Complete") must have an owner.

### 3. Overdue Items (`/triage --overdue`)

Items where Target Date / Due Date is in the past and Status ≠ "Complete":

| Days Overdue | Signal |
|--------------|--------|
| 1–7 days | 🟡 Yellow |
| 8–14 days | 🔴 Red |
| 15+ days | 🔴🔴 Critical — archive or escalate |

### 4. Orphaned Tasks (`/triage --orphans`)

Planner tasks that are not linked to any initiative in the SharePoint List or Loop table. These represent work being done with no program-level visibility.

**Detection:** Task exists in Planner but its title/reference doesn't match any active initiative row.

### 5. Priority 1 Review (`/triage --p0-only`)

All items marked Priority 1 (or equivalent) with any of the above issues:
- Stale P1 = immediate flag
- Overdue P1 = escalation required
- P1 with no owner = critical gap

## Output Format

```markdown
# Tracker Triage — [Date]

## Summary

| Check | Issues Found | Critical |
|-------|-------------|---------|
| Stale items (14+ days) | X | X |
| Missing owners | X | — |
| Overdue items | X | X |
| Orphaned tasks | X | — |
| P1 issues | X | X |

**Recommended actions: [count] items need immediate attention**

---

## 🔴 Critical — Act Today

### Stale Items (21+ days)
| Item | Owner | Last Updated | Days Stale | Action |
|------|-------|-------------|------------|--------|
| [Title] | [Name] | [Date] | X | Update status or close |

### Overdue P1 Items
| Item | Owner | Due Date | Days Overdue | Action |
|------|-------|----------|-------------|--------|
| [Title] | [Name] | [Date] | X | Escalate or reschedule |

---

## 🟡 Needs Attention — This Week

### Missing Owners
| Item | Status | Source | Action |
|------|--------|--------|--------|
| [Title] | [Status] | Planner/List | Assign owner |

### Stale Items (14–21 days)
| Item | Owner | Last Updated | Action |
|------|-------|-------------|--------|
| [Title] | [Name] | [Date] | Prompt owner |

---

## Orphaned Tasks (not linked to any initiative)
| Task | Assignee | Due Date | Suggested Initiative |
|------|----------|----------|---------------------|
| [Task] | [Name] | [Date] | [Best match or None] |

---

*Triage complete. Source: [SharePoint List name / Planner plan name]*
```

## Gotchas

- **"Last Modified" detection requires field visibility.** SharePoint Lists and Planner expose last-modified dates via Graph API, but Loop tables may not. If unavailable, staleness check falls back to manual prompt.
- **Orphan detection is fuzzy.** Linking tasks to initiatives in M365 is often done by naming convention, not a formal parent-child relationship. Orphan detection works best when task names reference the initiative they belong to.
- **Don't close items without owner confirmation.** Flag overdue items for the owner to close — don't auto-close.
- **Triage is a weekly practice, not a one-time fix.** Run this at the start of each week before the manager's weekly review.

## Related Skills

- `/epic-health` — Deeper health scoring per initiative
- `/weekly-status` — Per-person view that surfaces the same signals
- `/escalation-tracker` — Escalate critical blockers surfaced by triage
