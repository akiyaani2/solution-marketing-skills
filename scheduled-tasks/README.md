# Scheduled Tasks Library

Pre-built automation recipes for Copilot Studio + Power Automate. Each task runs on a schedule or event trigger, and posts results to Microsoft Teams or saves to SharePoint.

## Operating Rhythm

| Cadence | Task | Output |
|---------|------|--------|
| Daily 9am | [Morning Pulse](daily-morning-pulse.md) | Teams channel post |
| Weekly Monday | [Board Health Check](weekly-board-health.md) | Teams channel post |
| Weekly Friday 2pm | [Friday Digest](weekly-friday-digest.md) | Teams channel post |
| Monthly 8th | [MBR Data Package](monthly-mbr-package.md) | SharePoint document |
| On new issue | [Auto-Triage](new-issue-auto-triage.md) | GitHub issue comment |

## How Scheduled Tasks Work

Each task in this library combines two components:

1. **Copilot Studio Agent** -- An agent (or topic within an agent) that holds the logic for what to analyze and how to format the output. The agent receives structured data, applies rules, and generates human-readable summaries.

2. **Power Automate Flow** -- A cloud flow that handles the plumbing: triggering on a schedule or event, calling APIs (GitHub, SharePoint, etc.), passing data to the Copilot Studio agent, and delivering the final output to Teams or SharePoint.

The general pattern is:

```
Trigger (schedule or event)
    -> Power Automate pulls raw data from GitHub / SharePoint
    -> Power Automate sends data to Copilot Studio agent
    -> Agent formats and summarizes the data
    -> Power Automate posts the result to Teams or saves to SharePoint
```

Some simpler tasks skip the agent step entirely and use Power Automate expressions to format output directly.

## Prerequisites

Before setting up any of these tasks, ensure you have:

- **Copilot Studio access** -- Licensed through your Microsoft 365 plan. You need permissions to create and publish agents.
- **Power Automate Premium** -- Required for the HTTP connector (used to call GitHub API) and Copilot Studio connector. Standard connectors (Teams, SharePoint, Outlook) are included in most M365 plans.
- **GitHub Personal Access Token (PAT)** -- A fine-grained token with read access to Issues, Projects, and Labels for your repository. Store this in a Power Automate environment variable, not hardcoded in flows.
- **Microsoft Teams channel** -- A dedicated channel (e.g., "Automation Updates" or "Team Pulse") where scheduled posts will land.
- **SharePoint document library** -- A folder for storing generated reports (used by the MBR package task).
- **GitHub connector (optional)** -- Power Automate has a built-in GitHub connector for some operations. For advanced queries (GraphQL, project board fields), you will use the HTTP connector with your PAT instead.

## Environment Variables

Set these once in Power Automate and reference them across all flows:

| Variable | Value | Used By |
|----------|-------|---------|
| `GitHubPAT` | Your personal access token | All flows |
| `GitHubOwner` | Your GitHub org or username | All flows |
| `GitHubRepo` | Repository name | All flows |
| `TeamsChannelId` | Target channel for posts | Daily, Weekly, Friday flows |
| `TeamsTeamId` | Target team for posts | Daily, Weekly, Friday flows |
| `SharePointSiteUrl` | Site for document storage | MBR package flow |
| `SharePointFolderPath` | Folder within the document library | MBR package flow |

## Naming Conventions

All Power Automate flows follow this pattern:

```
[Agent Name] - [Action] - [Description]
```

Examples:
- `Reporting Engine - Post - Daily Morning Pulse`
- `Reporting Engine - Post - Weekly Board Health`
- `Program Tracker - Triage - New Issue Auto-Label`
