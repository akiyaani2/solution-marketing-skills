---
name: mbr
description: Generate a Monthly Business Review package from GitHub Issues. Produces leadership-ready output with executive summary, completions by owner, in-progress by priority, blockers, metrics, and narrative.
---

# MBR Generator

**Type:** Leadership Reporting
**Speed:** ~45 seconds
**Output:** Markdown MBR ready for deck, email, or document

## What This Skill Does

Generates a Monthly Business Review package by pulling data from GitHub Issues:
1. **Executive Summary** -- 2-3 sentence narrative of the month
2. **Completed Items** -- Closed issues grouped by owner with highlights
3. **In-Progress by Priority** -- Open P0/P1 items with status
4. **Blocked / At-Risk** -- Items needing escalation with context
5. **Key Metrics** -- Month-over-month and year-to-date tables
6. **Narrative Template** -- Leadership-ready framing of the month
7. **Next Month Preview** -- Upcoming milestones and focus areas
8. **Asks / Escalations** -- What you need from leadership

## Triggers

```
/mbr                    # Current month MBR
/mbr [month]            # Specific month (e.g., /mbr january, /mbr march)
/mbr --format deck      # Bullet-point format for slides
/mbr --format email     # Narrative format for email
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use `owner:[name]` labels for grouping by person
- Priority labels (`p0`, `p1`, `p2`) for sorting
- Status labels (`status:blocked`, `status:at-risk`) for risk identification
- Consistent use of issue close dates for completion tracking
- Repository name set as `REPO` variable

## Implementation

### Step 1: Determine Date Range

```bash
REPO="[org]/[repo]"

# Current month (default)
MONTH_START=$(date +%Y-%m-01)

# Or specific month: /mbr march -> derive MONTH_START
# For a named month, resolve to YYYY-MM-01 of that month in the current fiscal year
```

### Step 2: Query Closed Issues This Month

```bash
gh issue list --repo "$REPO" \
  --state closed \
  --search "closed:>=${MONTH_START}" \
  --json number,title,labels,closedAt,assignees \
  --limit 100
```

### Step 3: Query Open Issues by Priority

```bash
# P0 - Critical
gh issue list --repo "$REPO" \
  --state open \
  --label "p0" \
  --json number,title,labels,assignees \
  --limit 50

# P1 - Important
gh issue list --repo "$REPO" \
  --state open \
  --label "p1" \
  --json number,title,labels,assignees \
  --limit 50
```

### Step 4: Query Blocked and At-Risk Items

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "status:blocked" \
  --json number,title,labels,assignees,updatedAt \
  --limit 20

gh issue list --repo "$REPO" \
  --state open \
  --label "status:at-risk" \
  --json number,title,labels,assignees,updatedAt \
  --limit 20
```

### Step 5: Group by Owner and Assemble

Parse `owner:[name]` labels to group completions by person. Count items per owner. Identify top highlights (P0/P1 closures, milestones hit).

### Step 6: Generate Narrative

Synthesize data into the executive summary. Focus on:
- What the team delivered (outcomes, not activities)
- What moved forward vs. what stalled
- Key decisions made or needed
- Connection to strategic priorities

## Output Format

```markdown
# Monthly Business Review -- [Month] [Year]

## Executive Summary

[2-3 sentences: what the team delivered this month, key wins, and any significant risks. Frame in terms of outcomes and strategic impact, not task counts.]

---

## Completed This Month

### By Owner
| Owner | Completed | Key Highlights |
|-------|-----------|----------------|
| [Person A] | X items | [Top 1-2 completions] |
| [Person B] | X items | [Top 1-2 completions] |
| [Person C] | X items | [Top 1-2 completions] |
| [Team Lead] | X items | [Strategy, coordination] |
| **Total** | **XX** | |

### Top Highlights
- [Major accomplishment 1 -- why it matters]
- [Major accomplishment 2 -- why it matters]
- [Major accomplishment 3 -- why it matters]

---

## In Progress

### P0 Items (Critical Path)
| Issue | Owner | Status | Target Date |
|-------|-------|--------|-------------|
| #XX -- [Title] | [Name] | [Status] | [Date] |

### P1 Items (Important)
| Issue | Owner | Status | Target Date |
|-------|-------|--------|-------------|
| #XX -- [Title] | [Name] | [Status] | [Date] |

---

## Blocked / At-Risk

| Issue | Owner | Blocker | Days Stuck | Escalation Needed? |
|-------|-------|---------|------------|-------------------|
| #XX -- [Title] | [Name] | [Description] | X | Yes/No |

**Escalation Actions:**
- [Item]: [What's needed and from whom]

---

## Key Metrics

### Month-over-Month
| Metric | This Month | Last Month | Delta | Trend |
|--------|------------|------------|-------|-------|
| Issues closed | XX | XX | +X% | Up/Down |
| [Program metric 1] | XX | XX | +X% | Up/Down |
| [Program metric 2] | XX | XX | +X% | Up/Down |
| [Program metric 3] | XX | XX | +X% | Up/Down |

### Year-to-Date
| Metric | YTD Actual | YTD Target | % to Goal |
|--------|------------|------------|-----------|
| [Key metric 1] | XX | XX | XX% |
| [Key metric 2] | XX | XX | XX% |
| [Key metric 3] | XX | XX | XX% |

> **Note:** Populate metric values from your team's dashboards, tracking sheets, or external tools. GitHub Issues tracks task completion; program metrics (registrations, engagement, pipeline) typically come from separate data sources.

---

## Next Month Preview

### Key Dates
| Date | Event / Milestone | Owner |
|------|-------------------|-------|
| [Date] | [Event] | [Name] |

### Focus Areas
1. [Focus area 1 -- what and why]
2. [Focus area 2 -- what and why]
3. [Focus area 3 -- what and why]

---

## Asks / Escalations for Leadership

| Ask | For Whom | Urgency | Context |
|-----|----------|---------|---------|
| [What you need] | [Manager/VP] | High/Med/Low | [Why it matters now] |

---
*Generated [timestamp] from GitHub Issues*
```

## Leadership Framing Tips

Build these principles into how you write the Executive Summary and Highlights:

1. **Lead with insight, not activity.** "We accelerated partner onboarding by 40%" beats "We closed 12 partner issues."
2. **Frame constraints positively.** "Delivered with reduced vendor capacity" instead of "We were short-staffed."
3. **Include competitive context where relevant.** "This positions us ahead of [competitor]'s timeline" adds strategic value.
4. **Connect to strategic narrative.** Every highlight should tie back to a team or organizational priority.
5. **Separate what you built from what you need.** Keeps the "asks" section clean and actionable.

## Format Variants

### Deck Format (`--format deck`)

Strip to bullet points. No tables. Each section is 3-5 bullets max. Designed for copy-paste into PowerPoint or Google Slides.

### Email Format (`--format email`)

Narrative paragraphs instead of tables. Conversational but professional tone. Designed for a leadership email thread.

## Gotchas

- **GitHub Issues closed count is not the same as "work done."** Some issues are admin cleanup, duplicates, or won't-fix. Filter these out or note them separately.
- **Metric values beyond "issues closed" require external data sources.** The MBR template includes placeholders for program metrics (registrations, engagement, etc.) -- you must pull these from dashboards, spreadsheets, or other tools.
- **Month boundaries can be tricky.** If someone closes an issue at 11:59 PM on the last day, timezone differences may push it to the next month in GitHub's API.
- **The `--search "closed:>="` qualifier uses GitHub Search indexing**, which can lag by minutes. For end-of-month reports, wait at least an hour after month-end.
- **P0/P1 labels need team discipline to be useful here.** If everything is P0, nothing is. Review priority hygiene before relying on this grouping.
- **Don't generate the MBR too early in the month.** Wait until at least the 3rd business day of the following month for a complete picture.
- **Historical MBR data compounds in value.** Save generated MBRs to a consistent location (e.g., `operations/mbr/YYYY-MM-mbr.md`) for trend analysis.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Closed issue limit | 100 | Increase for larger teams |
| Highlight count | 3 | Adjust for leadership preference |
| Metric rows | 4 | Map to your team's KPIs |
| Output location | Terminal | File, clipboard, or issue comment |
| Leadership tone | Insight-driven | Match your manager's preferences |

## Related Skills

- `/weekly-status` -- Feeds into MBR as weekly data
- `/escalation-tracker` -- Blocked items feed into MBR escalations
- `/epic-health` -- Epic-level health complements MBR
- `/1on1-prep` -- Individual data that rolls up to MBR
