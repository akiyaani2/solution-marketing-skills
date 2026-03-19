# Solution Marketing Skills

**23 production-ready AI skills for marketing teams that run on GitHub.**

Built from real operations managing 175+ issues, 30+ epics, 5 concurrent programs, monthly MBRs, and cross-solution coordination. These skills work with **Claude Code**, **GitHub Copilot**, **Cursor**, **Codex CLI**, and any platform supporting the [Agent Skills](https://agentskills.io) open standard (26+ platforms).

---

## Table of Contents

- [Quick Start](#quick-start)
- [Skills by Category](#skills-by-category)
  - [Competitive & Market Insights](#1-competitive--market-insights)
  - [Writing & Meeting Prep](#2-writing--meeting-prep)
  - [Data Insights & Analytics](#3-data-insights--analytics)
  - [Devil's Advocate](#4-devils-advocate)
  - [Strategic Thinking & OKR Alignment](#5-strategic-thinking--okr-alignment)
- [How Skills Work Together](#how-skills-work-together)
- [MCP Servers](#mcp-servers)
- [Using Skills with Copilot](#using-skills-with-copilot)
- [Using Skills with Copilot Studio Agents](#using-skills-with-copilot-studio-agents)
- [The Agent Skills Standard](#the-agent-skills-standard)
- [Setting Up Your Repo](#setting-up-your-repo)
- [Customization Guide](#customization-guide)
- [FAQ](#faq)

---

## Quick Start

```bash
# Option 1: Copy all skills to your repo
git clone https://github.com/akiyaani2/solution-marketing-skills.git
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/

# Option 2: Copy to .github/skills/ for Copilot
cp -r solution-marketing-skills/.claude/skills/* your-repo/.github/skills/

# Option 3: Add as submodule
git submodule add https://github.com/akiyaani2/solution-marketing-skills.git .claude/shared-skills
```

Start with 3 skills: [`/standup`](.claude/skills/standup/) + [`/epic-health`](.claude/skills/epic-health/) + [`/pressure-test`](.claude/skills/pressure-test/). Add more as your workflow matures.

---

## Skills by Category

### 1. Competitive & Market Insights

*"What's happening out there?"*

Skills for understanding the competitive landscape, tracking market signals, and grounding decisions in real intelligence.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**competitive-scan**](.claude/skills/competitive-scan/) | `/competitive-scan` | Structured scanning of AWS, GCP, and partner ecosystems. Produces competitive briefs for leadership. |
| [**content-pipeline**](.claude/skills/content-pipeline/) | `/content-pipeline` | Track content production including competitive analysis content. Blog, video, social, and session pipeline. |
| [**cross-solution-sync**](.claude/skills/cross-solution-sync/) | `/x-sp` | Cross-team coordination that surfaces market context and shared intelligence across solution plays. |

**Best MCP servers for this category:**
- [Microsoft Learn MCP](#1-microsoft-learn-mcp) — ground competitive claims in official docs
- [Power BI MCP](#5-power-bi-modeling-mcp) — pull market dashboards and engagement data

---

### 2. Writing & Meeting Prep

*"Help me prepare and produce."*

Skills for meeting preparation, content drafting, report generation, and communication.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**meeting-notes**](.claude/skills/meeting-notes/) | `/meeting-notes` | Capture notes, auto-extract action items as GitHub Issues. Templates for 1:1s, syncs, and recurring insights sessions. |
| [**1on1-prep**](.claude/skills/1on1-prep/) | `/1on1-prep [name]` | 1:1 meeting prep with open issues, blockers, wins, and FIRE framework (Feedback, Ideas, Requests, Expectations). |
| [**mbr**](.claude/skills/mbr/) | `/mbr` | Monthly Business Review package. Executive summary, owner breakdowns, metrics, narrative template. |
| [**peer-digest**](.claude/skills/peer-digest/) | `/peer-digest` | Friday cross-team update. What shipped, what's coming, cross-team heads up. Under 10 lines. |
| [**standup**](.claude/skills/standup/) | `/standup` | Daily async standup from GitHub activity. Yesterday / today / blockers per person. |
| [**weekly-status**](.claude/skills/weekly-status/) | `/weekly-status [name]` | Weekly status per team member. Completed, in-progress, blocked, coming up. |
| [**escalation-tracker**](.claude/skills/escalation-tracker/) | `/escalations` | Track items escalated to leadership with SLA monitoring and response tracking. |
| [**git-sync**](.claude/skills/git-sync/) | `/git-sync` | Automated git pull / commit / push with standardized messages. |

**Best MCP servers for this category:**
- [GitHub MCP](#3-github-mcp-server) — pull issue context for meeting prep and reports
- [Work IQ MCP](#2-microsoft-work-iq-mcp) — calendar context, Teams posting, email distribution

---

### 3. Data Insights & Analytics

*"Show me the numbers."*

Skills for metrics, health scoring, trend analysis, and operational dashboards.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**epic-health**](.claude/skills/epic-health/) | `/epic-health` | Score epics on 5 health dimensions. Completion %, blocked count, staleness, RAPID, TBD count. |
| [**issue-triage**](.claude/skills/issue-triage/) | `/issue-triage` | Bulk triage with P0 downgrade cascade pattern. Stale detection, orphan reparenting, label hygiene. |
| [**solution-play-health**](.claude/skills/solution-play-health/) | `/sp-health` | Strategic health scoring for solution plays. Layer above epic-health. Quarter-over-quarter comparison. |
| [**hackathon-tracker**](.claude/skills/hackathon-tracker/) | `/hack-status` | Hackathon pipeline tracking. Registration funnels, judging status, cross-hack comparison, outcomes analysis. |
| [**event-countdown**](.claude/skills/event-countdown/) | `/event-countdown` | Event readiness dashboard. Three urgency tiers with readiness % and color coding. |
| [**event-ops**](.claude/skills/event-ops/) | `/event-ops` | Full event lifecycle. Workback schedules, vendor checklists, readiness scoring, cross-event calendar. |
| [**budget-ops**](.claude/skills/budget-ops/) | `/budget-ops` | PO tracking, co-marketing fund utilization, vendor management, budget reports for leadership. |

**Best MCP servers for this category:**
- [GitHub MCP](#3-github-mcp-server) — real-time issue and board data
- [Power BI MCP](#5-power-bi-modeling-mcp) — dashboard data, semantic model queries
- [Dataverse MCP](#4-dataverse-mcp) — business data, CRM records, program metrics

---

### 4. Devil's Advocate

*"Challenge my thinking."*

Skills that push back, find holes, and make your plans stronger before you commit. The category most teams don't have — and need most.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**pressure-test**](.claude/skills/pressure-test/) | `/pressure-test` | Challenge any plan through 7 lenses: assumptions, resources, timeline, stakeholders, competition, failure modes, opportunity cost. |
| [**pre-mortem**](.claude/skills/pre-mortem/) | `/pre-mortem` | "It failed. Why?" Reason backwards from failure to surface risks before they happen. |
| [**red-team**](.claude/skills/red-team/) | `/red-team` | Take the competitor's perspective. How would AWS or GCP counter your program? |

**Best MCP servers for this category:**
- [Microsoft Learn MCP](#1-microsoft-learn-mcp) — verify your own claims against official docs
- [GitHub MCP](#3-github-mcp-server) — pull actual data to check assumptions

---

### 5. Strategic Thinking & OKR Alignment

*"Am I working on the right things?"*

Skills for planning, goal alignment, decision governance, and making sure daily work connects to quarterly objectives.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**okr-check**](.claude/skills/okr-check/) | `/okr-check` | Score any work item against OKRs. Scan for orphaned work, uncover drift, check coverage of key results. |
| [**decision**](.claude/skills/decision/) | `/decision "desc"` | Create formatted decision records (TD-NNNN). Context, options, decision, RAPID roles, consequences. |
| [**solution-play-health**](.claude/skills/solution-play-health/) | `/sp-health` | Strategic health scoring. Also appears in Data Insights — it bridges analytics and strategy. |

**Best MCP servers for this category:**
- [GitHub MCP](#3-github-mcp-server) — pull issue data for alignment analysis
- [Dataverse MCP](#4-dataverse-mcp) — access OKR/goal data if stored in Dynamics/Dataverse

---

## How Skills Work Together

```
                ┌──────────────────────────────────────────────────┐
                │              YOUR OPERATING RHYTHM                │
                └──────────────────────────────────────────────────┘

   DAILY         /standup ──────────────────────► team pulse
                                                      │
   WEEKLY        /weekly-status ────────────────► per-person view
                 /issue-triage ─────────────────► board hygiene
                 /epic-health ──────────────────► initiative health
                 /x-sp ─────────────────────────► cross-team sync
                                                      │
   BIWEEKLY      /1on1-prep ────────────────────► meeting prep
                 /meeting-notes ────────────────► capture action items
                                                      │
   MONTHLY       /mbr ──────────────────────────► leadership package
                 /budget-ops ───────────────────► financial status
                 /sp-health ────────────────────► strategic view
                 /competitive-scan ─────────────► market intelligence
                                                      │
   FRIDAY        /peer-digest ──────────────────► cross-team update
                                                      │
   EVENTS        /event-ops ────────────────────► logistics
                 /hack-status ──────────────────► hackathon pipeline
                 /event-countdown ──────────────► readiness check
                                                      │
   BEFORE ANY    /pressure-test ────────────────► find the holes
   BIG DECISION  /pre-mortem ───────────────────► imagine failure
                 /red-team ─────────────────────► competitor view
                 /okr-check ────────────────────► alignment check
                                                      │
   AD HOC        /escalations ──────────────────► track upward items
                 /decision ─────────────────────► document decisions
                 /content-pipeline ─────────────► content status
```

**Key composition chains:**
- `/standup` → `/weekly-status` → `/mbr` (daily → weekly → monthly roll-up)
- `/epic-health` → `/sp-health` → `/okr-check` (tactical → strategic → alignment)
- `/1on1-prep` + `/meeting-notes` (prepare → capture → action items → issues)
- `/competitive-scan` → `/red-team` → `/pressure-test` (intelligence → adversarial thinking → plan hardening)
- `/event-countdown` → `/event-ops` → `/hack-status` (awareness → logistics → execution)

---

## MCP Servers

[Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers give AI agents real-time access to tools and data. These skills become dramatically more powerful when paired with the right MCP servers.

### Recommended Setup (Priority Order)

#### 1. Microsoft Learn MCP

**What:** Search and fetch official Microsoft/Azure documentation.
**Why first:** Grounds all research, competitive analysis, and content creation in accurate, official information. Prevents hallucinated product claims.

| Tool | What It Does |
|------|-------------|
| `microsoft_docs_search` | Search docs, return concise content chunks |
| `microsoft_code_sample_search` | Find working code examples |
| `microsoft_docs_fetch` | Fetch full page content from any Learn URL |

**Setup:**
```json
{
  "mcpServers": {
    "microsoft-learn": {
      "command": "npx",
      "args": ["-y", "@anthropic/microsoft-learn-mcp"]
    }
  }
}
```

**Skills it powers:** competitive-scan, content-pipeline, red-team, pressure-test

---

#### 2. Microsoft Work IQ MCP

**What:** Enterprise-grade MCP servers for Microsoft 365 — Teams, Calendar, Mail, SharePoint, OneDrive, Word.
**Why:** Connects your AI agent to the Microsoft ecosystem your team already uses.
**Requires:** M365 Copilot license. Currently in preview.

| Server | What It Does | Skills It Powers |
|--------|-------------|-----------------|
| Work IQ Calendar | Create/update events, suggest meeting times | meeting-notes, 1on1-prep, event-ops |
| Work IQ Teams | Post messages, create chats, channel ops | peer-digest, standup, escalation-tracker |
| Work IQ Mail | Send/search emails, reply-all | mbr (distribution), escalation-tracker |
| Work IQ SharePoint | Upload files, search, manage lists | budget-ops, content-pipeline |
| Work IQ Word | Create/read documents | mbr, meeting-notes, decision |

**Setup:** Managed via Microsoft 365 admin center or Copilot Studio. See [Work IQ documentation](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview).

---

#### 3. GitHub MCP Server

**What:** Full CRUD on GitHub Issues, PRs, Projects, Discussions, Actions, and code search.
**Why:** This is the data backbone. Every reporting and analytics skill pulls from GitHub.

| Capability | Skills It Powers |
|-----------|-----------------|
| Issue CRUD | issue-triage, epic-health, standup, weekly-status |
| Project board management | hackathon-tracker, event-ops, solution-play-health |
| Discussion access | meeting-notes, decision |
| Code search | competitive-scan (verify implementations) |

**Setup:**
```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "ghcr.io/github/github-mcp-server"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>" }
    }
  }
}
```

**Repo:** [github/github-mcp-server](https://github.com/github/github-mcp-server)

---

#### 4. Dataverse MCP

**What:** Chat over your business data — discover tables, run queries, retrieve/insert/update records, execute custom prompts grounded in business context.
**Why:** If your team tracks programs, customers, or OKRs in Dynamics 365 or Dataverse, this connects your AI skills directly to that data.

| Capability | Skills It Powers |
|-----------|-----------------|
| Query business records | budget-ops, okr-check, hackathon-tracker (if registration data is in Dataverse) |
| Discover tables | competitive-scan (partner data), event-ops (venue/logistics data) |
| Insert/update records | meeting-notes (sync action items to CRM) |

**Setup:** See [microsoft/Dataverse-MCP](https://github.com/microsoft/Dataverse-MCP) for authentication and configuration.

---

#### 5. Power BI Modeling MCP

**What:** Brings Power BI semantic modeling capabilities to AI agents. Query datasets, analyze models, generate DAX.
**Why:** If your team uses Power BI dashboards (engagement metrics, registration data, program performance), this gives your AI agent direct access.

| Capability | Skills It Powers |
|-----------|-----------------|
| Query semantic models | mbr (pull actual metrics), hackathon-tracker (registration data) |
| Analyze data models | solution-play-health (performance trends), budget-ops (spend analysis) |
| Generate DAX queries | Custom analytics on demand |

**Setup:** See [microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp) (519 stars, actively maintained).

---

### MCP + Skills Matrix

| MCP Server | Cat 1: Competitive | Cat 2: Writing | Cat 3: Analytics | Cat 4: Devil's Advocate | Cat 5: Strategy |
|-----------|-------------------|---------------|-----------------|----------------------|----------------|
| Microsoft Learn | Primary | Support | — | Verification | — |
| Work IQ | — | Primary | — | — | — |
| GitHub | Support | Primary | Primary | Data source | Data source |
| Dataverse | Support | — | Primary | — | Primary |
| Power BI | Support | — | Primary | — | Support |

---

## Using Skills with Copilot

These skills follow the [Agent Skills](https://agentskills.io) standard — the same format works across Claude Code and GitHub Copilot.

### Copilot Coding Agent (Autonomous)

1. Place skills in `.github/skills/` or `.claude/skills/`
2. Assign a GitHub Issue to Copilot (like assigning to a team member)
3. Copilot reads the relevant skill, pulls data via `gh` CLI, and opens a draft PR

**Example:** Create an issue "Run monthly epic health check" → assign to Copilot → it executes `/epic-health` and produces the report.

### Copilot Chat

```
@workspace Use the pressure-test skill to challenge our Build 2026 plan
```

### Copilot CLI

```bash
gh copilot suggest "use the standup skill to generate today's standup"
```

---

## Using Skills with Copilot Studio Agents

### Low-Code Setup

1. Create a new agent in **Copilot Studio**
2. Add this repo (or individual SKILL.md files) as **knowledge sources**
3. Map skills to **topics**: "Generate standup" triggers standup SKILL.md instructions
4. Connect **Power Automate flows** or **MCP connectors** for `gh` CLI execution
5. Deploy to **Microsoft Teams** — team members invoke via chat

### Pro-Code Setup (Microsoft Foundry)

Use the [Microsoft Agent Framework](https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/) to build agents that read SKILL.md files, use MCP servers for data, and run on Azure compute.

### Sharing Skills as Knowledge

- Upload SKILL.md files as **Copilot Studio knowledge articles**
- Add to **Copilot custom instructions** (`.github/copilot-instructions.md`)
- Embed in **Power Platform AI Builder** prompts
- Distribute through your org's **Copilot Extensions** catalog

---

## The Agent Skills Standard

These skills follow the [Agent Skills](https://agentskills.io) open specification by Anthropic. Adopted by **26+ platforms** including Claude Code, GitHub Copilot, Cursor, Codex CLI, Gemini CLI, VS Code, and more.

### Skill Structure

```
.claude/skills/my-skill/
  SKILL.md           # Required — YAML frontmatter + instructions
  scripts/           # Optional — helper scripts
  references/        # Optional — detailed docs
  assets/            # Optional — templates, samples
```

### SKILL.md Format

```yaml
---
name: my-skill
description: What this does and when to trigger it (for the AI, not humans)
---

# My Skill
Step-by-step instructions...
```

---

## Setting Up Your Repo

### Prerequisites

- GitHub repo with Issues enabled
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated
- An AI coding tool (Claude Code, Copilot, Cursor, etc.)

### Recommended Labels

```
Priority:     p0, p1, p2
Ownership:    owner:[name]
Function:     func:events, func:hackathon, func:content, func:gtm, func:ops
Solution:     sp:[play-name], sp:cross-solution
Status:       status:blocked, status:at-risk, status:waiting-external
Type:         type:initiative, type:task, type:decision-needed
Stakeholder:  stakeholder:[name]
Event:        event:[name]
```

### Project Board Structure

1. **Per-person boards** — one board per team member
2. **Leadership View** — initiative-level roll-up
3. **Events Calendar** — timeline visualization
4. **Cross-Solution Portfolio** — cross-team items

---

## Customization Guide

### Labels
```bash
# Replace label prefixes to match your repo
find .claude/skills -name "SKILL.md" -exec sed -i '' 's/func:/area:/g' {} +
```

### Team Members
Replace `[Team Lead]`, `[Manager]`, `owner:[name]` placeholders with your actual team.

### Events & Programs
Update event labels in `event-ops`, `event-countdown`, and `hackathon-tracker`.

### Decision Rights
Update approval chains in `escalation-tracker` and `budget-ops`.

### OKRs
Document your OKRs in the repo (`operations/okrs/`) so `/okr-check` can reference them.

---

## FAQ

**Do I need Claude Code?**
No. These work with any tool supporting [Agent Skills](https://agentskills.io) — Copilot, Cursor, Codex CLI, Gemini CLI, etc.

**Do I need all 23 skills?**
No. Start with `/standup` + `/epic-health` + `/pressure-test`. Add more as needed.

**Can Copilot Studio agents use these?**
Yes. Upload SKILL.md files as knowledge articles. See [Copilot Studio section](#using-skills-with-copilot-studio-agents).

**How do MCP servers relate to skills?**
Skills = instructions (what to do). MCP servers = connections (how to access data). Together: knowledge + capability.

**Can I contribute?**
Yes. PRs welcome. Follow the [Agent Skills spec](https://agentskills.io/specification), keep it generic, include a Gotchas section.

---

## Resources

| Resource | Link |
|----------|------|
| Agent Skills Specification | [agentskills.io](https://agentskills.io/specification) |
| GitHub Copilot Skills Docs | [docs.github.com](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| Microsoft MCP Catalog | [github.com/microsoft/mcp](https://github.com/microsoft/mcp) |
| GitHub MCP Server | [github.com/github/github-mcp-server](https://github.com/github/github-mcp-server) |
| Microsoft Work IQ | [learn.microsoft.com](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) |
| Dataverse MCP | [github.com/microsoft/Dataverse-MCP](https://github.com/microsoft/Dataverse-MCP) |
| Power BI MCP | [github.com/microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp) |
| Microsoft Learn MCP | [npmjs.com](https://www.npmjs.com/package/@anthropic/microsoft-learn-mcp) |
| Awesome MCP Servers | [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) |

---

*Built by the AI Apps & Agents Skilling team. Maintained by Victor (AI-CoS).*
*For questions or feedback, reach out to Aaron Stark.*
