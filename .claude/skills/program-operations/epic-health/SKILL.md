---
name: epic-health
description: Scores strategic initiatives on 5 health dimensions using M365-native sources (SharePoint Lists, Loop tables, Planner). No GitHub required. Produces RAG-graded health scores per initiative with aggregate summary. Matches the 3-layer initiative model used by Microsoft Solutions Marketing teams.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/epic-health"
  - "/epic-health --red-only"
  - "/epic-health [initiative-name]"
  - "check initiative health"
  - "are initiatives on track"
  - "initiative status"
  - "how are we doing on [program]"
---

# Initiative Health Scoring

Score every strategic initiative on 5 health dimensions. Produces a weighted composite score (0–100) with Green/Yellow/Red grading per initiative and an aggregate summary.

Follows the 3-layer model observed in Microsoft Solutions Marketing:
- **Viva Goals** — OKR/outcome layer (strategy)
- **SharePoint Lists / Loop tables** — Initiative layer ("epics")
- **Planner / Tasks** — Work item layer (execution)

This skill operates on the **initiative layer** (Layer B).

## Usage

```
/epic-health                      # All initiatives
/epic-health --red-only           # Only show unhealthy initiatives
/epic-health [initiative-name]    # Specific initiative by name
/epic-health [owner]              # Initiatives owned by a specific person
```

## Data Sources

| Source | Fields Used | Priority |
|--------|------------|----------|
| **SharePoint List / MS Lists** | Status, Owner, % Complete, Target Date, Obstacles, Last Updated | Primary |
| **Loop table (Main Tracker style)** | Priority, Motion, Owner, Progress, End Date, Obstacles, Notes | Primary |
| **Planner** | Open tasks, blocked tasks, overdue tasks linked to initiative | Secondary (for task counts) |
| **Manual input** | User provides initiative status directly | Fallback |

> **Expected schema (Loop/List):** Priority / Motion / Category / Title / Owner / Progress / End Date / Obstacles / Notes

## Health Dimensions

| # | Dimension | Weight | What It Measures |
|---|-----------|--------|------------------|
| 1 | **Completion Progress** | 30% | % complete vs. time elapsed in the initiative window |
| 2 | **Blocker Count** | 25% | Number of open blockers or items in the "Obstacles" field |
| 3 | **Staleness** | 20% | Days since last status update (item or task activity) |
| 4 | **Decision Debt** | 15% | Open decisions or unresolved dependencies (if tracked) |
| 5 | **Date Confidence** | 10% | End date vs. current progress trajectory |

## Scoring Logic

### Completion Progress (30 pts max)
```
time_elapsed_pct = (today - start_date) / (end_date - start_date)
progress_pct = item.progress_field (0–100)

if progress_pct >= time_elapsed_pct × 0.9:   30 pts (on track)
elif progress_pct >= time_elapsed_pct × 0.7:  20 pts (slight lag)
else:                                           0–10 pts (behind)
```

### Blocker Count (25 pts max)
```
blockers = count of non-empty Obstacles entries + Planner blocked tasks

if blockers == 0:    25 pts
elif blockers == 1:  15 pts
elif blockers == 2:  5 pts
else:                0 pts
```

### Staleness (20 pts max)
```
days_stale = today - last_modified_date

if days_stale <= 3:   20 pts
elif days_stale <= 7:  12 pts
elif days_stale <= 14:  5 pts
else:                   0 pts
```

### Decision Debt (15 pts max)
```
open_decisions = count of items where Notes contains "TBD" or "Decision needed"

if open_decisions == 0:   15 pts
elif open_decisions == 1:  8 pts
else:                       0 pts
```

### Date Confidence (10 pts max)
```
if end_date is in the future AND progress >= 50% AND no critical blockers:   10 pts
elif end_date is approaching (< 14 days) AND blockers > 0:                     3 pts
elif end_date has passed AND status != "Complete":                              0 pts
else:                                                                           7 pts
```

## Output Format

```markdown
# Initiative Health — [Date]

## Summary

| Initiatives | 🟢 Green | 🟡 Yellow | 🔴 Red |
|-------------|---------|---------|-------|
| [Total] | [Count] | [Count] | [Count] |

## Initiative Scores

| Initiative | Owner | Score | Status | Top Issue |
|------------|-------|-------|--------|-----------|
| [Name] | [Owner] | 87/100 | 🟢 | None |
| [Name] | [Owner] | 61/100 | 🟡 | Stale (9 days) |
| [Name] | [Owner] | 32/100 | 🔴 | 2 blockers + behind pace |

## Red Initiatives — Action Required

### [Initiative Name] — Score: 32/100 🔴
**Owner:** [Name]
**Progress:** 25% complete (should be ~55% by now)
**Blockers:** [Blocker 1], [Blocker 2]
**Last Updated:** [Date] (X days ago)
**Recommended Action:** [Specific next step]
```

## Gotchas

- **Staleness detection depends on list/table having a Last Modified or Last Updated field.** If your SharePoint List doesn't expose last modified date, staleness scoring will not be available.
- **"Obstacles" field discipline matters.** If team members don't update the Obstacles field when blocked, blocker count will underreport.
- **Progress % is often manually entered.** It reflects what the owner believes, not a computed value. This is a limitation of M365-native tracking vs. automated systems.
- **Green doesn't mean risk-free.** An initiative with 90/100 health today can slip to 40/100 next week if a key milestone is missed. Run this check weekly.
- **No GitHub required.** If the team uses GitHub Issues alongside M365, you can supplement with `gh issue list` queries, but it's not required.

## Related Skills

- `/solution-play-health` — Aggregates initiative scores into play-level view
- `/weekly-status` — Per-person view that feeds initiative health
- `/mbr` — Monthly rollup that draws on initiative health data
