# 🚀 Solution Marketing Skills

AI-powered toolkit for Solution and Partner Marketing teams. Prompts, skills, automations, and Copilot Studio agent blueprints — ready to use.

Works with **Copilot**, **Claude**, **ChatGPT**, and any AI assistant. No technical setup required.

---

## 🔍 What do you want to do?

| I want to... | Go here |
|-------------|---------|
| 📊 **Build a deck or presentation** | [deck-builder](.claude/skills/deck-builder/) or [doc-to-slides](prompts/doc-to-slides.md) |
| 🎯 **Prep for a meeting** | [talking-points](.claude/skills/talking-points/) or [1on1-prep](.claude/skills/1on1-prep/) |
| ✉️ **Draft an email** | [email-drafter](.claude/skills/email-drafter/) or [recap-email](prompts/recap-email.md) |
| 📈 **Analyze data or a spreadsheet** | [excel-analyzer](.claude/skills/excel-analyzer/) or [data-to-insights](prompts/data-to-insights.md) |
| 📝 **Summarize something for leadership** | [exec-summary](.claude/skills/exec-summary/) |
| 📄 **Create a one-pager or brief** | [one-pager](.claude/skills/one-pager/) |
| ✍️ **Write a blog post** | [blog-from-bullets](prompts/blog-from-bullets.md) |
| 📱 **Write social media posts** | [social-from-content](prompts/social-from-content.md) |
| 📣 **Write a weekly leadership update** | [leadership-update](.claude/skills/leadership-update/) |
| 🔥 **Challenge or pressure-test a plan** | [pressure-test](.claude/skills/pressure-test/) or [pre-mortem](.claude/skills/pre-mortem/) |
| 🥊 **Think like the competition** | [red-team](.claude/skills/red-team/) or [competitive-scan](.claude/skills/competitive-scan/) |
| 🎯 **Check if work aligns to OKRs** | [okr-check](.claude/skills/okr-check/) |
| 📅 **Track events or hackathons** | [event-ops](.claude/skills/event-ops/) or [hackathon-tracker](.claude/skills/hackathon-tracker/) |
| ⚙️ **Set up automated reports in Teams** | [scheduled-tasks/](scheduled-tasks/) |
| 🤖 **Build a Copilot Studio agent** | [How to Build an Agent](copilot-studio/HOW-TO-BUILD-AN-AGENT.md) |

---

## 💡 Prompts vs Skills — What's the difference?

| | 📝 Prompt | ⚡ Skill |
|---|----------|---------|
| **What it is** | A single instruction you paste into AI | A full playbook the AI follows like a trained expert |
| **Best for** | One-off tasks ("write me a blog post") | Repeatable workflows ("run meeting prep every week") |
| **Depth** | One question → one answer | Multi-step with templates, decision logic, quality checks |
| **Memory** | Starts fresh every time | Can track history across sessions |

**Start with prompts** if you're new. **Graduate to skills** when you repeat the same prompt weekly.

> 📖 [What are skills?](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) · [Skill best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — from Anthropic

---

## ⚡ How to use

**Prompts** (30 seconds) — Open any file in [`/prompts/`](prompts/), copy, paste into Copilot or Claude, done.

**Skills** (2 minutes) — Open any file in [`/skills/`](.claude/skills/), copy the content, paste as instructions, describe your task.

<details>
<summary>🔧 Install into your team's workspace (IT/admins)</summary>

```bash
git clone https://github.com/akiyaani2/solution-marketing-skills.git
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/
```
</details>

---

## 📦 What's inside

### 📝 [Prompts](prompts/) — Copy-paste, zero setup

10 ready-to-use prompts. Open one, copy, paste, go.

| Prompt | What You Get |
|--------|-------------|
| [Blog from Bullets](prompts/blog-from-bullets.md) | Structured blog post from rough notes |
| [Meeting to Actions](prompts/meeting-to-actions.md) | Action items with owners and dates |
| [Data to Insights](prompts/data-to-insights.md) | Executive narrative from raw numbers |
| [Recap Email](prompts/recap-email.md) | Professional recap with decisions and next steps |
| [Doc to Slides](prompts/doc-to-slides.md) | 5-7 slide outline with speaker notes |
| [Tighten Copy](prompts/tighten-copy.md) | Same message, half the words |
| [Social from Content](prompts/social-from-content.md) | LinkedIn + X posts from any content |
| [Partnership Proposal](prompts/partnership-proposal.md) | Structured proposal outline |
| [Kickoff Agenda](prompts/kickoff-agenda.md) | Meeting agenda with time allocations |
| [OKRs to Milestones](prompts/okrs-to-milestones.md) | Quarterly milestones from objectives |

---

### ⚡ [Skills](.claude/skills/) — Deeper capabilities

34 skills organized into 3 groups based on how you work.

<details>
<summary>💼 <strong>Everyday Work</strong> — Presentations, emails, meeting prep, data, writing (17 skills)</summary>

The skills your team uses daily. Decks, emails, summaries, meeting prep, data analysis, and content.

| Skill | What It Does |
|-------|-------------|
| [deck-builder](.claude/skills/deck-builder/) | Slide outlines from bullet points or data — templates for briefings, MBRs, one-pagers |
| [exec-summary](.claude/skills/exec-summary/) | Distills any document into a leadership-ready summary |
| [email-drafter](.claude/skills/email-drafter/) | Professional emails calibrated to recipient and situation |
| [one-pager](.claude/skills/one-pager/) | Single-page program brief (what/why/who/when/metrics/ask) |
| [talking-points](.claude/skills/talking-points/) | Meeting prep with main points, anticipated Q&A, and landmines |
| [excel-analyzer](.claude/skills/excel-analyzer/) | Spreadsheet analysis — trends, anomalies, insights |
| [stakeholder-brief](.claude/skills/stakeholder-brief/) | Same update reframed for VP, peers, team, and partners |
| [data-to-slides](.claude/skills/data-to-slides/) | Turn raw data into presentation content with chart suggestions |
| [leadership-update](.claude/skills/leadership-update/) | Weekly exec update with strategic framing and altitude check |
| [meeting-notes](.claude/skills/meeting-notes/) | Capture notes and auto-extract action items |
| [1on1-prep](.claude/skills/1on1-prep/) | 1:1 meeting prep with FIRE framework |
| [mbr](.claude/skills/mbr/) | Monthly Business Review package for leadership |
| [peer-digest](.claude/skills/peer-digest/) | Friday cross-team update in under 10 lines |
| [standup](.claude/skills/standup/) | Daily async standup from project activity |
| [weekly-status](.claude/skills/weekly-status/) | Weekly status per team member |
| [escalation-tracker](.claude/skills/escalation-tracker/) | Track items sent to leadership with SLA monitoring |
| [content-pipeline](.claude/skills/content-pipeline/) | Content production pipeline — blog, video, social, sessions |

</details>

<details>
<summary>🔥 <strong>Strategy & Challenge</strong> — Competitive intel, pressure testing, OKR alignment (10 skills)</summary>

Skills that make your strategy sharper. Research the market, challenge your plans, and stay aligned to goals.

| Skill | What It Does |
|-------|-------------|
| [pressure-test](.claude/skills/pressure-test/) | Challenge any plan through 7 lenses — assumptions, resources, timeline, competition, failure modes |
| [pre-mortem](.claude/skills/pre-mortem/) | "It's 6 months from now and this failed. Why?" — surface risks before they happen |
| [red-team](.claude/skills/red-team/) | Take the competitor's perspective — how would they counter your program? |
| [competitive-scan](.claude/skills/competitive-scan/) | Structured scanning of AWS, GCP, and partner ecosystems |
| [takeshi-tuesday](.claude/skills/takeshi-tuesday/) | Weekly market insights with persistent memory across sessions |
| [okr-check](.claude/skills/okr-check/) | Score any work item against OKRs — detect drift, find orphaned work |
| [decision](.claude/skills/decision/) | Formatted decision records with options, RAPID roles, consequences |
| [solution-play-health](.claude/skills/solution-play-health/) | Strategic health scoring across solution plays |
| [cross-solution-sync](.claude/skills/cross-solution-sync/) | Cross-team coordination — shared workstreams, dependencies, joint events |
| [post-mortem](.claude/skills/post-mortem/) | After-action review with persistent learning log |

</details>

<details>
<summary>📋 <strong>Program Operations</strong> — Events, hackathons, budgets, board management (7 skills)</summary>

Skills for running programs at scale. Event logistics, hackathon pipelines, budget tracking, and project health.

| Skill | What It Does |
|-------|-------------|
| [event-ops](.claude/skills/event-ops/) | Full event lifecycle — workback schedules, vendor checklists, readiness scoring |
| [hackathon-tracker](.claude/skills/hackathon-tracker/) | Hackathon pipeline — registration, judging, outcomes, cross-hack comparison |
| [event-countdown](.claude/skills/event-countdown/) | Event readiness dashboard with urgency tiers |
| [budget-ops](.claude/skills/budget-ops/) | PO tracking, co-marketing funds, vendor management, budget reports |
| [epic-health](.claude/skills/epic-health/) | Score initiatives on 5 health dimensions |
| [issue-triage](.claude/skills/issue-triage/) | Bulk triage with priority cascade pattern |
| [git-sync](.claude/skills/git-sync/) | Automated project sync |

</details>

---

### ⏰ [Scheduled Tasks](scheduled-tasks/) — Automations for Copilot Studio

Set up once, runs on autopilot. Posts to Teams or saves to SharePoint.

| When | What | Where |
|------|------|-------|
| Daily 9am | [Morning Pulse](scheduled-tasks/daily-morning-pulse.md) | 💬 Teams |
| Weekly Monday | [Board Health Check](scheduled-tasks/weekly-board-health.md) | 💬 Teams |
| Weekly Friday | [Friday Digest](scheduled-tasks/weekly-friday-digest.md) | 💬 Teams |
| Monthly 8th | [MBR Data Package](scheduled-tasks/monthly-mbr-package.md) | 📁 SharePoint |
| On new item | [Auto-Triage](scheduled-tasks/new-issue-auto-triage.md) | 🏷️ Auto-label |

### 🤖 [Agents](agents/) — Copilot Studio blueprints

4 agents ready to deploy to Teams. Each includes build guide, knowledge sources, and instructions.

| Agent | What It Does | For |
|-------|-------------|-----|
| [Marketing Assistant](agents/marketing-assistant/) | Decks, emails, summaries, meeting prep | 👥 Everyone |
| [Strategy Advisor](agents/strategy-advisor/) | Competitive intel, pressure testing, OKR alignment | 👤 Leads |
| [Program Tracker](agents/program-tracker/) | Events, hackathons, budgets, countdowns | 📋 Program owners |
| [Reporting Engine](agents/reporting-engine/) | Standups, weekly status, MBR, 1:1 prep | 📊 Team leads |

---

## 📚 Going deeper

| Topic | Guide |
|-------|-------|
| 🤖 Build a Copilot Studio agent | [How to Build an Agent](copilot-studio/HOW-TO-BUILD-AN-AGENT.md) |
| 🚀 Deploy agents to your team | [Deployment Guide](copilot-studio/DEPLOYMENT-GUIDE.md) |
| 📏 Agent creation rules | [Agent Rules](AGENT-RULES.md) |
| 🔒 Security & compliance | [Security Overview](copilot-studio/SECURITY-OVERVIEW.md) |

<details>
<summary>🔌 <strong>Live data connectors</strong> (advanced — may need IT support)</summary>

| Connector | What It Does | More Info |
|-----------|-------------|-----------|
| Microsoft Learn | Search official Microsoft docs | [Setup](https://www.npmjs.com/package/@anthropic/microsoft-learn-mcp) |
| Work IQ (M365) | Teams, Calendar, Mail, SharePoint | [Docs](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) |
| GitHub | Issues, boards, project status | [Docs](https://github.com/github/github-mcp-server) |
| Dataverse | Business data and CRM records | [Docs](https://github.com/microsoft/Dataverse-MCP) |
| Power BI | Dashboard metrics and charts | [Docs](https://github.com/microsoft/powerbi-modeling-mcp) |

</details>

---

## ❓ FAQ

**Do I need to be technical?** No. Start with [Prompts](prompts/).

**Do I need GitHub?** Only for Program Operations skills. Everything else works without it.

**What AI tool do I need?** Any — Copilot, Claude, ChatGPT.

**How do I contribute?** Suggestions welcome. Open an issue or reach out.

---

*🤖 Built by Victor, AI Chief of Staff. Questions? Open an issue or reach out.*
