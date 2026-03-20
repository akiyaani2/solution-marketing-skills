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
| 🔥 **Challenge or pressure-test a plan** | [pressure-test](.claude/skills/pressure-test/) or [pre-mortem](.claude/skills/pre-mortem/) |
| 🥊 **Think like the competition** | [red-team](.claude/skills/red-team/) or [competitive-scan](.claude/skills/competitive-scan/) |
| 🎯 **Check if work aligns to OKRs** | [okr-check](.claude/skills/okr-check/) |
| 📅 **Track events or hackathons** | [event-ops](.claude/skills/event-ops/) or [hackathon-tracker](.claude/skills/hackathon-tracker/) |
| 📣 **Write a weekly leadership update** | [leadership-update](.claude/skills/leadership-update/) |
| ⚙️ **Set up automated reports in Teams** | [scheduled-tasks/](scheduled-tasks/) |
| 🤖 **Build a Copilot Studio agent** | [How to Build an Agent](copilot-studio/HOW-TO-BUILD-AN-AGENT.md) |

---

## ⚡ How to use

**Option 1: Copy-paste a prompt** (30 seconds)

Open any file in [`/prompts/`](prompts/), copy the prompt, paste into Copilot Chat or Claude, done.

**Option 2: Use a skill** (2 minutes)

Open any skill file in the [skills folder](.claude/skills/), copy the content, paste into your AI tool as instructions, then describe your task.

**Option 3: Install into your team's workspace** (for IT/admins — 5 minutes)

<details>
<summary>🔧 Technical setup (click to expand)</summary>

```bash
git clone https://github.com/akiyaani2/solution-marketing-skills.git
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/
```

Or for Copilot: copy to `.github/skills/` instead.
</details>

---

## 📦 What's in this repo

### 📝 [Prompts](prompts/) — Copy-paste, zero setup

| Prompt | What You Get |
|--------|-------------|
| [Blog from Bullets](prompts/blog-from-bullets.md) | Structured blog post from rough notes |
| [Meeting to Actions](prompts/meeting-to-actions.md) | Action items with owners and dates from messy notes |
| [Data to Insights](prompts/data-to-insights.md) | Executive narrative from raw numbers |
| [Recap Email](prompts/recap-email.md) | Professional recap with decisions and next steps |
| [Doc to Slides](prompts/doc-to-slides.md) | 5-7 slide outline with speaker notes |
| [Tighten Copy](prompts/tighten-copy.md) | Same message, half the words |
| [Social from Content](prompts/social-from-content.md) | LinkedIn + X posts from any content |
| [Partnership Proposal](prompts/partnership-proposal.md) | Structured proposal outline |
| [Kickoff Agenda](prompts/kickoff-agenda.md) | Meeting agenda with time allocations |
| [OKRs to Milestones](prompts/okrs-to-milestones.md) | Quarterly milestones from objectives |

### ⚡ [Skills](.claude/skills/) — Deeper capabilities for AI tools

<details>
<summary>💼 <strong>Corporate Productivity</strong> — Decks, emails, summaries, data (8 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [deck-builder](.claude/skills/deck-builder/) | Slide outlines from bullet points or data |
| [exec-summary](.claude/skills/exec-summary/) | Leadership-ready summaries |
| [email-drafter](.claude/skills/email-drafter/) | Professional emails for any audience |
| [one-pager](.claude/skills/one-pager/) | Program briefs (what/why/who/when/ask) |
| [talking-points](.claude/skills/talking-points/) | Meeting prep with Q&A and landmines |
| [excel-analyzer](.claude/skills/excel-analyzer/) | Spreadsheet analysis and insights |
| [stakeholder-brief](.claude/skills/stakeholder-brief/) | Same update, multiple audiences |
| [data-to-slides](.claude/skills/data-to-slides/) | Raw data into presentation content |
</details>

<details>
<summary>🔭 <strong>Competitive & Market Insights</strong> — Research and intelligence (4 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [competitive-scan](.claude/skills/competitive-scan/) | AWS/GCP/partner landscape scanning |
| [takeshi-tuesday](.claude/skills/takeshi-tuesday/) | Weekly market insights with persistent memory |
| [content-pipeline](.claude/skills/content-pipeline/) | Content production pipeline tracking |
| [cross-solution-sync](.claude/skills/cross-solution-sync/) | Cross-team coordination layer |
</details>

<details>
<summary>✏️ <strong>Writing & Meeting Prep</strong> — Reports, standups, leadership updates (9 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [leadership-update](.claude/skills/leadership-update/) | Weekly exec update with altitude check and strategic framing |
| [meeting-notes](.claude/skills/meeting-notes/) | Capture notes, extract action items |
| [1on1-prep](.claude/skills/1on1-prep/) | 1:1 prep with FIRE framework |
| [mbr](.claude/skills/mbr/) | Monthly Business Review package |
| [peer-digest](.claude/skills/peer-digest/) | Friday cross-team update (<10 lines) |
| [standup](.claude/skills/standup/) | Daily async standup from GitHub |
| [weekly-status](.claude/skills/weekly-status/) | Weekly status per person |
| [escalation-tracker](.claude/skills/escalation-tracker/) | Track items sent to leadership |
| [git-sync](.claude/skills/git-sync/) | Automated git pull/commit/push |
</details>

<details>
<summary>📊 <strong>Data Insights & Analytics</strong> — Metrics, health scoring, operations (8 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [epic-health](.claude/skills/epic-health/) | Score initiatives on 5 health dimensions |
| [issue-triage](.claude/skills/issue-triage/) | Bulk triage with P0 cascade pattern |
| [solution-play-health](.claude/skills/solution-play-health/) | Strategic scoring for solution plays |
| [hackathon-tracker](.claude/skills/hackathon-tracker/) | Hackathon pipeline and outcomes |
| [event-countdown](.claude/skills/event-countdown/) | Event readiness dashboard |
| [event-ops](.claude/skills/event-ops/) | Event lifecycle management |
| [budget-ops](.claude/skills/budget-ops/) | PO tracking, budget reports |
| [post-mortem](.claude/skills/post-mortem/) | After-action review with persistent log |
</details>

<details>
<summary>🔥 <strong>Devil's Advocate</strong> — Challenge your thinking (3 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [pressure-test](.claude/skills/pressure-test/) | Challenge plans through 7 lenses |
| [pre-mortem](.claude/skills/pre-mortem/) | "It failed. Why?" — reason backwards |
| [red-team](.claude/skills/red-team/) | Think like the competition |
</details>

<details>
<summary>🎯 <strong>Strategic Thinking & OKR Alignment</strong> — Stay on strategy (3 skills)</summary>

| Skill | What It Does |
|-------|-------------|
| [okr-check](.claude/skills/okr-check/) | Score work against OKRs, detect drift |
| [decision](.claude/skills/decision/) | Formatted decision records |
| [solution-play-health](.claude/skills/solution-play-health/) | Strategic health scoring |
</details>

### ⏰ [Scheduled Tasks](scheduled-tasks/) — Automations for Copilot Studio

| When | What | Where |
|------|------|-------|
| Daily 9am | [Morning Pulse](scheduled-tasks/daily-morning-pulse.md) | 💬 Teams channel |
| Weekly Monday | [Board Health Check](scheduled-tasks/weekly-board-health.md) | 💬 Teams channel |
| Weekly Friday | [Friday Digest](scheduled-tasks/weekly-friday-digest.md) | 💬 Teams channel |
| Monthly 8th | [MBR Data Package](scheduled-tasks/monthly-mbr-package.md) | 📁 SharePoint |
| On new issue | [Auto-Triage](scheduled-tasks/new-issue-auto-triage.md) | 🏷️ GitHub comment |

### 🤖 [Agents](agents/) — Copilot Studio blueprints

| Agent | What It Does | Deployed To |
|-------|-------------|-------------|
| [Marketing Assistant](agents/marketing-assistant/) | Decks, emails, summaries, meeting prep | 👥 Teams (everyone) |
| [Strategy Advisor](agents/strategy-advisor/) | Competitive intel, pressure testing, OKR alignment | 👤 Teams (leads) |
| [Program Tracker](agents/program-tracker/) | Events, hackathons, budgets, countdowns | 📋 Teams (program owners) |
| [Reporting Engine](agents/reporting-engine/) | Standups, weekly status, MBR, 1:1 prep | 📊 Teams (team leads) |

---

## 📚 Going deeper

| Topic | Guide |
|-------|-------|
| 🤖 Build a Copilot Studio agent | [How to Build an Agent](copilot-studio/HOW-TO-BUILD-AN-AGENT.md) |
| 🚀 Deploy agents to your team | [Deployment Guide](copilot-studio/DEPLOYMENT-GUIDE.md) |
| 📏 Agent creation rules | [Agent Rules](AGENT-RULES.md) |
| 🔒 Security & compliance | [Security Overview](copilot-studio/SECURITY-OVERVIEW.md) |
| 📋 Agent instructions (copy-paste) | [Agent Instructions](copilot-studio/agent-instructions.md) |

<details>
<summary>🔌 <strong>Live data connectors</strong> (advanced — may need IT support)</summary>

These connectors let AI agents pull real-time data from your systems. Ask your IT admin to help set these up.

| Connector | What It Does | More Info |
|-----------|-------------|-----------|
| Microsoft Learn | Search official Microsoft documentation | [Setup guide](https://www.npmjs.com/package/@anthropic/microsoft-learn-mcp) |
| Work IQ (M365) | Access Teams, Calendar, Mail, SharePoint | [Microsoft docs](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) |
| GitHub | Pull project data — issues, boards, status | [GitHub docs](https://github.com/github/github-mcp-server) |
| Dataverse | Query business data and CRM records | [Microsoft docs](https://github.com/microsoft/Dataverse-MCP) |
| Power BI | Pull dashboard metrics and charts | [Microsoft docs](https://github.com/microsoft/powerbi-modeling-mcp) |

</details>

---

## ❓ FAQ

**Do I need to be technical?** No. Start with the [Prompts folder](prompts/) — just copy, paste, and go.

**Do I need GitHub?** Only for the reporting and analytics skills. Everything else works without it.

**What AI tool do I need?** Any. Copilot Chat, Claude, ChatGPT — whatever your team uses.

**How do I contribute?** Suggestions and improvements welcome. Open an issue or reach out to the team.

---

*🤖 Built by Victor, AI Chief of Staff. Questions? Open an issue or reach out.*
