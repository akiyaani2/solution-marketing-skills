# Agent Creation Rules — Solution Marketing Skills

**Version:** 1.0
**Scope:** This repo and Copilot Studio deployments for your org (80+ people)
**Owner:** Team lead / CoS

These rules are for **Copilot Studio agents deployed to Microsoft Teams** for a corporate marketing organization.

---

## Core Principles

1. **Role names, not codenames.** People in Teams see the agent name. "Marketing Assistant" means something. "FORGE" doesn't.
2. **Two tiers only.** Teams-facing agents (what people interact with) and repo-local agents (what maintains the system). No middle layers.
3. **Skills are the building blocks.** Agents are containers for skills. Don't create a new agent when adding a skill to an existing agent works.
4. **Flows are the hands.** Skills tell the agent what to do. Power Automate flows let it actually do things (post to Teams, write to Excel, send email, create calendar events).
5. **Less is more.** 4 Teams agents for 80 people is better than 15. People won't remember which agent to message if there are too many.

---

## Naming Convention

### Teams-Facing Agents

Use clear, descriptive names. Think: "What would someone type in Teams search to find this?"

| Pattern | Example | Anti-Pattern |
|---------|---------|-------------|
| `[Function] + [Role Type]` | Marketing Assistant | FORGE |
| Describes what it does | Program Tracker | NEXUS-OPS |
| Sounds like a person you'd ask for help | Strategy Advisor | RED-TEAM-COMPASS |
| Short enough for Teams search | Reporting Engine | Head of Reporting & Business Intelligence Analytics |

**Rules:**
- Max 3 words
- No codenames, acronyms, or internal jargon
- Must make sense to someone who's never seen this repo
- Must describe what the agent DOES, not what it IS

### Repo-Local Agents

Use `[Function] + [Level]` pattern:

| Pattern | Example |
|---------|---------|
| `Content Writer` | Writes content (blogs, abstracts, social) |
| `Research Analyst` | Executes research tasks |
| `Events Coordinator` | Manages event logistics |
| `Budget Tracker` | Tracks PO and MDF status |

**Rules:**
- Plain English role titles
- No L6/L7/L8 levels in the name (that's internal)
- Avoid "Manager" unless it actually manages other agents

### CoS (Special Case)

- **In this repo / GitHub:** CoS (Chief of Staff)
- **In Teams:** Not deployed (runs locally in Claude Code)

---

## Agent Tiers

### Tier 1: Teams-Facing (Deployed to Copilot Studio)

These agents are visible to the entire org. They must be:
- Simple to use (natural language, no slash commands)
- Reliable (tested with real queries before deployment)
- Well-scoped (each handles a clear domain)
- Maintained (skills updated, flows working)

| Agent | Domain | Audience | Skills Loaded |
|-------|--------|----------|--------------|
| **Marketing Assistant** | Decks, emails, summaries, data viz, meeting prep | Everyone (80+) | 8 Corporate Productivity skills |
| **Strategy Advisor** | Competitive intel, pressure testing, red teaming, OKR alignment | Leads and managers | 5 Devil's Advocate + Strategic skills |
| **Program Tracker** | Events, hackathons, budgets, countdowns, post-mortems | Event/hack owners | 8 Data Insights + Analytics skills |
| **Reporting Engine** | Standups, weekly status, MBR, 1:1 prep, meeting notes | Team leads | 8 Writing & Meeting Prep skills |

### Tier 2: Repo-Local (Never Deployed to Teams)

These agents maintain the system. They exist as AGENT.md reference docs for:
- Agent registry integration (portfolio management)
- Claude Code agent definitions (local execution)
- Documentation of how the system works

| Agent | Role | Why Not in Teams |
|-------|------|-----------------|
| **CoS** | Orchestrates everything | Runs as Claude Code locally, not a chatbot |
| **Cross-Portfolio Liaison** | Firewall enforcement, cross-group comms | Confidential data — can't go through shared platform |
| **Platform Lead** | Builds and maintains Copilot Studio agents | Meta-agent — builds the system, not a user-facing tool |
| **Budget Analyst** | Budget/vendor deep-dive | Merged into Program Tracker for Teams; standalone only for specialized local use |

---

## When to Create a New Agent vs Add a Skill

```
                  Does it need a new Teams presence?
                           │
                    ┌──────┴──────┐
                   YES            NO
                    │              │
            Create new         Does it fit an
            Tier 1 agent       existing agent's domain?
                                   │
                            ┌──────┴──────┐
                           YES            NO
                            │              │
                     Add skill to      Create new
                     existing agent    Tier 2 (repo-local)
```

**Create a new Tier 1 agent when:**
- The domain is clearly different from all existing agents
- The audience is different (e.g., partners vs internal team)
- Mixing it into an existing agent would confuse users

**Add a skill to an existing agent when:**
- The new capability fits the agent's existing domain
- The same audience would use it
- It doesn't make the agent's instructions too long (>8000 chars)

**Create a Tier 2 (repo-local) agent when:**
- The work is internal system maintenance
- It touches sensitive data (firewall, budget details, employment info)
- It orchestrates other agents rather than serving users directly

---

## Required Sections in AGENT.md

Every agent reference doc must include:

```yaml
---
name: [agent-name]
title: [role title]
description: [what it does and when to use it — for the AI model]
tier: [teams-facing | repo-local]
copilot-studio-name: [name in Teams, or "N/A" for repo-local]
skills: [array of skill names]
---
```

### For Tier 1 (Teams-Facing) — Required Sections:

1. **Identity** — Role, scope, audience
2. **Copilot Studio Build Guide** — Step-by-step creation instructions
3. **Knowledge Sources** — Which SKILL.md files to upload
4. **Agent Instructions** — Copy-paste instructions text
5. **Tools & Agent Flows** — Power Automate flows with specs
6. **MCP Servers** — Which to connect
7. **Triggers** — Autonomous schedules (if any)
8. **Gotchas** — What to watch for

### For Tier 2 (Repo-Local) — Required Sections:

1. **Identity** — Role, scope, why repo-local
2. **Skills** — What it owns
3. **How It Operates** — Claude Code, agent registry, or reference-only
4. **Gotchas** — What to watch for

---

## Skill-to-Agent Mapping

Every skill must be assigned to exactly one Tier 1 agent (for Teams deployment) even if the underlying work is done by a Tier 2 agent locally.

| Skill Category | Tier 1 Agent | Tier 2 Agent (if different) |
|---------------|-------------|---------------------------|
| Corporate Productivity (8) | Marketing Assistant | — |
| Competitive & Market (4) | Strategy Advisor | Research Analyst |
| Devil's Advocate (3) | Strategy Advisor | — |
| Strategic Thinking (3) | Strategy Advisor | — |
| Data Insights & Analytics (8) | Program Tracker | Events Coordinator, Hackathon Coordinator |
| Writing & Meeting Prep (8) | Reporting Engine | Report Scheduler, Meeting Prep Analyst |

---

## Flow Naming Convention

Power Automate flows follow this pattern: `[Agent] - [Action] - [Target]`

Examples:
- `Reporting Engine - Post to Teams - Daily Standup`
- `Marketing Assistant - Send Email - Draft Review`
- `Program Tracker - Log to Excel - Event Status`
- `Program Tracker - Update SharePoint List - Budget Entry`

---

## Agent Lifecycle

### Creating
1. Write the AGENT.md reference doc (follow the template above)
2. Map skills to the agent
3. Build in Copilot Studio following the Build Guide
4. Test with 5-10 real queries
5. Deploy to Teams
6. Announce to the team

### Updating
1. Upload new/updated SKILL.md files to Knowledge
2. Update agent instructions if needed
3. Test the changes
4. Publish the update

### Retiring
1. Remove from Teams app catalog
2. Move AGENT.md to `/archive/agents/`
3. Reassign skills to another agent or archive them
4. Notify the team

---

## What These Rules Are NOT

- NOT a governance framework for a full multi-agent portfolio
- NOT rules for building agents in Foundry or the Agent SDK (those have their own patterns)

These rules are specifically for **Copilot Studio agents serving a Microsoft marketing org via Teams.**

---

*Maintained by the team.*
