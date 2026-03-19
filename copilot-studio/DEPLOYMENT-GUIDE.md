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
│  "What's the budget status for NVIDIA?"                  │
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
    │ SharePoint│   │ Post to    │   │ GitHub    │
    │ Websites │    │ Teams      │   │ Dataverse │
    │ Dataverse│    │ Create     │   │ Power BI  │
    │          │    │ calendar   │   │           │
    └──────────┘    └───────────┘   └───────────┘
```

## Phase 1: Quick Deploy (This Week)

### Step 1: Create the Main Agent

1. Open [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Select **Agents** > **New Agent**
3. Configure:
   - **Name:** "Solution Marketing Assistant"
   - **Description:** "AI assistant for the Solution Marketing team — builds decks, preps meetings, analyzes data, drafts emails, and challenges your plans."
   - **Icon:** Upload a team-branded icon
   - **Instructions:** See [agent-instructions.md](agent-instructions.md)

### Step 2: Upload Skills as Knowledge

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

### Step 3: Deploy to Teams

1. Go to **Channels** > **Microsoft Teams**
2. Select **Publish** > **Deploy to Teams**
3. Share the agent with your team via Teams app catalog or direct link

### Step 4: Share With Your Team

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
| **Program Tracker** | Events, hackathons, budgets, countdowns | event-ops, hackathon-tracker, event-countdown, budget-ops, post-mortem | Event/hack owners |
| **Strategy Advisor** | Pressure-test, pre-mortem, competitive scan, OKR check | pressure-test, pre-mortem, red-team, competitive-scan, okr-check | Leads and managers |
| **Reporting Engine** | Standup, weekly status, MBR, peer digest, epic health | standup, weekly-status, mbr, peer-digest, epic-health, 1on1-prep | Team leads |

See the `/agents/` folder for full configuration docs per agent.

---

## Phase 3: Add Automation (Month 2)

### Agent Flows (Power Automate)

Agent Flows let your Copilot Studio agent take real actions — not just generate text.

| Flow | What It Does | Trigger | Agent |
|------|-------------|---------|-------|
| **Log to Excel** | Writes a row to a SharePoint Excel file | Agent calls after generating a report | Reporting Engine |
| **Post to Teams Channel** | Posts formatted message to a Teams channel | Scheduled (daily standup, weekly digest) | Reporting Engine |
| **Create Calendar Event** | Creates an Outlook calendar event with attendees | Agent calls after meeting prep | Marketing Assistant |
| **Create SharePoint List Item** | Logs a decision, escalation, or action item | Agent calls after meeting notes | Marketing Assistant |
| **Send Email** | Sends a formatted email via Outlook | Agent calls after email drafting | Marketing Assistant |
| **Update Excel Tracker** | Updates a row in a tracking spreadsheet | Agent calls after status update | Program Tracker |

### Autonomous Triggers

These make agents run WITHOUT someone messaging them.

| Trigger | Agent | What Happens |
|---------|-------|-------------|
| **Daily 9am** | Reporting Engine | Generates standup, posts to Teams channel |
| **Every Friday 2pm** | Reporting Engine | Generates weekly digest, posts to Teams channel |
| **Monthly 8th** | Reporting Engine | Generates MBR draft, saves to SharePoint |
| **New email from NVIDIA** | Program Tracker | Flags and summarizes for Aaron |
| **New SharePoint file in /events/** | Program Tracker | Analyzes and logs to event tracker |

### MCP Servers

Connect these in Copilot Studio for live data access:

| MCP Server | What It Gives the Agent | Setup |
|-----------|------------------------|-------|
| **Microsoft Learn** | Official Azure docs for content accuracy | Add via MCP wizard in Copilot Studio |
| **Work IQ** | Teams, Calendar, Mail, SharePoint, Word | Enabled by M365 Copilot license |
| **Dataverse** | Business data, CRM records, program metrics | Connect via Dataverse connector |
| **Power BI** | Dashboard data, semantic model queries | Connect via Power BI connector |

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
| Agent responds generically | Add more specific instructions that reference the knowledge source by name: "When asked to build a deck, follow the deck-builder knowledge source." |
| Agent can't access live data | Connect MCP servers or Power Automate flows. Skills alone are instructions — they need tools for data. |
| Agent triggers too often (autonomous) | Add conditions to the trigger: only run on weekdays, only when specific keywords appear. |
| Team can't find the agent in Teams | Check that the agent is published AND deployed to the Teams app catalog. Share the direct link. |
