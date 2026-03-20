# Weekly Board Health Check

**Cadence:** Every Monday at 8:00 AM PT
**Platform:** Copilot Studio + Power Automate
**Output channel:** Microsoft Teams channel post

---

## What This Does

Performs a hygiene audit across your GitHub project boards every Monday morning. It catches the issues that silently rot -- stale items no one has touched in two weeks, P0 tickets sitting without an owner, issues missing required labels, and orphaned tasks that are not connected to any epic. Without this check, boards degrade over time and project health becomes invisible until something breaks at review time.

## Sample Output

The following message appears in your Teams channel on Monday morning:

---

**Board Health Check -- Week of March 16**

**Overall Score: 78/100**

**Stale Issues -- No update in 14+ days (5)**
| Issue | Owner | Last Updated | Days Stale |
|-------|-------|-------------|------------|
| #142 Draft partner FAQ for NVIDIA program | [GTM Lead] | Mar 1 | 15 |
| #118 Finalize Reactor session lineup for April | [Hackathon Lead] | Feb 28 | 16 |
| #95 Update competitive positioning for Google Cloud Next | [GTM Lead] | Feb 27 | 17 |
| #201 Secure catering vendor for Build mixer | [Events Lead] | Mar 2 | 14 |
| #88 Review lab content for badge certification | [Hackathon Lead] | Feb 25 | 19 |

**Missing Required Labels (3)**
| Issue | Missing |
|-------|---------|
| #215 New blog post draft about agent architectures | `owner:`, `func:` |
| #218 Follow up with SDC team on acceleration program | `owner:`, `sp:` |
| #220 Prepare deck for [Sr. Director] sync | `func:` |

**P0 Without Owner (1)**
| Issue | Title | Created |
|-------|-------|---------|
| #198 | Build 2026 keynote demo environment broken | Mar 14 |

**Orphaned Issues -- Not linked to any epic (4)**
- #210 Order swag for hackathon winners
- #212 Test new registration form flow
- #214 Upload session recordings to SharePoint
- #216 Schedule dry run for Ignite CFP

**Recommendations:**
1. Assign #198 immediately -- P0 items need an owner within 24 hours
2. Review the 5 stale items in your next standup -- close or update each one
3. Add missing labels to #215, #218, #220 so they route to the correct boards

---

## Setup in Copilot Studio

### Step 1: Create the Agent (if not already created)

Use your **Reporting Engine** agent. (Same agent as the Daily Morning Pulse -- one agent can host multiple topics.)

### Step 2: Add the Trigger

1. Open your **Reporting Engine** agent
2. Go to **Topics** in the left sidebar
3. Click **+ Add** > **Topic** > **From blank**
4. Name the topic `Board Health Check`
5. For the trigger, select **When a flow calls this topic**

### Step 3: Configure the Topic

Add a **Message** node with the following instructions:

> You will receive a JSON object with four arrays: `staleIssues`, `missingLabels`, `unownedP0`, and `orphanedIssues`. Each item includes its issue number, title, owner (if any), labels, last updated date, and linked epic (if any).
>
> Calculate a health score from 0-100 using this formula:
> - Start at 100
> - Subtract 3 points per stale issue
> - Subtract 4 points per P0 without an owner
> - Subtract 2 points per issue missing required labels
> - Subtract 1 point per orphaned issue
> - Minimum score is 0
>
> Format the output as a Teams message with:
> - Header: "Board Health Check -- Week of [Month] [Day]"
> - Overall Score line
> - Tables for each category with relevant columns
> - A "Recommendations" section with 2-3 actionable next steps based on the worst findings
> - If a category has zero items, show it as "[Category] -- All clear" with a checkmark

Add an **Output** variable called `healthReport` of type Text.

### Step 4: Connect the Flow

The topic receives structured data from the Power Automate flow and returns `healthReport` for posting.

## Power Automate Flow Design

### Flow Name

`Reporting Engine - Post - Weekly Board Health`

### Trigger

**Recurrence** (Schedule connector)
- Frequency: Week
- Interval: 1
- On these days: Monday
- At these hours: 8
- Time zone: Pacific Standard Time

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `GitHubPAT` | String (env variable) | GitHub personal access token |
| `GitHubOwner` | String (env variable) | GitHub organization or username |
| `GitHubRepo` | String (env variable) | Repository name |
| `RequiredLabelPrefixes` | Array | `["owner:", "func:", "sp:"]` -- the label prefixes every issue should have |
| `StaleThresholdDays` | Integer | `14` -- how many days without an update counts as stale |

### Actions (Step by Step)

1. **HTTP -- Get all open issues (paginated)**
   - Connector: HTTP
   - Method: GET
   - URI: `https://api.github.com/repos/@{variables('GitHubOwner')}/@{variables('GitHubRepo')}/issues?state=open&per_page=100&page=1`
   - Headers: `Authorization: Bearer @{variables('GitHubPAT')}`, `Accept: application/vnd.github+json`
   - Note: If you have more than 100 open issues, add a **Do Until** loop incrementing the page number until the response returns an empty array. Merge all pages into a single array.

2. **Select -- Extract relevant fields**
   - Connector: Data Operation > Select
   - From: the combined issues array from step 1
   - Map:
     - `number`: `@{item()?['number']}`
     - `title`: `@{item()?['title']}`
     - `labels`: `@{item()?['labels']}`
     - `assignee`: `@{item()?['assignee']?['login']}`
     - `updated_at`: `@{item()?['updated_at']}`
     - `body`: `@{item()?['body']}`

3. **Filter -- Stale issues**
   - Connector: Data Operation > Filter array
   - From: output of step 2
   - Condition: `updated_at` is less than `@{formatDateTime(addDays(utcNow(), -14), 'yyyy-MM-ddTHH:mm:ssZ')}`

4. **Filter -- Missing required labels**
   - Connector: Data Operation > Filter array
   - From: output of step 2
   - Condition: Use an **Apply to each** with a **Compose** that checks whether the issue's label names contain at least one label starting with each required prefix. Collect issues where any prefix is missing.

5. **Filter -- P0 without owner**
   - Connector: Data Operation > Filter array
   - From: output of step 2
   - Condition: labels array contains a label with name matching `P0` AND `assignee` is null

6. **Filter -- Orphaned issues (not linked to an epic)**
   - Connector: Data Operation > Filter array
   - From: output of step 2
   - Condition: body does not contain `Epic:` or `Parent:` or any issue cross-reference pattern AND labels do not contain `type:initiative` (initiatives are epics themselves, not orphans)

7. **Compose -- Build health check payload**
   - Connector: Data Operation > Compose
   - Input: JSON object with keys `staleIssues`, `missingLabels`, `unownedP0`, `orphanedIssues`, each containing the filtered arrays from steps 3-6

8. **Copilot Studio -- Run Board Health Check topic**
   - Connector: Microsoft Copilot Studio
   - Action: Run a flow-connected topic
   - Agent: Reporting Engine
   - Topic: Board Health Check
   - Input: the composed payload from step 7

9. **Microsoft Teams -- Post message in channel**
   - Connector: Microsoft Teams
   - Action: Post message in a chat or channel
   - Team: `@{variables('TeamsTeamId')}`
   - Channel: `@{variables('TeamsChannelId')}`
   - Message: `@{outputs('Run_Board_Health_Check_topic')?['healthReport']}`

### Output

A formatted Teams channel message with health score, issue tables, and recommendations.

## Customization

| Setting | Default | How to Change |
|---------|---------|---------------|
| Run day | Monday | Edit the Recurrence trigger "On these days" |
| Run time | 8:00 AM PT | Edit the Recurrence trigger hours |
| Stale threshold | 14 days | Change `StaleThresholdDays` variable and the `-14` in the filter |
| Required label prefixes | `owner:`, `func:`, `sp:` | Edit the `RequiredLabelPrefixes` variable |
| Score formula | See topic instructions | Edit the Copilot Studio topic message node |
| Epic linkage detection | Checks body for `Epic:` or `Parent:` | Adjust the orphan filter condition in step 6 if your repo uses a different convention |

## Gotchas

- The GitHub REST API returns pull requests in the `/issues` endpoint. Filter them out by checking that `pull_request` key is absent on each item, or add `?type=issue` if your API version supports it.
- Pagination is critical. If you have 200+ open issues and only fetch page 1, you will miss half your board. Always paginate.
- The "orphaned issue" detection relies on body text or labels. If your team links epics via GitHub sub-issues (beta feature) instead of body text, you will need to use the GraphQL API to check the `sub_issues` connection.
- Label prefix matching is case-sensitive. If someone creates `owner:[events-lead]` instead of `owner:[events-lead]`, the filter will flag it as missing. Normalize to lowercase in the Select step.
- This flow can take 30-60 seconds to run on repos with 300+ issues. Power Automate has a 5-minute timeout for HTTP actions, so you have headroom, but monitor run history if issues spike.
