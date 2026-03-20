# Solution Marketing Skills

**33 production-ready AI skills for marketing teams.**

Built from real operations managing 175+ issues, 30+ epics, 5 concurrent programs, monthly MBRs, and cross-solution coordination. These skills work with **Claude Code**, **GitHub Copilot**, **Copilot Studio**, **Cursor**, **Codex CLI**, and any platform supporting the [Agent Skills](https://agentskills.io) open standard (26+ platforms).

No technical setup required for the Corporate Productivity skills — they work in Claude.ai, Copilot Chat, or any AI assistant your team already uses.

---

## Table of Contents

- [Quick Start](#quick-start)
- [For Your Team (No GitHub Required)](#for-your-team-no-github-required)
- [Skills by Category](#skills-by-category)
  - [Corporate Productivity](#1-corporate-productivity)
  - [Competitive & Market Insights](#2-competitive--market-insights)
  - [Writing & Meeting Prep](#3-writing--meeting-prep)
  - [Data Insights & Analytics](#4-data-insights--analytics)
  - [Devil's Advocate](#5-devils-advocate)
  - [Strategic Thinking & OKR Alignment](#6-strategic-thinking--okr-alignment)
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

### For non-technical team members

Open **any** SKILL.md file in this repo, copy the content, and paste it into Claude.ai or Copilot Chat as instructions. That's it. For example:

1. Open [deck-builder/SKILL.md](.claude/skills/deck-builder/SKILL.md)
2. Copy the content
3. Paste into Claude.ai or Copilot Chat
4. Say: "Build me a stakeholder briefing deck on our hackathon program"

### For teams using Claude Code or Copilot

```bash
# Copy all skills to your repo
git clone https://github.com/akiyaani2/solution-marketing-skills.git
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/

# OR copy to .github/skills/ for Copilot coding agent
cp -r solution-marketing-skills/.claude/skills/* your-repo/.github/skills/

# OR add as submodule
git submodule add https://github.com/akiyaani2/solution-marketing-skills.git .claude/shared-skills
```

### Where to start

| If you... | Start with these |
|-----------|-----------------|
| Create presentations weekly | [`/deck-builder`](.claude/skills/deck-builder/) + [`/data-to-slides`](.claude/skills/data-to-slides/) |
| Prep for leadership meetings | [`/talking-points`](.claude/skills/talking-points/) + [`/exec-summary`](.claude/skills/exec-summary/) |
| Manage events or hackathons | [`/event-ops`](.claude/skills/event-ops/) + [`/hack-status`](.claude/skills/hackathon-tracker/) |
| Write emails to stakeholders | [`/email-drafter`](.claude/skills/email-drafter/) + [`/stakeholder-brief`](.claude/skills/stakeholder-brief/) |
| Track team work in GitHub | [`/standup`](.claude/skills/standup/) + [`/epic-health`](.claude/skills/epic-health/) |
| Need someone to challenge your plans | [`/pressure-test`](.claude/skills/pressure-test/) + [`/pre-mortem`](.claude/skills/pre-mortem/) |

---

## For Your Team (No GitHub Required)

These 8 skills are designed for people who live in **PowerPoint, Excel, Teams, and Outlook** — not GitHub. They work by pasting the skill instructions into any AI assistant.

| Skill | What Your Team Says | What They Get |
|-------|-------------------|--------------|
| [**deck-builder**](.claude/skills/deck-builder/) | "Build me slides for the leadership briefing" | Slide-by-slide outlines with titles, bullets, speaker notes, and chart suggestions. Ready to paste into PowerPoint. |
| [**exec-summary**](.claude/skills/exec-summary/) | "Summarize this 5-page doc for my director" | Half-page or one-page leadership-ready summary. Pyramid principle. Lead with the answer. |
| [**email-drafter**](.claude/skills/email-drafter/) | "Write a follow-up to the partner team after their conference" | Professional email calibrated to recipient seniority. Templates for status updates, asks, escalations, partner outreach. |
| [**one-pager**](.claude/skills/one-pager/) | "I need a brief on the NFL hackathon" | Structured program overview: what/why/who/when/metrics/investment/ask. One page. |
| [**talking-points**](.claude/skills/talking-points/) | "Prep me for tomorrow's meeting with my director" | Main points, supporting data, anticipated Q&A, and landmines to avoid. |
| [**excel-analyzer**](.claude/skills/excel-analyzer/) | "What do these registration numbers mean?" | Data summary, trend analysis, anomaly detection, and actionable insights from any spreadsheet. |
| [**stakeholder-brief**](.claude/skills/stakeholder-brief/) | "I need to update everyone on the same thing" | Same facts reframed for each audience: VP, skip-level, peers, team, and partners. |
| [**data-to-slides**](.claude/skills/data-to-slides/) | "Turn these metrics into MBR slides" | Presentation-ready content with chart recommendations, narrative framing, and key takeaways. |

**How your team uses these:**
- **In Claude.ai**: Paste the SKILL.md content as a message, then describe what you need
- **In Copilot Chat** (Teams, Word, PowerPoint): Reference the skill by saying "follow these instructions: [paste SKILL.md]"
- **In Copilot Studio**: Upload SKILL.md files as knowledge articles for a custom agent (see [Copilot Studio section](#using-skills-with-copilot-studio-agents))

---

## Skills by Category

### 1. Corporate Productivity

*"Help me do my actual job faster."*

Skills for the daily work of corporate marketing — presentations, emails, meeting prep, data analysis. No GitHub or CLI required.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**deck-builder**](.claude/skills/deck-builder/) | `/deck-builder` | Builds PowerPoint slide outlines from bullet points or data. Templates for stakeholder briefings, MBRs, event one-pagers. |
| [**exec-summary**](.claude/skills/exec-summary/) | `/exec-summary` | Distills complex documents into leadership-ready summaries. Half-page and one-page formats. |
| [**email-drafter**](.claude/skills/email-drafter/) | `/email-drafter` | Drafts professional emails calibrated to recipient and situation. Status updates, asks, escalations, partner outreach. |
| [**one-pager**](.claude/skills/one-pager/) | `/one-pager` | Creates single-page program briefs. What/why/who/when/metrics/investment/ask structure. |
| [**talking-points**](.claude/skills/talking-points/) | `/talking-points` | Generates meeting prep with main points, anticipated Q&A, and landmines to avoid. |
| [**excel-analyzer**](.claude/skills/excel-analyzer/) | `/excel-analyzer` | Analyzes spreadsheets — summaries, trends, anomalies, budget tracking, registration funnels. |
| [**stakeholder-brief**](.claude/skills/stakeholder-brief/) | `/stakeholder-brief` | Adapts one update for multiple audiences: VP, skip-level, peers, team, partners. |
| [**data-to-slides**](.claude/skills/data-to-slides/) | `/data-to-slides` | Transforms raw data into presentation-ready content with chart suggestions and narrative framing. |

---

### 2. Competitive & Market Insights

*"What's happening out there?"*

Skills for understanding the competitive landscape, tracking market signals, and grounding decisions in real intelligence.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**competitive-scan**](.claude/skills/competitive-scan/) | `/competitive-scan` | Structured scanning of AWS, GCP, and partner ecosystems. Produces competitive briefs for leadership. |
| [**takeshi-tuesday**](.claude/skills/takeshi-tuesday/) | `/takeshi-tuesday` | Captures weekly market insights with persistent memory. Tracks signals, competitive moves, and content ideas over time. |
| [**content-pipeline**](.claude/skills/content-pipeline/) | `/content-pipeline` | Tracks content production including competitive analysis content. Blog, video, social, and session pipeline. |
| [**cross-solution-sync**](.claude/skills/cross-solution-sync/) | `/x-sp` | Cross-team coordination that surfaces market context and shared intelligence across solution plays. |

**Best MCP servers:** [Microsoft Learn MCP](#1-microsoft-learn-mcp), [Power BI MCP](#5-power-bi-modeling-mcp)

---

### 3. Writing & Meeting Prep

*"Help me prepare and produce."*

Skills for meeting preparation, report generation, and team communication.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**meeting-notes**](.claude/skills/meeting-notes/) | `/meeting-notes` | Captures notes, auto-extracts action items. Templates for 1:1s, syncs, Takeshi Tuesday, and MBR prep. |
| [**1on1-prep**](.claude/skills/1on1-prep/) | `/1on1-prep [name]` | 1:1 meeting prep with open issues, blockers, wins, and FIRE framework. |
| [**mbr**](.claude/skills/mbr/) | `/mbr` | Monthly Business Review package. Executive summary, owner breakdowns, metrics, narrative template. |
| [**peer-digest**](.claude/skills/peer-digest/) | `/peer-digest` | Friday cross-team update. What shipped, what's coming, cross-team heads up. Under 10 lines. |
| [**standup**](.claude/skills/standup/) | `/standup` | Daily async standup from GitHub activity. Yesterday / today / blockers per person. |
| [**weekly-status**](.claude/skills/weekly-status/) | `/weekly-status [name]` | Weekly status per team member. Completed, in-progress, blocked, coming up. |
| [**escalation-tracker**](.claude/skills/escalation-tracker/) | `/escalations` | Tracks items escalated to leadership with SLA monitoring and response tracking. |
| [**git-sync**](.claude/skills/git-sync/) | `/git-sync` | Automated git pull / commit / push with standardized messages. |

**Best MCP servers:** [GitHub MCP](#3-github-mcp-server), [Work IQ MCP](#2-microsoft-work-iq-mcp)

---

### 4. Data Insights & Analytics

*"Show me the numbers."*

Skills for metrics, health scoring, trend analysis, and operational dashboards.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**epic-health**](.claude/skills/epic-health/) | `/epic-health` | Scores epics on 5 health dimensions. Completion %, blocked count, staleness, RAPID, TBD count. |
| [**issue-triage**](.claude/skills/issue-triage/) | `/issue-triage` | Bulk triage with P0 downgrade cascade pattern. Stale detection, orphan reparenting, label hygiene. |
| [**solution-play-health**](.claude/skills/solution-play-health/) | `/sp-health` | Strategic health scoring for solution plays. Layer above epic-health. Quarter-over-quarter comparison. |
| [**hackathon-tracker**](.claude/skills/hackathon-tracker/) | `/hack-status` | Hackathon pipeline tracking. Registration funnels, judging status, cross-hack comparison, outcomes analysis. |
| [**event-countdown**](.claude/skills/event-countdown/) | `/event-countdown` | Event readiness dashboard. Three urgency tiers with readiness % and color coding. |
| [**event-ops**](.claude/skills/event-ops/) | `/event-ops` | Full event lifecycle. Workback schedules, vendor checklists, readiness scoring, cross-event calendar. |
| [**budget-ops**](.claude/skills/budget-ops/) | `/budget-ops` | PO tracking, co-marketing fund utilization, vendor management, budget reports for leadership. |
| [**post-mortem**](.claude/skills/post-mortem/) | `/post-mortem` | Structured retrospective after events/programs. Persistent learning log for cross-event improvement. |

**Best MCP servers:** [GitHub MCP](#3-github-mcp-server), [Power BI MCP](#5-power-bi-modeling-mcp), [Dataverse MCP](#4-dataverse-mcp)

---

### 5. Devil's Advocate

*"Challenge my thinking."*

Skills that push back, find holes, and make your plans stronger before you commit. The category most teams don't have and need most.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**pressure-test**](.claude/skills/pressure-test/) | `/pressure-test` | Challenges any plan through 7 lenses: assumptions, resources, timeline, stakeholders, competition, failure modes, opportunity cost. |
| [**pre-mortem**](.claude/skills/pre-mortem/) | `/pre-mortem` | "It failed. Why?" Reasons backwards from failure to surface risks before they happen. |
| [**red-team**](.claude/skills/red-team/) | `/red-team` | Takes the competitor's perspective. How would AWS or GCP counter your program? |

**Best MCP servers:** [Microsoft Learn MCP](#1-microsoft-learn-mcp) (verify claims), [GitHub MCP](#3-github-mcp-server) (check assumptions against data)

---

### 6. Strategic Thinking & OKR Alignment

*"Am I working on the right things?"*

Skills for planning, goal alignment, decision governance, and making sure daily work connects to quarterly objectives.

| Skill | Command | What It Does |
|-------|---------|-------------|
| [**okr-check**](.claude/skills/okr-check/) | `/okr-check` | Scores any work item against OKRs. Scans for orphaned work, uncovers drift, checks KR coverage. |
| [**decision**](.claude/skills/decision/) | `/decision "desc"` | Creates formatted decision records (TD-NNNN). Context, options, decision, RAPID roles, consequences. |
| [**solution-play-health**](.claude/skills/solution-play-health/) | `/sp-health` | Strategic health scoring. Also appears in Data Insights — it bridges analytics and strategy. |

**Best MCP servers:** [GitHub MCP](#3-github-mcp-server), [Dataverse MCP](#4-dataverse-mcp)

---

## How Skills Work Together

```
                ┌──────────────────────────────────────────────────┐
                │              YOUR OPERATING RHYTHM                │
                └──────────────────────────────────────────────────┘

   DAILY         /standup ──────────────────────► team pulse

   WEEKLY        /weekly-status ────────────────► per-person view
                 /issue-triage ─────────────────► board hygiene
                 /epic-health ──────────────────► initiative health
                 /x-sp ─────────────────────────► cross-team sync
                 /takeshi-tuesday ──────────────► market intelligence

   BIWEEKLY      /1on1-prep ────────────────────► meeting prep
                 /meeting-notes ────────────────► capture action items

   MONTHLY       /mbr ──────────────────────────► leadership package
                 /budget-ops ───────────────────► financial status
                 /sp-health ────────────────────► strategic view
                 /competitive-scan ─────────────► market landscape

   FRIDAY        /peer-digest ──────────────────► cross-team update

   EVENTS        /event-ops ────────────────────► logistics
                 /hack-status ──────────────────► hackathon pipeline
                 /event-countdown ──────────────► readiness check
                 /post-mortem ──────────────────► after-action review

   BEFORE ANY    /pressure-test ────────────────► find the holes
   BIG DECISION  /pre-mortem ───────────────────► imagine failure
                 /red-team ─────────────────────► competitor view
                 /okr-check ────────────────────► alignment check

   ANYTIME       /deck-builder ─────────────────► create slides
                 /exec-summary ─────────────────► condense for leadership
                 /email-drafter ────────────────► draft communications
                 /one-pager ────────────────────► program brief
                 /talking-points ───────────────► meeting prep
                 /excel-analyzer ───────────────► analyze data
                 /stakeholder-brief ────────────► multi-audience comms
                 /data-to-slides ───────────────► metrics → presentation
                 /escalations ──────────────────► track upward items
                 /decision ─────────────────────► document decisions
                 /content-pipeline ─────────────► content status
```

**Key composition chains:**
- `/standup` → `/weekly-status` → `/mbr` (daily → weekly → monthly roll-up)
- `/epic-health` → `/sp-health` → `/okr-check` (tactical → strategic → alignment)
- `/1on1-prep` + `/meeting-notes` → action items → `/issue-triage` (prepare → capture → track)
- `/competitive-scan` → `/red-team` → `/pressure-test` (intelligence → adversarial → hardening)
- `/event-countdown` → `/event-ops` → `/hack-status` → `/post-mortem` (awareness → logistics → execution → learning)
- `/takeshi-tuesday` → `/competitive-scan` + `/content-pipeline` (insights → research + content ideas)
- `/excel-analyzer` → `/data-to-slides` → `/deck-builder` (raw data → charts → full deck)
- `/exec-summary` → `/email-drafter` or `/stakeholder-brief` (condense → distribute)

---

## MCP Servers

[Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers give AI agents real-time access to tools and data. These skills become dramatically more powerful when paired with the right MCP servers.

### Recommended Setup (Priority Order)

#### 1. Microsoft Learn MCP

**What:** Search and fetch official Microsoft/Azure documentation.
**Why first:** Grounds all research, competitive analysis, and content creation in accurate, official information.

| Tool | What It Does |
|------|-------------|
| `microsoft_docs_search` | Search docs, return concise content chunks |
| `microsoft_code_sample_search` | Find working code examples |
| `microsoft_docs_fetch` | Fetch full page content from any Learn URL |

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

---

#### 2. Microsoft Work IQ MCP

**What:** Enterprise MCP servers for Microsoft 365 — Teams, Calendar, Mail, SharePoint, OneDrive, Word.
**Why:** Connects your AI to the tools your team already uses daily.
**Requires:** M365 Copilot license. Currently in preview.

| Server | Skills It Powers |
|--------|-----------------|
| Work IQ Calendar | meeting-notes, 1on1-prep, event-ops |
| Work IQ Teams | peer-digest, standup, escalation-tracker |
| Work IQ Mail | email-drafter, mbr, escalation-tracker |
| Work IQ SharePoint | budget-ops, content-pipeline, one-pager |
| Work IQ Word | exec-summary, mbr, meeting-notes, decision |

**Setup:** [Work IQ documentation](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview)

---

#### 3. GitHub MCP Server

**What:** Full CRUD on GitHub Issues, PRs, Projects, Discussions.
**Why:** Data backbone for all reporting and analytics skills.

| Capability | Skills It Powers |
|-----------|-----------------|
| Issue CRUD | issue-triage, epic-health, standup, weekly-status |
| Project boards | hackathon-tracker, event-ops, solution-play-health |
| Discussions | meeting-notes, decision |

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

**What:** Query business data in Dynamics 365 / Dataverse via natural language.
**Why:** Connects skills to CRM data, program records, OKR tracking.

**Repo:** [microsoft/Dataverse-MCP](https://github.com/microsoft/Dataverse-MCP)

---

#### 5. Power BI Modeling MCP

**What:** Query Power BI semantic models, generate DAX, analyze dashboards.
**Why:** Connects analytics skills directly to your existing dashboards.

**Repo:** [microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp) (519 stars)

---

### MCP + Skills Matrix

| MCP Server | Corporate Prod. | Competitive | Writing | Analytics | Devil's Advocate | Strategy |
|-----------|----------------|-------------|---------|-----------|-----------------|----------|
| Microsoft Learn | Support | Primary | Support | — | Verification | — |
| Work IQ | **Primary** | — | **Primary** | — | — | — |
| GitHub | — | Support | Primary | **Primary** | Data source | Data source |
| Dataverse | Support | Support | — | Primary | — | **Primary** |
| Power BI | **Primary** | Support | — | **Primary** | — | Support |

---

## Using Skills with Copilot

These skills follow the [Agent Skills](https://agentskills.io) standard — the same format works across Claude Code and GitHub Copilot.

### Copilot Coding Agent (Autonomous)

1. Place skills in `.github/skills/` or `.claude/skills/`
2. Assign a GitHub Issue to Copilot (like assigning to a team member)
3. Copilot reads the relevant skill, pulls data, and opens a draft PR

### Copilot Chat

```
@workspace Use the pressure-test skill to challenge our Build 2026 plan
```

### Copilot in PowerPoint (for Corporate Productivity skills)

```
Create a stakeholder briefing using these instructions: [paste deck-builder SKILL.md content]
```

### Copilot in Excel (for excel-analyzer)

```
Analyze this data following these instructions: [paste excel-analyzer SKILL.md content]
```

---

## Using Skills with Copilot Studio Agents

### For Teams That Don't Use GitHub

1. **Create a new agent** in Copilot Studio
2. **Upload SKILL.md files** as knowledge sources (no repo needed)
3. **Map topics** to skills: "Build me a deck" → triggers deck-builder instructions
4. **Deploy to Teams** — team members ask the agent in a Teams chat
5. **Connect Work IQ MCP** for Calendar, Mail, and SharePoint access

### For Teams With GitHub

1. **Create agent** in Copilot Studio
2. **Point knowledge source** to this repo
3. **Connect GitHub MCP** for live issue/board data
4. **Connect Power Automate** for `gh` CLI execution
5. **Deploy to Teams** with full GitHub integration

### Pro-Code (Microsoft Foundry)

Use the [Microsoft Agent Framework](https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/) to build agents that read SKILL.md files, use MCP servers, and run on Azure.

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
description: What this does and when to trigger it (written for the AI model, not humans)
allowed-tools: Read, Write, Edit, Bash
---

# My Skill
Step-by-step instructions...
```

### Best Practices (from [Anthropic's Guide](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf))

- **Description is a trigger, not a summary** — it tells the AI when to activate the skill
- **Gotchas section is highest-signal content** — capture common failure points
- **Skills are folders, not just markdown** — include scripts, templates, data
- **Progressive disclosure** — keep SKILL.md under 500 lines, link to reference files
- **Don't state the obvious** — Claude already knows how to code and reason
- **Build memory** — store logs (JSONL) so skills can track history and deltas

---

## Setting Up Your Repo

### Prerequisites

- An AI coding tool (Claude Code, Copilot, Cursor, etc.)
- For GitHub-based skills: [GitHub CLI](https://cli.github.com/) (`gh`) installed
- For Corporate Productivity skills: Just an AI assistant (Claude.ai, Copilot Chat, etc.)

### Recommended Labels (for GitHub-based skills)

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

---

## Customization Guide

### For Corporate Productivity Skills
Replace audience names and organizational references in deck-builder, email-drafter, talking-points, stakeholder-brief with your actual leadership chain and team names.

### For GitHub-Based Skills
Update label prefixes, team member names, event labels, decision rights, and OKR references.

```bash
# Replace label prefixes to match your repo
find .claude/skills -name "SKILL.md" -exec sed -i '' 's/func:/area:/g' {} +
```

---

## FAQ

**Do I need Claude Code?**
No. Corporate Productivity skills (deck-builder, email-drafter, etc.) work by pasting instructions into any AI assistant. GitHub-based skills work with any tool supporting [Agent Skills](https://agentskills.io).

**Do I need GitHub?**
Only for the reporting/analytics skills (standup, epic-health, etc.). The Corporate Productivity and Devil's Advocate skills work without GitHub.

**Do I need all 33 skills?**
No. Start with what fits your role. See the [Where to Start](#where-to-start) table.

**Can Copilot Studio agents use these?**
Yes. Upload SKILL.md files as knowledge articles, or point to this repo as a knowledge source. See [Copilot Studio section](#using-skills-with-copilot-studio-agents).

**How do MCP servers relate to skills?**
Skills = instructions (what to do). MCP servers = connections (how to access data). Together: knowledge + capability.

**Can I contribute?**
Yes. PRs welcome. Follow the [Agent Skills spec](https://agentskills.io/specification), keep it generic, include a Gotchas section.

---

## Resources

| Resource | Link |
|----------|------|
| Agent Skills Specification | [agentskills.io](https://agentskills.io/specification) |
| Anthropic's Complete Guide to Skills (PDF) | [resources.anthropic.com](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) |
| Thariq's Blog: How Anthropic Uses Skills | [x.com/trq212](https://x.com/trq212/status/2033949937936085378) |
| Skill Authoring Best Practices | [platform.claude.com](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) |
| Claude Code Skills Docs | [code.claude.com](https://code.claude.com/docs/en/skills) |
| GitHub Copilot Skills Docs | [docs.github.com](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| Microsoft MCP Catalog | [github.com/microsoft/mcp](https://github.com/microsoft/mcp) |
| GitHub MCP Server | [github.com/github/github-mcp-server](https://github.com/github/github-mcp-server) |
| Microsoft Work IQ | [learn.microsoft.com](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) |
| Dataverse MCP | [github.com/microsoft/Dataverse-MCP](https://github.com/microsoft/Dataverse-MCP) |
| Power BI MCP | [github.com/microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp) |
| Microsoft Learn MCP | [npmjs.com](https://www.npmjs.com/package/@anthropic/microsoft-learn-mcp) |

---

*Built by the AI Apps & Agents Skilling team. Maintained by the team.*
*For questions or feedback, open an issue or submit a PR.*
