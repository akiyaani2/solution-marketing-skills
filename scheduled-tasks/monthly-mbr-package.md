# Monthly MBR Data Package

**Cadence:** 8th of every month at 7:00 AM PT
**Platform:** Copilot Studio + Power Automate
**Output channel:** SharePoint document library + Teams notification

---

## What This Does

Assembles the raw data and narrative scaffolding for the Monthly Business Review package on the 8th of each month, giving the team lead a full week before the typical MBR meeting window (around the 19th). Instead of spending a day pulling GitHub data, counting completions, and formatting tables, the automation delivers a pre-built document with all the numbers filled in and narrative placeholders ready for human judgment. The team lead reviews, adds strategic commentary, and the package is ready.

## Sample Output

A markdown file saved to SharePoint at `/MBR Reports/MBR-2026-03.md` with this content:

---

**Monthly Business Review -- March 2026**
*AI Apps & Agents Skilling Team*
*Prepared: March 8, 2026*

### Executive Summary
> [FILL IN: 3-4 sentence narrative covering the month's theme, biggest win, and key risk. Frame in terms of pipeline impact and competitive positioning.]

### Completions by Owner

| Owner | Closed | Opened | Net | Completion Rate |
|-------|--------|--------|-----|-----------------|
| [Events Lead] | 8 | 3 | +5 | 73% |
| [GTM Lead] | 6 | 4 | +2 | 60% |
| [Hackathon Lead] | 5 | 6 | -1 | 45% |
| [Ops Lead] | 3 | 1 | +2 | 75% |
| **Total** | **22** | **14** | **+8** | **61%** |

### Initiative Health

| Initiative | Status | Owner | % Complete | Notes |
|-----------|--------|-------|------------|-------|
| Build 2026 Skilling Track | On Track | [Events Lead] | 65% | Content lock on schedule |
| NVIDIA 1:Many Hack Series | At Risk | [Hackathon Lead] | 40% | MDF approval delayed |
| AI Show 2.0 | On Track | [Hackathon Lead] | 55% | Episodes 1-3 shipped |
| Competitive Intel Dashboard | On Track | [GTM Lead] | 80% | Final viz pass remaining |
| 8K Marketplace Hack | On Track | [Events Lead] | 35% | Venue locked, speakers pending |
| Cross-Solution Sync Framework | Complete | [GTM Lead] | 100% | Delivered to [Peer Leads] |

### Blocked Items

| Issue | Owner | Blocked Since | Reason | Escalation Needed? |
|-------|-------|--------------|--------|-------------------|
| #142 NVIDIA MDF approval | [Events Lead] | Feb 28 | Partner legal review | Yes -- escalate to [Sr. Director] |
| #188 Lab badge cert content | [Hackathon Lead] | Mar 5 | Learn platform team queue | No -- expected Mar 12 |

### Key Metrics

| Metric | This Month | Last Month | Trend |
|--------|-----------|------------|-------|
| Total issues closed | 22 | 18 | +22% |
| Avg days to close | 5.3 | 6.8 | Improving |
| P0 items resolved | 3 | 2 | +50% |
| Blocked items (end of month) | 2 | 4 | Improving |
| New issues created | 14 | 16 | -12% |

### Events & Hackathons This Month

| Event | Date | Status | Attendees/Registrations |
|-------|------|--------|------------------------|
| NVIDIA 1:Many Hack #4 | Mar 12 | Completed | 142 registrations |
| Reactor: AI Agents Deep Dive | Mar 18 | Completed | 89 live attendees |
| AI Dev Days planning kickoff | Mar 22 | Upcoming | N/A |

### Budget Snapshot
> [FILL IN: Pull current spend vs. allocation from budget tracker. Note any MDF utilization updates.]

### Stakeholder Visibility
> [FILL IN: Key items [Sr. Director]/[Director] need to know. Decisions needed. Escalations. Wins to highlight upward.]

### Next Month Preview
- Build 2026 content lock deadline (Apr 15)
- NVIDIA 1:Many Hack #5 (Apr 9)
- AI Show 2.0 episode 4 recording (Apr 3)
- Q4 planning kickoff with [Director] (Apr 7)

---

## Setup in Copilot Studio

### Step 1: Create the Agent (if not already created)

Use your **Reporting Engine** agent. Add a new topic for MBR generation.

### Step 2: Add the Trigger

1. Open your **Reporting Engine** agent
2. Go to **Topics** in the left sidebar
3. Click **+ Add** > **Topic** > **From blank**
4. Name the topic `MBR Package`
5. For the trigger, select **When a flow calls this topic**

### Step 3: Configure the Topic

Add a **Message** node with the following instructions:

> You will receive a JSON object with: `closedByOwner` (object with owner names as keys and arrays of issues as values), `openByInitiative` (array of initiatives with completion percentages), `blockedItems` (array), `metricsThisMonth` and `metricsLastMonth` (objects with counts), `eventsThisMonth` (array), and `upcomingNextMonth` (array of key dates).
>
> Generate a complete MBR document in markdown format with these sections:
> 1. Executive Summary -- leave as a placeholder with guidance: "[FILL IN: 3-4 sentence narrative...]"
> 2. Completions by Owner -- table with Closed, Opened, Net, Completion Rate columns. Calculate completion rate as closed / (closed + still open at month end) x 100.
> 3. Initiative Health -- table with Status (On Track / At Risk / Blocked / Complete), Owner, % Complete, and Notes. Derive % complete from closed sub-issues vs total sub-issues.
> 4. Blocked Items -- table with issue number, owner, blocked since date, reason (from latest comment or label), and whether escalation is needed (yes if blocked > 10 days).
> 5. Key Metrics -- comparison table for this month vs last month with trend indicators.
> 6. Events & Hackathons -- table of events that occurred or are occurring this month.
> 7. Budget Snapshot -- placeholder: "[FILL IN: Pull current spend...]"
> 8. Stakeholder Visibility -- placeholder: "[FILL IN: Key items [Sr. Director]/[Director] need to know...]"
> 9. Next Month Preview -- bullet list of key dates in the coming month.
>
> Use professional, insight-driven language. Frame trends positively where possible but flag genuine risks clearly. This document will be reviewed by a senior director.

Add an **Output** variable called `mbrDocument` of type Text.

### Step 4: Connect the Flow

The topic receives aggregated monthly data and returns the full MBR document as markdown text.

## Power Automate Flow Design

### Flow Name

`Reporting Engine - Generate - Monthly MBR Package`

### Trigger

**Recurrence** (Schedule connector)
- Frequency: Month
- Interval: 1
- On these days: 8
- At these hours: 7
- Time zone: Pacific Standard Time

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `GitHubPAT` | String (env variable) | GitHub personal access token |
| `GitHubOwner` | String (env variable) | GitHub organization or username |
| `GitHubRepo` | String (env variable) | Repository name |
| `SharePointSiteUrl` | String (env variable) | SharePoint site for report storage |
| `SharePointFolderPath` | String (env variable) | Folder path (e.g., `/MBR Reports`) |
| `TeamsTeamId` | String (env variable) | Teams team ID for notification |
| `TeamsChannelId` | String (env variable) | Teams channel ID for notification |
| `OwnerLabels` | Array | `["owner:[events-lead]", "owner:[gtm-lead]", "owner:[hack-lead]", "owner:[ops-lead]"]` |

### Actions (Step by Step)

1. **Compose -- Calculate date range for this month and last month**
   - Connector: Data Operation > Compose
   - Input:
     ```json
     {
       "thisMonthStart": "@{formatDateTime(startOfMonth(addDays(utcNow(), -7)), 'yyyy-MM-dd')}",
       "thisMonthEnd": "@{formatDateTime(utcNow(), 'yyyy-MM-dd')}",
       "lastMonthStart": "@{formatDateTime(startOfMonth(addMonths(addDays(utcNow(), -7), -1)), 'yyyy-MM-dd')}",
       "lastMonthEnd": "@{formatDateTime(endOfMonth(addMonths(addDays(utcNow(), -7), -1)), 'yyyy-MM-dd')}"
     }
     ```
   - Note: `addDays(utcNow(), -7)` anchors to the 1st of the reporting month (since the flow runs on the 8th).

2. **Apply to Each -- Get closed issues by owner (this month)**
   - Connector: Control > Apply to each
   - Input: `OwnerLabels` array
   - Inside the loop:
     - **HTTP -- Get closed issues for this owner this month**
       - Method: GET
       - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:closed+label:@{items('Apply_to_each')}+closed:@{outputs('date_range')?['thisMonthStart']}..@{outputs('date_range')?['thisMonthEnd']}`
       - Headers: standard GitHub headers
     - **Append to array variable** -- store owner name + count + issue details

3. **Apply to Each -- Get closed issues by owner (last month)** (same structure as step 2 but with last month date range)

4. **HTTP -- Get all open issues with type:initiative label**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+label:type:initiative`
   - Headers: standard GitHub headers

5. **Apply to Each -- Calculate initiative completion**
   - For each initiative from step 4, search for sub-issues that reference it (by issue number in body text)
   - Count closed vs total to get completion percentage

6. **HTTP -- Get blocked issues**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+label:status:blocked`
   - Headers: standard GitHub headers

7. **HTTP -- Get issues with event labels (this month)**
   - Search for issues with `event:` or `func:events` or `func:hackathon` labels that have dates in the current month

8. **HTTP -- Get issues due next month**
   - Search for open issues with due dates in the following month for the "Next Month Preview" section

9. **Compose -- Build MBR payload**
   - Connector: Data Operation > Compose
   - Combine all data from steps 2-8 into the JSON structure the Copilot Studio topic expects

10. **Copilot Studio -- Run MBR Package topic**
    - Connector: Microsoft Copilot Studio
    - Action: Run a flow-connected topic
    - Agent: Reporting Engine
    - Topic: MBR Package
    - Input: the composed payload from step 9

11. **SharePoint -- Create file**
    - Connector: SharePoint
    - Action: Create file
    - Site Address: `@{variables('SharePointSiteUrl')}`
    - Folder Path: `@{variables('SharePointFolderPath')}`
    - File Name: `MBR-@{formatDateTime(addDays(utcNow(), -7), 'yyyy-MM')}.md`
    - File Content: `@{outputs('Run_MBR_Package_topic')?['mbrDocument']}`

12. **Microsoft Teams -- Post notification**
    - Connector: Microsoft Teams
    - Action: Post message in a chat or channel
    - Team: `@{variables('TeamsTeamId')}`
    - Channel: `@{variables('TeamsChannelId')}`
    - Message: `MBR data package for @{formatDateTime(addDays(utcNow(), -7), 'MMMM yyyy')} is ready for review. [View in SharePoint](@{outputs('Create_file')?['body/Path']}). Placeholders marked [FILL IN] need your input before the review meeting.`

### Output

A markdown document saved to SharePoint plus a Teams notification linking to the file.

## Customization

| Setting | Default | How to Change |
|---------|---------|---------------|
| Run date | 8th of month | Edit Recurrence trigger day. If the 8th falls on a weekend, Power Automate still runs; add a Condition to skip or shift to Monday. |
| Output format | Markdown (.md) | Change the file extension to `.docx` and use the Word Online connector "Convert file" action to produce a Word document |
| Owner list | [Events Lead], [GTM Lead], [Hackathon Lead], [Ops Lead] | Edit the `OwnerLabels` array |
| SharePoint location | `/MBR Reports` | Change `SharePointFolderPath` env variable |
| Fill-in placeholders | 3 sections | Add or remove placeholder sections in the Copilot Studio topic instructions |
| Completion rate formula | closed / (closed + open) | Edit the topic instructions to use a different denominator |

## Gotchas

- The flow runs on the 8th, which means it captures data through the 7th. Issues closed on the 8th before the flow runs will be included; issues closed later that day will not. If precision matters, set the `thisMonthEnd` to the last day of the previous month instead.
- Initiative completion calculation (step 5) relies on sub-issues referencing the parent initiative by issue number in the body text. If your team uses GitHub sub-issues (beta) or a different linking convention, adjust the search query accordingly.
- The SharePoint "Create file" action will fail if a file with the same name already exists. Add a "Get file metadata" action first to check, and use "Update file" if it exists. Or append a timestamp to the filename.
- Power Automate's `startOfMonth()` and `endOfMonth()` functions are available in Workflow Definition Language but may not appear in the expression builder UI. Type them manually in the expression editor.
- This flow makes 10-15 HTTP requests depending on team size. Space them out or run in sequence to avoid GitHub rate limits. The Apply to Each loops run sequentially by default, which helps.
- The MBR document is a starting point, not a finished product. The placeholders for Executive Summary, Budget Snapshot, and Stakeholder Visibility intentionally require human input -- these sections need strategic judgment that an automation cannot reliably provide.
