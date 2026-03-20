# Weekly Friday Digest

**Cadence:** Every Friday at 2:00 PM PT
**Platform:** Copilot Studio + Power Automate
**Output channel:** Microsoft Teams channel post

---

## What This Does

Sends a week-in-review summary to the team channel every Friday afternoon. This is the highlight reel -- what the team shipped, what is actively being worked on, what is stuck, and what is coming up next week. It replaces the manual end-of-week recap that someone would otherwise have to write by hand, and gives the team a shared sense of momentum going into the weekend.

## Sample Output

The following message appears in your Teams channel on Friday at 2:00 PM:

---

**Week in Review -- March 16-20**

**Shipped This Week (6)**
- Competitive landscape deck for Q3 planning -- *[GTM Lead]*
- Reactor session recording uploaded to Learn -- *[Hackathon Lead]*
- NVIDIA 1:Many registration page live -- *[Hackathon Lead]*
- Build 2026 session abstracts submitted -- *[GTM Lead]*
- Hackathon winner swag order placed -- *[Events Lead]*
- Updated vendor SOW for Q4 -- *[Ops Lead]*

**In Progress (8)**
- Build 2026 keynote demo environment setup -- *[Events Lead]* -- 60% complete
- Agent Fest speaker confirmations -- *[Hackathon Lead]* -- waiting on 3 of 7 speakers
- Blog series: AI Agents in Production -- *[GTM Lead]* -- draft 2 of 4 published
- Q4 budget forecast -- *[Ops Lead]* -- final review pending
- NVIDIA MDF tracking dashboard -- *[GTM Lead]* -- data model complete, viz in progress
- 8K Marketplace hack logistics -- *[Events Lead]* -- venue confirmed, AV pending
- AI Show 2.0 episode 3 script -- *[Hackathon Lead]* -- first draft complete
- Cross-solution sync deck for [Sr. Director] -- *[GTM Lead]* -- outline approved

**Blocked (2)**
- NVIDIA MDF approval -- *[Events Lead]* -- waiting on partner legal (9 days)
- Lab badge certification content -- *[Hackathon Lead]* -- waiting on Learn platform team (5 days)

**Coming Next Week**
- Monday: Hackathon planning sync ([Hackathon Lead], [Events Lead])
- Tuesday: [Sr. Director] 1:1 prep due
- Wednesday: AI Show 2.0 episode 3 recording
- Thursday: Build 2026 content lock deadline
- Friday: Peer digest goes out to [Peer Lead 1] and [Peer Lead 2]

**Team Stats**
| Metric | This Week | Last Week | Trend |
|--------|-----------|-----------|-------|
| Issues closed | 6 | 4 | Up |
| Issues opened | 3 | 5 | Down |
| Avg days to close | 4.2 | 6.1 | Improving |
| Blocked items | 2 | 3 | Improving |

---

## Setup in Copilot Studio

### Step 1: Create the Agent (if not already created)

Use your **Reporting Engine** agent. (Same agent used for the Daily Pulse and Board Health Check.)

### Step 2: Add the Trigger

1. Open your **Reporting Engine** agent
2. Go to **Topics** in the left sidebar
3. Click **+ Add** > **Topic** > **From blank**
4. Name the topic `Friday Digest`
5. For the trigger, select **When a flow calls this topic**

### Step 3: Configure the Topic

Add a **Message** node with the following instructions:

> You will receive a JSON object with these arrays: `closedThisWeek`, `inProgress`, `blocked`, `upcomingNextWeek`, and `statsThisWeek` / `statsLastWeek`.
>
> Format the output as a weekly digest with:
> - Header: "Week in Review -- [Month] [StartDay]-[EndDay]"
> - "Shipped This Week" -- list each closed issue with title and owner. This is the celebration section. Use confident, accomplished tone.
> - "In Progress" -- list each active issue with title, owner, and a brief status note (from the most recent comment or a default "in progress")
> - "Blocked" -- list each blocked issue with title, owner, reason, and days stuck
> - "Coming Next Week" -- list items due in the next 7 days, organized by day of week
> - "Team Stats" -- table comparing this week vs last week for issues closed, opened, avg days to close, and blocked count. Add a Trend column (Up/Down/Flat and Improving/Declining/Flat)
> - If "Shipped This Week" is empty, lead with "Quiet week on closures -- but the pipeline is building" instead of showing an empty section

Add an **Output** variable called `digestMessage` of type Text.

### Step 4: Connect the Flow

The topic receives structured data from the Power Automate flow and returns `digestMessage` for posting.

## Power Automate Flow Design

### Flow Name

`Reporting Engine - Post - Weekly Friday Digest`

### Trigger

**Recurrence** (Schedule connector)
- Frequency: Week
- Interval: 1
- On these days: Friday
- At these hours: 14
- Time zone: Pacific Standard Time

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `GitHubPAT` | String (env variable) | GitHub personal access token |
| `GitHubOwner` | String (env variable) | GitHub organization or username |
| `GitHubRepo` | String (env variable) | Repository name |
| `TeamsTeamId` | String (env variable) | Target Teams team ID |
| `TeamsChannelId` | String (env variable) | Target Teams channel ID |

### Actions (Step by Step)

1. **Compose -- Calculate date range**
   - Connector: Data Operation > Compose
   - Input:
     ```json
     {
       "weekStart": "@{formatDateTime(addDays(utcNow(), -4), 'yyyy-MM-dd')}",
       "weekEnd": "@{formatDateTime(utcNow(), 'yyyy-MM-dd')}",
       "nextWeekEnd": "@{formatDateTime(addDays(utcNow(), 7), 'yyyy-MM-dd')}",
       "lastWeekStart": "@{formatDateTime(addDays(utcNow(), -11), 'yyyy-MM-dd')}",
       "lastWeekEnd": "@{formatDateTime(addDays(utcNow(), -5), 'yyyy-MM-dd')}"
     }
     ```

2. **HTTP -- Get issues closed this week**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:closed+closed:@{outputs('Calculate_date_range')?['weekStart']}..@{outputs('Calculate_date_range')?['weekEnd']}`
   - Headers: `Authorization: Bearer @{variables('GitHubPAT')}`, `Accept: application/vnd.github+json`

3. **HTTP -- Get issues closed last week** (for comparison stats)
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:closed+closed:@{outputs('Calculate_date_range')?['lastWeekStart']}..@{outputs('Calculate_date_range')?['lastWeekEnd']}`
   - Headers: same as above

4. **HTTP -- Get in-progress issues**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+label:status:in-progress`
   - Headers: same as above
   - Note: If you use a project board status column instead of a label, you will need the GraphQL API to filter by project field value.

5. **HTTP -- Get blocked issues**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+label:status:blocked`
   - Headers: same as above

6. **HTTP -- Get issues due next week**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+due:@{outputs('Calculate_date_range')?['weekEnd']}..@{outputs('Calculate_date_range')?['nextWeekEnd']}`
   - Headers: same as above

7. **HTTP -- Get issues opened this week** (for stats)
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+created:@{outputs('Calculate_date_range')?['weekStart']}..@{outputs('Calculate_date_range')?['weekEnd']}`
   - Headers: same as above

8. **HTTP -- Get issues opened last week** (for stats)
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+created:@{outputs('Calculate_date_range')?['lastWeekStart']}..@{outputs('Calculate_date_range')?['lastWeekEnd']}`
   - Headers: same as above

9. **Compose -- Build digest payload**
   - Connector: Data Operation > Compose
   - Input: JSON object combining:
     - `closedThisWeek`: mapped from step 2 (number, title, assignee, closed_at)
     - `inProgress`: mapped from step 4 (number, title, assignee, labels, updated_at)
     - `blocked`: mapped from step 5 (number, title, assignee, labels, updated_at)
     - `upcomingNextWeek`: mapped from step 6 (number, title, assignee, due_date)
     - `statsThisWeek`: `{ closed: length(step2), opened: length(step7) }`
     - `statsLastWeek`: `{ closed: length(step3), opened: length(step8) }`

10. **Copilot Studio -- Run Friday Digest topic**
    - Connector: Microsoft Copilot Studio
    - Action: Run a flow-connected topic
    - Agent: Reporting Engine
    - Topic: Friday Digest
    - Input: the composed payload from step 9

11. **Microsoft Teams -- Post message in channel**
    - Connector: Microsoft Teams
    - Action: Post message in a chat or channel
    - Team: `@{variables('TeamsTeamId')}`
    - Channel: `@{variables('TeamsChannelId')}`
    - Message: `@{outputs('Run_Friday_Digest_topic')?['digestMessage']}`

### Output

A formatted Teams channel message with the week-in-review digest.

## Customization

| Setting | Default | How to Change |
|---------|---------|---------------|
| Post time | 2:00 PM PT Friday | Edit Recurrence trigger hours and day |
| In-progress detection | `status:in-progress` label | Change label in step 4 URI, or switch to GraphQL for project board status |
| Blocked detection | `status:blocked` label | Change label in step 5 URI |
| Week-over-week stats | Enabled | Remove steps 3, 7, 8 and the stats keys from the payload if not needed |
| Trend language | Improving/Declining/Flat | Edit the Copilot Studio topic instructions |
| Additional recipients | Teams channel only | Add an Outlook "Send an email" action after step 11 to also email the digest |

## Gotchas

- This flow makes 7 HTTP requests. GitHub Search API allows 30/minute for authenticated users, so you are safe. But if you run this alongside the Daily Pulse or other flows at the same time, stagger them by a few minutes to avoid rate limits.
- The `due:` search qualifier depends on how your repo tracks due dates. If due dates live in project custom fields rather than milestone dates, the search API will not find them. Use GraphQL project item queries instead.
- "In Progress" detection via labels only works if your team consistently applies the `status:in-progress` label. If your workflow uses project board columns instead, replace step 4 with a GraphQL query against the project board's Status field.
- The "avg days to close" stat requires calculating `closed_at - created_at` for each closed issue. Do this in a Compose action with an Apply to Each loop, then average the results. The Copilot Studio agent cannot do date math on raw ISO timestamps reliably.
- Large teams with 50+ closed issues per week may hit the Teams message character limit (28 KB for connector messages). If this happens, truncate the "Shipped" section to top 15 items and add a "and X more" note.
