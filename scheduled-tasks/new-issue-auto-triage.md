# New Issue Auto-Triage

**Cadence:** On every new GitHub issue (event-driven)
**Platform:** Copilot Studio + Power Automate
**Output channel:** GitHub issue comment

---

## What This Does

Analyzes every new GitHub issue the moment it is created and posts a comment suggesting labels for owner, function area, priority, and solution play. This eliminates the lag between issue creation and triage -- instead of waiting for a weekly review or someone to manually scan new issues, every issue gets immediate classification suggestions. The automation does NOT apply labels automatically; it suggests them for human review, so the team stays in control.

## Sample Output

When someone creates an issue titled "Draft blog post about AI Agent architectures for Build 2026", the following comment appears on the issue within 60 seconds:

---

**Triage Suggestion**

Based on the issue title and description, here are recommended labels:

| Category | Suggested Label | Confidence | Reason |
|----------|----------------|------------|--------|
| Owner | `owner:[gtm-lead]` | High | Content creation aligns with [GTM Lead]'s GTM & Content role |
| Function | `func:content` | High | "Blog post" indicates content workstream |
| Solution Play | `sp:ai-apps-agents` | High | "AI Agent architectures" maps to AI Apps & Agents play |
| Priority | `P1` | Medium | Build 2026 content has a hard deadline but is not yet urgent |
| Event | `event:build-2026` | High | Explicitly references Build 2026 |
| Type | `type:task` | High | Single deliverable, not an initiative or recurring item |

**To apply these labels**, add them manually or reply to this comment with "apply" to confirm.

**If these suggestions are wrong**, just apply the correct labels and ignore this comment. The triage model improves over time as it learns from corrections.

---
*Posted by Triage Agent*

---

## Setup in Copilot Studio

### Step 1: Create the Agent

Create a dedicated **Triage Agent** (separate from the Reporting Engine, since this agent has a different purpose):
1. Go to [Copilot Studio](https://copilotstudio.microsoft.com)
2. Click **Create** > **New agent**
3. Name it `Triage Agent`
4. Set the description to: "Analyzes new GitHub issues and suggests appropriate labels based on title, body, and team structure"
5. Click **Create**

### Step 2: Add the Trigger

1. Open your **Triage Agent**
2. Go to **Topics** in the left sidebar
3. Click **+ Add** > **Topic** > **From blank**
4. Name the topic `Triage New Issue`
5. For the trigger, select **When a flow calls this topic**

### Step 3: Configure the Topic

Add a **Message** node with the following instructions:

> You will receive a JSON object with `title`, `body`, `author`, and `existingLabels` (any labels already applied by an issue template).
>
> Analyze the title and body to suggest labels from these categories:
>
> **Owner** (who should own this):
> - `owner:[events-lead]` -- Events, partner execution, logistics, venues, catering, Build/Ignite planning
> - `owner:[gtm-lead]` -- Content (blogs, social, case studies), GTM systems, dashboards, competitive intel
> - `owner:[hack-lead]` -- Hackathons (NVIDIA, X-Org, AI Dev Days, AI Show 2.0), labs, badges
> - `owner:[ops-lead]` -- Budget, POs, vendor management, SOWs, finance tracking
> - `owner:aaron` -- Strategy, leadership decisions, cross-team coordination, stakeholder management
>
> **Function** (what type of work):
> - `func:events` -- Event planning and execution
> - `func:hackathon` -- Hackathon programs
> - `func:content` -- Written content, blogs, social media
> - `func:gtm` -- Go-to-market systems and processes
> - `func:data` -- Dashboards, analytics, reporting
> - `func:partnership` -- Partner relationships (NVIDIA, etc.)
> - `func:ops` -- Operations, processes, budget
>
> **Solution Play**:
> - `sp:ai-apps-agents` -- AI applications, agents, copilots, Azure AI services
> - `sp:data-platform` -- Data, analytics, databases
> - `sp:migration-modernization` -- Infrastructure, migration
> - `sp:cross-solution` -- Spans multiple solution plays
>
> **Priority**:
> - `P0` -- Urgent, blocking other work, executive visibility, or hard deadline within 48 hours
> - `P1` -- Important, has a deadline within 2 weeks, or is on the critical path for an initiative
> - `P2` -- Standard work, no immediate deadline pressure
>
> **Event** (if applicable):
> - `event:build-2026`, `event:ignite-2026`, `event:nvidia-hack`, `event:agent-fest`, `event:8k-hack`, `event:ai-dev-days`, `event:ai-show`
>
> **Type**:
> - `type:initiative` -- Epic-level work with sub-tasks
> - `type:task` -- Single deliverable
> - `type:blocker` -- Something preventing progress
> - `type:decision-needed` -- Requires a decision from someone
> - `type:recurring` -- Repeating work
>
> For each suggestion, provide:
> - The label
> - Confidence: High (obvious match), Medium (reasonable inference), Low (best guess)
> - Reason: One sentence explaining why
>
> Do NOT suggest labels that are already applied (check `existingLabels`).
> If the issue is too vague to classify, say so and suggest the author add more detail.

Add an **Output** variable called `triageComment` of type Text.

### Step 4: Connect the Flow

The topic receives the issue data and returns the formatted triage comment.

## Power Automate Flow Design

### Flow Name

`Triage Agent - Comment - New Issue Auto-Triage`

### Trigger

**When a new issue is created** (GitHub connector)
- Connection: Your GitHub account
- Repository owner: Your org or username
- Repository name: Your repository name

If the built-in GitHub connector trigger is not available or not reliable, use this alternative:

**GitHub Webhook (HTTP trigger)**
- Set up a webhook in your GitHub repo settings pointing to the flow's HTTP trigger URL
- Event: `issues` with action `opened`
- The flow receives the full webhook payload including issue title, body, labels, and author

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `issueTitle` | String | Title of the new issue (from trigger payload) |
| `issueBody` | String | Body/description of the issue (from trigger payload) |
| `issueNumber` | Integer | Issue number (for posting the comment back) |
| `issueAuthor` | String | GitHub username of the issue creator |
| `existingLabels` | Array | Any labels already applied (from issue templates) |
| `GitHubPAT` | String (env variable) | GitHub personal access token |
| `GitHubOwner` | String (env variable) | GitHub org or username |
| `GitHubRepo` | String (env variable) | Repository name |

### Actions (Step by Step)

1. **Compose -- Extract issue data from trigger**
   - Connector: Data Operation > Compose
   - Input:
     ```json
     {
       "title": "@{triggerOutputs()?['body/title']}",
       "body": "@{triggerOutputs()?['body/body']}",
       "author": "@{triggerOutputs()?['body/user/login']}",
       "existingLabels": "@{triggerOutputs()?['body/labels']}"
     }
     ```

2. **Condition -- Skip if issue was created by a bot**
   - Connector: Control > Condition
   - Condition: `author` does not end with `[bot]` AND `author` is not equal to `github-actions`
   - If No: Terminate (do not triage bot-created issues)

3. **Condition -- Skip if issue already has 3+ labels**
   - Connector: Control > Condition
   - Condition: `length(existingLabels)` is less than 3
   - If No: Terminate (issue was likely created from a template with pre-applied labels; triage is less needed)
   - Note: Adjust this threshold based on your workflow. Set to 0 if you want triage on every issue regardless.

4. **Copilot Studio -- Run Triage New Issue topic**
   - Connector: Microsoft Copilot Studio
   - Action: Run a flow-connected topic
   - Agent: Triage Agent
   - Topic: Triage New Issue
   - Input: the composed payload from step 1

5. **HTTP -- Post comment on the GitHub issue**
   - Connector: HTTP
   - Method: POST
   - URI: `https://api.github.com/repos/@{variables('GitHubOwner')}/@{variables('GitHubRepo')}/issues/@{triggerOutputs()?['body/number']}/comments`
   - Headers: `Authorization: Bearer @{variables('GitHubPAT')}`, `Accept: application/vnd.github+json`
   - Body:
     ```json
     {
       "body": "@{outputs('Run_Triage_New_Issue_topic')?['triageComment']}"
     }
     ```

6. **(Optional) HTTP -- Apply labels if "apply" reply detected**
   - This is a separate flow triggered by issue comment events. If a comment on the issue says "apply" from an authorized user, parse the triage comment for suggested labels and apply them via:
   - Method: POST
   - URI: `https://api.github.com/repos/@{variables('GitHubOwner')}/@{variables('GitHubRepo')}/issues/@{issueNumber}/labels`
   - Body: `{"labels": ["owner:[gtm-lead]", "func:content", ...]}`

### Output

A comment posted on the new GitHub issue with label suggestions.

## Customization

| Setting | Default | How to Change |
|---------|---------|---------------|
| Skip bot issues | Yes | Remove or edit the Condition in step 2 |
| Skip pre-labeled issues | Yes (3+ labels) | Change the threshold in step 3, or remove the condition entirely |
| Label vocabulary | Full set listed in topic instructions | Edit the Copilot Studio topic message node to add/remove label options |
| Confidence display | High/Medium/Low | Edit the topic instructions to use percentages or remove confidence |
| Auto-apply on "apply" reply | Optional (step 6) | Build the companion flow described in step 6 |
| Comment signature | "Triage Agent" | Edit the sign-off text in the topic instructions |

## Gotchas

- The GitHub connector's "When a new issue is created" trigger can have a 1-5 minute delay. If near-instant triage is important, use the webhook approach (HTTP trigger) instead.
- GitHub webhooks require your Power Automate flow to have a publicly accessible HTTP endpoint. Premium license required for the "When an HTTP request is received" trigger.
- Issue bodies can be very long (especially from templates with checklists). Truncate the body to 2000 characters before sending to Copilot Studio to avoid hitting input limits.
- The triage agent's accuracy depends entirely on the quality of the instructions in the topic. If it consistently miscategorizes a certain type of issue, update the owner/function descriptions with more specific keywords.
- Watch for issues created by GitHub Actions or bots (e.g., dependabot, automated release notes). The bot-skip condition in step 2 catches most of these, but add additional author names to the exclusion list as needed.
- The "apply" reply flow (step 6) needs careful permissions. Only allow authorized users (team members) to trigger label application. Check the comment author against an allowed list before applying labels.
- If your repo uses issue templates with default labels, many issues will arrive pre-labeled. The skip condition in step 3 prevents unnecessary triage comments, but tune the threshold based on how many labels your templates apply.
