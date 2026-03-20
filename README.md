# 🚀 Solution Marketing Skills

AI-powered toolkit for Solution and Partner Marketing teams. Prompts, skills, automations, and Copilot Studio agent blueprints — ready to use.

Works with **Copilot**, **Claude**, **ChatGPT**, and any AI assistant. No technical setup required.

---

## 🔍 What do you want to do?

| I want to... | Go here |
|-------------|---------|
| 📊 **Build a deck or presentation** | [deck-builder](.claude/skills/everyday-work/deck-builder/) or [doc-to-slides](prompts/doc-to-slides.md) |
| 🎯 **Prep for a meeting** | [talking-points](.claude/skills/everyday-work/talking-points/) or [1on1-prep](.claude/skills/everyday-work/1on1-prep/) |
| ✉️ **Draft an email** | [email-drafter](.claude/skills/everyday-work/email-drafter/) or [recap-email](prompts/recap-email.md) |
| 📈 **Analyze data or a spreadsheet** | [excel-analyzer](.claude/skills/everyday-work/excel-analyzer/) or [data-to-insights](prompts/data-to-insights.md) |
| 📝 **Summarize something for leadership** | [exec-summary](.claude/skills/everyday-work/exec-summary/) |
| 📄 **Create a one-pager or brief** | [one-pager](.claude/skills/everyday-work/one-pager/) |
| ✍️ **Write a blog post** | [blog-from-bullets](prompts/blog-from-bullets.md) |
| 📱 **Write social media posts** | [social-from-content](prompts/social-from-content.md) |
| 📣 **Write a weekly leadership update** | [leadership-update](.claude/skills/everyday-work/leadership-update/) |
| 🔥 **Challenge or pressure-test a plan** | [pressure-test](.claude/skills/strategy-and-challenge/pressure-test/) or [pre-mortem](.claude/skills/strategy-and-challenge/pre-mortem/) |
| 🥊 **Think like the competition** | [red-team](.claude/skills/strategy-and-challenge/red-team/) or [competitive-scan](.claude/skills/strategy-and-challenge/competitive-scan/) |
| 🎯 **Check if work aligns to OKRs** | [okr-check](.claude/skills/strategy-and-challenge/okr-check/) |
| 📅 **Track events or hackathons** | [event-ops](.claude/skills/program-operations/event-ops/) or [hackathon-tracker](.claude/skills/program-operations/hackathon-tracker/) |
| ⚙️ **Set up automated reports in Teams** | [scheduled-tasks/](scheduled-tasks/) |
| 🤖 **Build a Copilot Studio agent** | [How to Build an Agent](copilot-studio/HOW-TO-BUILD-AN-AGENT.md) |

---

## 💡 Prompts vs Skills — When to use which

This repo has **prompts** and **skills**. They solve different problems.

### 📝 Prompts — for one-off tasks

A prompt is a single instruction. You paste it into Copilot or Claude, describe what you need, and get a result. It's like texting a really smart colleague — you ask one question, you get one answer.

**Use prompts when:** You need something done once. A blog post, a recap email, a slide outline. You won't need to do this exact thing the same way next week.

**Example:** You paste the [blog-from-bullets](prompts/blog-from-bullets.md) prompt, add your bullet points, and get a polished blog draft back.

### ⚡ Skills — for repeatable workflows

A skill is a full playbook. Instead of a single question-and-answer, the AI follows a multi-step process — gathering input, making decisions, applying quality checks, and producing structured output. Think of it like handing someone a runbook, not just a question.

**Use skills when:** You do the same type of work regularly and want consistent, high-quality results every time. Weekly leadership updates. Monthly business reviews. Meeting prep before every 1:1.

**Example:** The [leadership-update](.claude/skills/everyday-work/leadership-update/) skill doesn't just write an update — it first filters your raw notes through an altitude check (is this strategic enough for leadership?), then routes each bullet to the strongest framing pattern (competitive, metric-led, or strategic narrative), then assembles the update in the approved format, and finally checks for 7 common failure modes before you submit.

**One prompt = one question, one answer. One skill = a trained process that runs the same way every time.**

> 📖 [What are skills?](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) · [Best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) — from Anthropic

---

## ⚡ How to use

**Prompts** (30 seconds) — Open any file in [`/prompts/`](prompts/), copy, paste into Copilot or Claude, done.

**Skills** (2 minutes) — Open any skill, copy the content, paste as instructions, describe your task.

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

10 prompts. Open one, copy, paste, go.

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

### ⚡ Skills — 34 skills in 3 folders

Skills are organized by how you use them. Browse the folder that matches your work.

#### 💼 [Everyday Work](.claude/skills/everyday-work/) — 17 skills

Presentations, emails, meeting prep, summaries, data analysis, reporting, and content.

| Skill | What It Does |
|-------|-------------|
| [deck-builder](.claude/skills/everyday-work/deck-builder/) | Slide outlines from bullet points or data |
| [exec-summary](.claude/skills/everyday-work/exec-summary/) | Distills any document into a leadership-ready summary |
| [email-drafter](.claude/skills/everyday-work/email-drafter/) | Professional emails calibrated to recipient and situation |
| [one-pager](.claude/skills/everyday-work/one-pager/) | Single-page program brief |
| [talking-points](.claude/skills/everyday-work/talking-points/) | Meeting prep with Q&A and landmines |
| [excel-analyzer](.claude/skills/everyday-work/excel-analyzer/) | Spreadsheet analysis — trends, anomalies, insights |
| [stakeholder-brief](.claude/skills/everyday-work/stakeholder-brief/) | Same update reframed for multiple audiences |
| [data-to-slides](.claude/skills/everyday-work/data-to-slides/) | Raw data into presentation content |
| [leadership-update](.claude/skills/everyday-work/leadership-update/) | Weekly exec update with altitude check and strategic framing |
| [meeting-notes](.claude/skills/everyday-work/meeting-notes/) | Capture notes and extract action items |
| [1on1-prep](.claude/skills/everyday-work/1on1-prep/) | 1:1 meeting prep with FIRE framework |
| [mbr](.claude/skills/everyday-work/mbr/) | Monthly Business Review package |
| [peer-digest](.claude/skills/everyday-work/peer-digest/) | Friday cross-team update (<10 lines) |
| [standup](.claude/skills/everyday-work/standup/) | Daily async standup |
| [weekly-status](.claude/skills/everyday-work/weekly-status/) | Weekly status per person |
| [escalation-tracker](.claude/skills/everyday-work/escalation-tracker/) | Track items sent to leadership |
| [content-pipeline](.claude/skills/everyday-work/content-pipeline/) | Content production pipeline |

#### 🔥 [Strategy & Challenge](.claude/skills/strategy-and-challenge/) — 10 skills

Competitive intel, pressure testing, decision records, OKR alignment, and strategic health.

| Skill | What It Does |
|-------|-------------|
| [pressure-test](.claude/skills/strategy-and-challenge/pressure-test/) | Challenge plans through 7 lenses |
| [pre-mortem](.claude/skills/strategy-and-challenge/pre-mortem/) | "It failed. Why?" — reason backwards from failure |
| [red-team](.claude/skills/strategy-and-challenge/red-team/) | Think like the competition |
| [competitive-scan](.claude/skills/strategy-and-challenge/competitive-scan/) | AWS/GCP/partner landscape scanning |
| [takeshi-tuesday](.claude/skills/strategy-and-challenge/takeshi-tuesday/) | Weekly market insights with persistent memory |
| [okr-check](.claude/skills/strategy-and-challenge/okr-check/) | Score work against OKRs, detect drift |
| [decision](.claude/skills/strategy-and-challenge/decision/) | Formatted decision records |
| [solution-play-health](.claude/skills/strategy-and-challenge/solution-play-health/) | Strategic health scoring across plays |
| [cross-solution-sync](.claude/skills/strategy-and-challenge/cross-solution-sync/) | Cross-team coordination |
| [post-mortem](.claude/skills/strategy-and-challenge/post-mortem/) | After-action review with persistent log |

#### 📋 [Program Operations](.claude/skills/program-operations/) — 7 skills

Event logistics, hackathon pipelines, budgets, project health, and board management.

| Skill | What It Does |
|-------|-------------|
| [event-ops](.claude/skills/program-operations/event-ops/) | Event lifecycle — workback schedules, vendor checklists, readiness |
| [hackathon-tracker](.claude/skills/program-operations/hackathon-tracker/) | Hackathon pipeline — registration, judging, outcomes |
| [event-countdown](.claude/skills/program-operations/event-countdown/) | Event readiness dashboard with urgency tiers |
| [budget-ops](.claude/skills/program-operations/budget-ops/) | PO tracking, co-marketing funds, budget reports |
| [epic-health](.claude/skills/program-operations/epic-health/) | Score initiatives on 5 health dimensions |
| [issue-triage](.claude/skills/program-operations/issue-triage/) | Bulk triage with priority cascade pattern |
| [git-sync](.claude/skills/program-operations/git-sync/) | Automated project sync |

---

### ⏰ [Scheduled Tasks](scheduled-tasks/) — Automations for Copilot Studio

Set up once, runs on autopilot.

| When | What | Where |
|------|------|-------|
| Daily 9am | [Morning Pulse](scheduled-tasks/daily-morning-pulse.md) | 💬 Teams |
| Weekly Monday | [Board Health Check](scheduled-tasks/weekly-board-health.md) | 💬 Teams |
| Weekly Friday | [Friday Digest](scheduled-tasks/weekly-friday-digest.md) | 💬 Teams |
| Monthly 8th | [MBR Data Package](scheduled-tasks/monthly-mbr-package.md) | 📁 SharePoint |
| On new item | [Auto-Triage](scheduled-tasks/new-issue-auto-triage.md) | 🏷️ Auto-label |

### 🤖 [Agents](agents/) — Copilot Studio blueprints

4 agents ready to deploy to Teams.

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
| Microsoft Learn | Official Microsoft docs | [Setup](https://www.npmjs.com/package/@anthropic/microsoft-learn-mcp) |
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

---

*🤖 Built by Victor, AI Chief of Staff. Questions? Open an issue or reach out.*
