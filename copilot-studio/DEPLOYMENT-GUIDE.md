# Copilot Studio Deployment Guide

How to deploy the Solution Marketing Skills as Copilot Studio agents for your team. No GitHub or CLI required — everything runs in Teams.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    MICROSOFT TEAMS                        │
│                                                           │
│  Your team messages agents in Teams chat.                │
│  Agents respond using skills + live data.                │
│                                                           │
│  "Build me a deck on our hackathon results"              │
│  "What's the budget status for our partner program?"     │
│  "Pressure-test my Build 2026 plan"                      │
│                                                           │
└────────────────────────┬──────────────────────────────────┘
                         │
              ┌──────────┴──────────┐
              │   COPILOT STUDIO     │
              │                      │
              │  8 Agents            │
              │  33 Skills (knowledge│
              │  MCP Servers         │
              │  Agent Flows         │
              │  Triggers            │
              └──────────┬──────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    ┌────┴────┐    ┌─────┴─────┐   ┌─────┴─────┐
    │ Knowledge│    │   Tools    │   │ MCP       │
    │ Sources  │    │ (Flows)    │   │ Servers   │
    │          │    │            │   │           │
    │ SKILL.md │    │ Log to     │   │ MS Learn  │
    │ files    │    │ Excel      │   │ Work IQ   │
    │ SharePoint│   │ Post to    │   │ Microsoft │
    │ Websites │    │ Teams      │   │ Graph     │
    │ Dataverse│    │ Create     │   │ Power BI  │
    │          │    │ calendar   │   │ (GitHub:  │
    └──────────┘    └───────────┘   │ optional) │
                                    └───────────┘
```

> **Data sources:** This system is M365-first. All agent skills pull from Planner, SharePoint Lists, Loop tables, Teams channels, Excel, and Power BI. GitHub is optional — only connect it if your team actively uses GitHub Issues for work tracking.

---

## Phase 1: Quick Deploy (This Week)

### Step 1: Set Up Your SharePoint Initiative Tracker

Before deploying agents, create the SharePoint List that most agents will read from:

1. Go to your team's SharePoint site
2. Create a new **Microsoft List** named "Initiative Tracker" (or your preferred name)
3. Add these columns:

| Column | Type |
|--------|------|
| Title | Single line text (default) |
| Owner | Person |
| Priority | Choice: P0, P1, P2, P3 |
| Motion | Choice: [your team's program motions] |
| Status | Choice: Not Started, In Progress, At Risk, Blocked, Complete |
| Target_Date | Date |
| Progress | Number (0–100) |
| Obstacles | Multi-line text |
| Notes | Multi-line text |

4. Pin the list to your SharePoint page or Teams tab

> This list is the single source of truth that powers standups, weekly status, MBR, initiative health, and triage automation.

### Step 2: Create the Main Agent

1. Open [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Select **Agents** > **New Agent**
3. Configure:
   - **Name:** "Solution Marketing Assistant"
   - **Description:** "AI assistant for the Solution Marketing team — builds decks, preps meetings, analyzes data, drafts emails, and challenges your plans."
   - **Icon:** Upload a team-branded icon
   - **Instructions:** See [agent-instructions.md](agent-instructions.md)

### Step 3: Upload Skills as Knowledge

1. Go to **Knowledge** > **Add Knowledge** > **Upload Files**
2. Upload these 8 SKILL.md files first (Corporate Productivity):
   - `deck-builder/SKILL.md`
   - `exec-summary/SKILL.md`
   - `email-drafter/SKILL.md`
   - `one-pager/SKILL.md`
   - `talking-points/SKILL.md`
   - `excel-analyzer/SKILL.md`
   - `stakeholder-brief/SKILL.md`
   - `data-to-slides/SKILL.md`
3. Wait for status to show **Ready** for each file
4. Test in the test pane: "Build me a stakeholder briefing deck"

### Step 4: Deploy to Teams

1. Go to **Channels** > **Microsoft Teams**
2. Select **Publish** > **Deploy to Teams**
3. Share the agent with your team via Teams app catalog or direct link

### Step 5: Share With Your Team

Send this message to your team in Teams:

> We have a new AI assistant in Teams called "Solution Marketing Assistant." You can message it to:
> - "Build me slides for [meeting/event]"
> - "Summarize this for [leader name]"
> - "Draft an email to [recipient] about [topic]"
> - "Prep me for my meeting with [name]"
> - "Analyze this data" (paste a table or attach an Excel file)
> - "Pressure-test this plan" (paste your plan)
>
> Try it and let me know what works and what doesn't.

---

## Phase 2: Add Specialist Agents (Next 2 Weeks)

Instead of one big agent, deploy specialized agents that your team can message by name in Teams.

| Agent | What It Does | Skills (Knowledge) | Who Uses It |
|-------|-------------|-------------------|-------------|
| **Marketing Assistant** | Corporate productivity — decks, emails, summaries | 8 Corporate Productivity skills | Everyone (80+) |
| **Program Tracker** | Events, hackathons, budgets, initiative health | event-ops, hackathon-tracker, event-countdown, budget-ops, post-mortem, epic-health | Event/hack owners |
| **Strategy Advisor** | Pressure-test, pre-mortem, competitive scan, OKR check | pressure-test, pre-mortem, red-team, competitive-scan, okr-check | Leads and managers |
| **Reporting Engine** | Standup, weekly status, MBR, peer digest, 1:1 prep | standup, weekly-status, mbr, peer-digest, 1on1-prep, escalation-tracker | Team leads |

See the `/agents/` folder for full configuration docs per agent.

---

## Phase 3: Add Automation (Month 2)

### Agent Flows (Power Automate)

Agent Flows let your Copilot Studio agent take real actions — not just generate text.

| Flow | What It Does | Trigger | Agent |
|------|-------------|---------|-------|
| **Log to Excel** | Writes a row to a SharePoint Excel file | Agent calls after generating a report | Reporting Engine |
| **Post to Teams Channel** | Posts HTML-formatted message to a Teams channel | Scheduled (daily standup, weekly digest) | Reporting Engine |
| **Create Calendar Event** | Creates an Outlook calendar event with attendees | Agent calls after meeting prep | Marketing Assistant |
| **Create SharePoint List Item** | Logs a decision, escalation, or action item | Agent calls after meeting notes | Marketing Assistant |
| **Send Email** | Sends a formatted email via Outlook | Agent calls after email drafting | Marketing Assistant |
| **Update SharePoint List** | Updates initiative status, blockers, or progress | Agent calls after status update | Program Tracker |

> ⚠️ **Teams formatting gotcha:** When posting to Teams via Power Automate, use HTML-formatted content — plain text loses line breaks and collapses in Teams channel posts. Always set content type to HTML and use `<b>`, `<br>`, and `<a href>` tags.

### Autonomous Triggers

These make agents run WITHOUT someone messaging them.

| Trigger | Agent | What Happens |
|---------|-------|-------------|
| **Daily 9am weekdays** | Reporting Engine | Generates standup from Planner/SharePoint, posts to Teams channel |
| **Every Friday 2pm** | Reporting Engine | Generates weekly digest, posts to Teams + sends email to team DL |
| **Monthly 8th** | Reporting Engine | Generates MBR scaffold, flags metric gaps, saves to SharePoint |
| **New SharePoint List item** | Program Tracker | Triage check — flags missing owner, priority, target date |
| **Monday 8am** | Program Tracker | Weekly tracker health check — stale, overdue, missing owners |

### MCP Servers

Connect these in Copilot Studio for live data access:

| MCP Server | What It Gives the Agent | Required? |
|-----------|------------------------|----------|
| **Microsoft Graph** | Planner tasks, SharePoint Lists, calendar, Teams messages | Required for Reporting Engine + Program Tracker |
| **Work IQ** | Teams, Calendar, Mail, SharePoint, Word integration | Recommended — enabled by M365 Copilot license |
| **Microsoft Learn** | Official Azure docs for content accuracy | Recommended for Strategy Advisor |
| **Power BI** | Dashboard data, semantic model queries | Recommended for MBR metrics |
| **Dataverse** | Business data, CRM records, program metrics | Optional |
| **GitHub** | Issue data if team uses GitHub for work tracking | Optional |

> ⚠️ **SharePoint List as knowledge source:** Using SharePoint Lists directly as a Copilot Studio knowledge source is a staged rollout feature. If not yet available in your tenant, use the **recommended fallback**: Power Automate generates a SharePoint document from list data, and the agent grounds on the document. See `scheduled-tasks/` for examples of this pattern.

---

## How to Add a New Skill to an Existing Agent

1. Open the agent in Copilot Studio
2. Go to **Knowledge** > **Add Knowledge** > **Upload Files**
3. Upload the new SKILL.md file
4. Wait for **Ready** status
5. Test in the test pane
6. Publish to update the live agent

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Agent doesn't follow skill instructions | Check that the SKILL.md was uploaded correctly. Try uploading as a .txt file if .md isn't recognized. |
| Agent responds generically | Add more specific instructions that reference the knowledge source by name: "When asked to generate a standup, follow the standup knowledge source." |
| Agent can't access live data | Connect MCP servers (Microsoft Graph minimum) or Power Automate flows. Skills alone are instructions — they need tools for data. |
| Teams posts lose line breaks | Switch from plain text to HTML in Power Automate. Use `<br>` for line breaks and `<b>` for headers. |
| SharePoint List not available as knowledge | Use the fallback: Power Automate writes list data to a SharePoint document. Ground the agent on the document instead. |
| Agent triggers too often (autonomous) | Add weekday conditions to the trigger. Add a check for whether the previous run already completed. |
| Team can't find the agent in Teams | Check that the agent is published AND deployed to the Teams app catalog. Share the direct link. |
