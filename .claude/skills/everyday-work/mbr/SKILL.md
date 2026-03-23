---
name: mbr
description: Generates a Monthly Business Review package for Solutions and Partner Marketing teams. Pulls data from M365 sources (SharePoint, Excel, Power BI, Planner, Teams/email) or manual input. Produces leadership-ready output with executive summary, completions by owner, in-progress by priority, blockers, metrics, and narrative.
allowed-tools: [Bash, Read, Grep, Glob]
---

# MBR Generator

**Type:** Leadership Reporting
**Speed:** ~45–90 seconds (varies by data source availability)
**Output:** Markdown MBR ready for deck, email, or document

## What This Skill Does

Generates a Monthly Business Review package by pulling data from wherever the team actually stores it — SharePoint lists, Excel trackers, Power BI dashboards, Planner/Tasks, Teams channels, Outlook email, or shared documents. GitHub Issues is one possible source but is not required.

Produces:
1. **Executive Summary** — 2-3 sentence narrative of the month
2. **Completed Items** — Initiatives/deliverables grouped by owner with highlights
3. **In-Progress by Priority** — Active workstreams with status and target dates
4. **Blocked / At-Risk** — Items needing escalation with context
5. **Key Metrics** — Month-over-month and year-to-date tables
6. **Narrative Template** — Leadership-ready framing of the month
7. **Next Month Preview** — Upcoming milestones and focus areas
8. **Asks / Escalations** — What you need from leadership

## Triggers

```
/mbr                    # Current month MBR
/mbr [month]            # Specific month (e.g., /mbr january, /mbr march)
/mbr --format deck      # Bullet-point format for slides
/mbr --format email     # Narrative format for email
/mbr --source sharepoint  # Pull from SharePoint/MS Lists as primary source
/mbr --source excel       # Pull from Excel trackers as primary source
/mbr --source manual      # Prompt user to paste in data
```

## Data Sources (M365-First)

MBR data for solutions and partner marketing teams lives across **multiple sources**. This skill supports all of them. GitHub Issues is treated as an optional, supplemental source — not the primary.

### Primary Sources (in priority order)

| Source | What It Contains | How to Pull |
|--------|-----------------|-------------|
| **SharePoint Lists / MS Lists** | Initiative status, owners, % complete, project health | Query via Graph API or have user paste list export |
| **Excel Trackers** (SharePoint/OneDrive) | Budget vs. plan, event results, metrics, partner pipeline | Read from file path or have user paste key numbers |
| **Power BI Dashboards** | KPIs, registrations, completions, pipeline — with MoM/YTD | Screenshot or numeric export; user must provide values |
| **Microsoft Planner / Tasks** | Task completions by owner, milestone tracking | Aggregate via Graph API or user summary |
| **Teams Channels & Outlook** | Blockers, escalations, qualitative wins not in trackers | Scan or user paste; use Copilot search if available |
| **Shared Docs** (Word/OneNote) | Narrative updates from each program lead | Read file or have user paste content |

### Secondary Sources (if applicable)

| Source | Notes |
|--------|-------|
| **GitHub Issues** | Useful for technical/product-adjacent marketing teams tracking work items |
| **Azure DevOps / Jira** | Same pattern as GitHub — query and count by label/status |
| **CRM (Dynamics/Salesforce)** | Partner pipeline, leads generated — user must export or paste |
| **Event platforms** | Registration/attendance counts — user must paste from platform |

## Prerequisites

- Access to at least one of the data sources above
- If using SharePoint Lists: Graph API access or exported list data
- If using Excel: file path accessible or user pastes key numbers
- If using Power BI: user provides metric values (dashboard screenshots acceptable)
- If using GitHub Issues (optional): `gh` CLI authenticated with repo access
- Team uses consistent status language (on-track / at-risk / complete / blocked)

## Implementation

### Step 1: Determine Date Range

```
MONTH_START = first day of the target month
MONTH_END   = last day of the target month
```

### Step 2: Identify Available Data Sources

Ask the user (or detect from context) which sources are available:

```
Available sources: SharePoint, Excel, Power BI, Planner, Teams/email, manual input, GitHub Issues (optional)
```

If no source is specified, default to **prompt mode**: ask the user to provide data per section or confirm sources to query.

### Step 3: Gather Data by Section

#### Completions (What was finished this month)

**From SharePoint/MS Lists:**
- Filter items where `Status = "Complete"` and `CompletedDate >= MONTH_START`
- Group by `Owner` field
- Identify P0/P1 items or milestones as highlights

**From Excel tracker:**
- Look for rows marked "Done" / "Complete" with a date in the target month
- Group by owner column

**From Planner/Tasks:**
- Completed tasks with completion date in range, grouped by assignee

**From GitHub Issues (if in use):**
```bash
gh issue list --repo "$REPO" \
  --state closed \
  --search "closed:>=${MONTH_START}" \
  --json number,title,labels,closedAt,assignees \
  --limit 100
```

**If data unavailable:** Prompt the user:
> "Please paste or describe what was completed this month. Include owner and brief description."

#### In-Progress Items

**From SharePoint/MS Lists:**
- Filter items where `Status != "Complete"` and `Status != "Blocked"`
- Sort by priority field (P0/P1 first)

**From Excel tracker:**
- Rows marked "In Progress" with an owner and target date

**From Planner/Tasks:**
- Active tasks, filtered by priority label or bucket

#### Blocked / At-Risk

**From SharePoint/MS Lists:**
- Filter items where `Status = "At Risk"` or `Status = "Blocked"`

**From Teams/Outlook:**
- Search for messages containing "blocked," "at risk," "delayed," "escalation" in the target month
- Use Copilot in Teams/Outlook to surface relevant threads if available

**From any tracker:**
- Items with overdue dates or explicitly flagged as blocked

#### Key Metrics

**From Power BI dashboards:**
- User provides current-month values and last-month values
- If YTD/target available, include those

**From event platforms or CRM (user-provided):**
- Registrations, attendance, certifications, pipeline influence

**Common metrics for Solutions Marketing MBRs:**
- Event/webinar registrations and attendance
- Content deliverables shipped (blogs, modules, case studies)
- Training completions or certifications (partner or customer)
- Partner pipeline influenced ($ or deals)
- New leads or opportunities generated

> **Note:** Metric values beyond task completion counts require data from external systems. The MBR template includes placeholders — you must provide values from your dashboards, CRM, or event platforms. Flag any unavailable metrics explicitly rather than omitting them.

### Step 4: Flag Data Gaps

Before generating the MBR, explicitly list what's missing:

```
Data gaps detected:
- Key Metrics section: No Power BI values provided. Please supply current month and last month figures for [Metric 1], [Metric 2], [Metric 3].
- Completions: No data from Excel tracker — using SharePoint list only.
- Qualitative updates: No Teams/email scan — escalations section may be incomplete.
```

### Step 5: Generate Narrative

Synthesize data into the executive summary. Focus on:
- What the team delivered (outcomes, not activities)
- What moved forward vs. what stalled
- Key decisions made or needed
- Connection to strategic priorities

## Output Format

```markdown
# Monthly Business Review — [Month] [Year]

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
- [Major accomplishment 1 — why it matters]
- [Major accomplishment 2 — why it matters]
- [Major accomplishment 3 — why it matters]

---

## In Progress

### P0 Items (Critical Path)
| Initiative | Owner | Status | Target Date |
|------------|-------|--------|-------------|
| [Title] | [Name] | [Status] | [Date] |

### P1 Items (Important)
| Initiative | Owner | Status | Target Date |
|------------|-------|--------|-------------|
| [Title] | [Name] | [Status] | [Date] |

---

## Blocked / At-Risk

| Initiative | Owner | Blocker | Days Stuck | Escalation Needed? |
|------------|-------|---------|------------|-------------------|
| [Title] | [Name] | [Description] | X | Yes/No |

**Escalation Actions:**
- [Item]: [What's needed and from whom]

---

## Key Metrics

### Month-over-Month
| Metric | This Month | Last Month | Delta | Trend |
|--------|------------|------------|-------|-------|
| [Program metric 1] | XX | XX | +X% | Up/Down |
| [Program metric 2] | XX | XX | +X% | Up/Down |
| [Program metric 3] | XX | XX | +X% | Up/Down |
| [Program metric 4] | XX | XX | +X% | Up/Down |

### Year-to-Date
| Metric | YTD Actual | YTD Target | % to Goal |
|--------|------------|------------|-----------|
| [Key metric 1] | XX | XX | XX% |
| [Key metric 2] | XX | XX | XX% |
| [Key metric 3] | XX | XX | XX% |

> **Note:** Metric values come from dashboards, event platforms, or CRM — not from task tracking systems. Any missing values are flagged above.

---

## Next Month Preview

### Key Dates
| Date | Event / Milestone | Owner |
|------|-------------------|-------|
| [Date] | [Event] | [Name] |

### Focus Areas
1. [Focus area 1 — what and why]
2. [Focus area 2 — what and why]
3. [Focus area 3 — what and why]

---

## Asks / Escalations for Leadership

| Ask | For Whom | Urgency | Context |
|-----|----------|---------|---------|
| [What you need] | [Manager/VP] | High/Med/Low | [Why it matters now] |

---
*Generated [timestamp] from [data sources used]*
```

## Leadership Framing Tips

1. **Lead with insight, not activity.** "We accelerated partner onboarding by 40%" beats "We closed 12 partner tasks."
2. **Frame constraints positively.** "Delivered with reduced vendor capacity" instead of "We were short-staffed."
3. **Include competitive context where relevant.** "This positions us ahead of [competitor]'s timeline" adds strategic value.
4. **Connect to strategic narrative.** Every highlight should tie back to a team or organizational priority.
5. **Separate what you built from what you need.** Keeps the "asks" section clean and actionable.

## Format Variants

### Deck Format (`--format deck`)

Strip to bullet points. No tables. Each section is 3–5 bullets max. Designed for copy-paste into PowerPoint or Google Slides.

### Email Format (`--format email`)

Narrative paragraphs instead of tables. Conversational but professional tone. Designed for a leadership email thread.

## Gotchas

- **Data is scattered.** The biggest challenge is that MBR data lives across 5–7 different systems. Plan for 30–60 minutes of manual data collection even with automation assist.
- **SharePoint lists require team discipline.** If team members don't update their status fields, the list is stale. Always confirm with owners before publishing.
- **Power BI exports are snapshots.** Pull metric values at end-of-month (or first few days of the following month) for accuracy.
- **Excel trackers may have inconsistent owners/labels.** Normalize owner names and status values before aggregating.
- **Teams/email search surfaces qualitative data only.** Don't skip this step — blockers and risks often surface here before appearing in formal trackers.
- **Don't generate the MBR too early in the month.** Wait until at least the 3rd business day of the following month for a complete picture.
- **Historical MBR data compounds in value.** Save generated MBRs to a consistent location (e.g., `operations/mbr/YYYY-MM-mbr.md`) for trend analysis.
- **Metric values beyond "tasks closed" require external data.** Be explicit about what's missing rather than leaving blanks.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Primary data source | SharePoint + Excel | Specify `--source` flag |
| Highlight count | 3 | Adjust for leadership preference |
| Metric rows | 4 | Map to your team's KPIs |
| Output location | Terminal | File, clipboard, or SharePoint doc |
| Leadership tone | Insight-driven | Match your manager's preferences |

## Related Skills

- `/weekly-status` — Feeds into MBR as weekly data
- `/escalation-tracker` — Blocked items feed into MBR escalations
- `/epic-health` — Epic-level health complements MBR
- `/1on1-prep` — Individual data that rolls up to MBR
