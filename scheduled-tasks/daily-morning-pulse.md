# Daily Morning Pulse

**Cadence:** Every weekday at 9:00 AM PT
**Platform:** Copilot Studio + Power Automate
**Output channel:** Microsoft Teams channel post

---

## What This Does

Delivers a concise morning briefing to your team's Teams channel so everyone starts the day knowing what is due, what is stuck, and what moved yesterday. Without this, team leads spend 15-20 minutes manually scanning GitHub boards each morning to assemble the same picture. The automation eliminates that overhead and ensures nothing slips through the cracks overnight.

## Sample Output

The following message appears in your Teams channel at 9:00 AM:

---

**Daily Pulse -- Wednesday, March 18**

**Due Today (3)**
- [ ] Finalize hackathon registration page copy -- *[Hackathon Lead]* -- P1
- [ ] Submit Build session abstracts to review board -- *[GTM Lead]* -- P0
- [ ] Send updated budget forecast to finance -- *[Ops Lead]* -- P1

**Blocked (1)**
- NVIDIA MDF approval waiting on partner legal review -- *[Events Lead]* -- stuck 4 days

**Closed Yesterday (2)**
- Competitive landscape deck for Q3 planning -- *[GTM Lead]*
- Reactor session recording uploaded to Learn -- *[Hackathon Lead]*

**No Update in 7+ Days (2)**
- Blog post: AI Agents customer story -- *[GTM Lead]* -- last updated Mar 10
- 3P event sponsor contract review -- *[Events Lead]* -- last updated Mar 9

---

## Setup in Copilot Studio

### Step 1: Create the Agent (if not already created)

Use your **Reporting Engine** agent. If you have not created one yet:
1. Go to [Copilot Studio](https://copilotstudio.microsoft.com)
2. Click **Create** > **New agent**
3. Name it `Reporting Engine`
4. Set the description to: "Generates scheduled reports and summaries from GitHub project data"
5. Click **Create**

### Step 2: Add the Trigger

1. Open your **Reporting Engine** agent
2. Go to **Topics** in the left sidebar
3. Click **+ Add** > **Topic** > **From blank**
4. Name the topic `Morning Pulse`
5. For the trigger, select **When a flow calls this topic** (this allows Power Automate to invoke it)

### Step 3: Configure the Topic

Add a **Message** node with the following instructions for the agent:

> You will receive three JSON arrays: `dueToday`, `blocked`, and `closedYesterday`, plus an array called `stale` for items with no update in 7+ days. Format them into a Teams-friendly summary using this structure:
>
> - Header: "Daily Pulse -- [Day of week], [Month] [Day]"
> - Section 1: "Due Today" -- list each item with title, owner, and priority
> - Section 2: "Blocked" -- list each item with title, owner, and days stuck
> - Section 3: "Closed Yesterday" -- list each item with title and owner
> - Section 4: "No Update in 7+ Days" -- list each item with title, owner, and last updated date
> - If any section has zero items, show "None" under that heading

Add an **Output** variable called `formattedMessage` of type Text.

### Step 4: Connect the Flow

The topic receives input from the Power Automate flow below and returns `formattedMessage` back to the flow for posting to Teams.

## Power Automate Flow Design

### Flow Name

`Reporting Engine - Post - Daily Morning Pulse`

### Trigger

**Recurrence** (Schedule connector)
- Frequency: Week
- Interval: 1
- On these days: Monday, Tuesday, Wednesday, Thursday, Friday
- At these hours: 9
- Time zone: Pacific Standard Time

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `GitHubPAT` | String (env variable) | GitHub personal access token for API authentication |
| `GitHubOwner` | String (env variable) | GitHub organization or username |
| `GitHubRepo` | String (env variable) | Repository name |
| `TeamsTeamId` | String (env variable) | Target Teams team ID |
| `TeamsChannelId` | String (env variable) | Target Teams channel ID |

### Actions (Step by Step)

1. **HTTP -- Get issues due today**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+due:@{formatDateTime(utcNow(),'yyyy-MM-dd')}`
   - Headers: `Authorization: Bearer @{variables('GitHubPAT')}`, `Accept: application/vnd.github+json`

2. **HTTP -- Get blocked issues**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+label:status:blocked`
   - Headers: same as above

3. **HTTP -- Get issues closed yesterday**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:closed+closed:>@{formatDateTime(addDays(utcNow(),-1),'yyyy-MM-dd')}`
   - Headers: same as above

4. **HTTP -- Get stale issues (no update in 7+ days)**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/search/issues?q=repo:@{variables('GitHubOwner')}/@{variables('GitHubRepo')}+is:issue+is:open+updated:<@{formatDateTime(addDays(utcNow(),-7),'yyyy-MM-dd')}`
   - Headers: same as above

5. **Compose -- Build input payload**
   - Connector: Data Operation > Compose
   - Input: JSON object combining the four API responses into `dueToday`, `blocked`, `closedYesterday`, and `stale` arrays (map each to title, assignee, labels, updated_at)

6. **Copilot Studio -- Run Morning Pulse topic**
   - Connector: Microsoft Copilot Studio
   - Action: Run a flow-connected topic
   - Agent: Reporting Engine
   - Topic: Morning Pulse
   - Input: the composed JSON payload from step 5

7. **Microsoft Teams -- Post message in channel**
   - Connector: Microsoft Teams
   - Action: Post message in a chat or channel
   - Team: `@{variables('TeamsTeamId')}`
   - Channel: `@{variables('TeamsChannelId')}`
   - Message: `@{outputs('Run_Morning_Pulse_topic')?['formattedMessage']}`

### Output

A formatted Teams channel message visible to the entire team.

## Customization

| Setting | Default | How to Change |
|---------|---------|---------------|
| Post time | 9:00 AM PT | Edit the Recurrence trigger hours and time zone |
| Stale threshold | 7 days | Change the `-7` in step 4 URI to your preferred number |
| Blocked label | `status:blocked` | Update the label filter in step 2 URI |
| Include weekends | No (Mon-Fri only) | Add Saturday/Sunday to the Recurrence trigger days |
| Channel | Single channel | Change `TeamsChannelId` env variable or duplicate the post step for multiple channels |

## Gotchas

- GitHub Search API has a rate limit of 30 requests per minute for authenticated users. This flow makes 4 requests, so you are well within limits for a single run.
- The `due:` qualifier in GitHub search only works if your issues use the milestone due date or a project field mapped to a date. If you use custom project fields for due dates, you will need to use the GraphQL API (step 1 becomes an HTTP POST to `https://api.github.com/graphql` with a project items query).
- Power Automate Recurrence triggers can drift by a few minutes. If exact timing matters, use the "Start time" field in the trigger to anchor it.
- If the Copilot Studio agent takes more than 120 seconds to respond, the flow will time out. For large repos with hundreds of open issues, consider pre-filtering in the Compose step to limit payload size.
- Make sure your GitHub PAT has `repo` scope (classic) or `Issues: Read` permission (fine-grained). Without it, private repo queries return empty results with no error.
