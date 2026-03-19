# Solution Marketing Skills

**18 production-ready AI skills for marketing teams that run on GitHub.**

Built from 3 months of real operations managing 175+ issues, 30+ epics, 5 concurrent hackathon programs, monthly MBRs, and cross-solution coordination — all through GitHub Issues and Project boards.

These skills work with **Claude Code**, **GitHub Copilot** (coding agent & chat), **Cursor**, **Codex CLI**, and any platform that supports the [Agent Skills](https://agentskills.io) open standard.

---

## Table of Contents

- [Quick Start](#quick-start)
- [Skills by Category](#skills-by-category)
  - [Reporting & Communication](#reporting--communication)
  - [Project Management](#project-management)
  - [Events & Programs](#events--programs)
  - [Operations](#operations)
- [How Skills Work Together](#how-skills-work-together)
- [Using Skills with GitHub Copilot](#using-skills-with-github-copilot)
- [Using Skills with Copilot Studio Agents](#using-skills-with-copilot-studio-agents)
- [MCP Servers That Extend These Skills](#mcp-servers-that-extend-these-skills)
- [The Agent Skills Standard](#the-agent-skills-standard)
- [Setting Up Your Repo](#setting-up-your-repo)
- [Customization Guide](#customization-guide)
- [FAQ](#faq)

---

## Quick Start

### Option 1 — Copy what you need

Browse the [`skills/`](.claude/skills/) directory. Copy any skill folder into your repo:

```bash
# Copy one skill
cp -r solution-marketing-skills/.claude/skills/standup your-repo/.claude/skills/

# Copy all skills
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/
```

### Option 2 — Clone the full collection

```bash
git clone https://github.com/akiyaani2/solution-marketing-skills.git
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/
```

### Option 3 — Add as a Git submodule

```bash
git submodule add https://github.com/akiyaani2/solution-marketing-skills.git .claude/shared-skills
```

### Option 4 — Use with Copilot

Copy skills to `.github/skills/` instead of `.claude/skills/`:

```bash
mkdir -p your-repo/.github/skills
cp -r solution-marketing-skills/.claude/skills/* your-repo/.github/skills/
```

Copilot also reads `.claude/skills/` automatically — either location works.

---

## Skills by Category

### Reporting & Communication

6 skills for team visibility, leadership reporting, and cross-team coordination.

| Skill | Command | What It Does | Details |
|-------|---------|-------------|---------|
| [**standup**](.claude/skills/standup/) | `/standup` | Daily async standup from GitHub activity | Yesterday / today / blockers per person. Supports `--brief` for one-liners. |
| [**weekly-status**](.claude/skills/weekly-status/) | `/weekly-status [name]` | Weekly status per team member | Completed, in-progress, blocked, coming up. Roll-up mode for team leads. |
| [**mbr**](.claude/skills/mbr/) | `/mbr` | Monthly Business Review package | Executive summary, owner breakdowns, metrics, narrative template. Ready for leadership. |
| [**peer-digest**](.claude/skills/peer-digest/) | `/peer-digest` | Friday cross-team update | What shipped, what's coming, cross-team heads up. Under 10 lines. |
| [**1on1-prep**](.claude/skills/1on1-prep/) | `/1on1-prep [name]` | 1:1 meeting prep | Open issues, blockers, recent wins, FIRE framework (Feedback, Ideas, Requests, Expectations). |
| [**escalation-tracker**](.claude/skills/escalation-tracker/) | `/escalations` | Track leadership escalations | Pending / responded / resolved with SLA monitoring. Configurable by leadership tier. |

### Project Management

5 skills for board hygiene, initiative health, and decision governance.

| Skill | Command | What It Does | Details |
|-------|---------|-------------|---------|
| [**epic-health**](.claude/skills/epic-health/) | `/epic-health` | Score epics on 5 health dimensions | Completion %, blocked count, staleness, RAPID, TBD count. Weighted scoring. |
| [**issue-triage**](.claude/skills/issue-triage/) | `/issue-triage` | Bulk triage with P0 cascade | Stale detection, priority downgrade pattern, orphan reparenting, label hygiene. |
| [**solution-play-health**](.claude/skills/solution-play-health/) | `/sp-health` | Strategic health scoring | Layer above epic-health. Scores solution plays across multiple dimensions. |
| [**event-countdown**](.claude/skills/event-countdown/) | `/event-countdown` | Event readiness dashboard | Three urgency tiers. Readiness % with color coding adjusted for time proximity. |
| [**decision**](.claude/skills/decision/) | `/decision "desc"` | Decision records (TD-NNNN) | Context, options, decision, RAPID roles, consequences. Auto-numbered. |

### Events & Programs

3 skills for event logistics, hackathon operations, and content production.

| Skill | Command | What It Does | Details |
|-------|---------|-------------|---------|
| [**event-ops**](.claude/skills/event-ops/) | `/event-ops` | Full event lifecycle management | Workback schedules, vendor checklists, readiness scoring, cross-event calendar. |
| [**hackathon-tracker**](.claude/skills/hackathon-tracker/) | `/hack-status` | Hackathon program tracking | Pipeline stages (planning → outcomes), registration funnels, cross-hack comparison. |
| [**content-pipeline**](.claude/skills/content-pipeline/) | `/content-pipeline` | Content production pipeline | Blog, video, social, session abstracts. Ideation → production → review → published. |

### Operations

4 skills for budget management, cross-team coordination, meeting capture, and git workflow.

| Skill | Command | What It Does | Details |
|-------|---------|-------------|---------|
| [**budget-ops**](.claude/skills/budget-ops/) | `/budget-ops` | Budget & vendor operations | PO lifecycle tracking, co-marketing fund utilization, vendor status, leadership reports. |
| [**cross-solution-sync**](.claude/skills/cross-solution-sync/) | `/x-sp` | Cross-team coordination | Shared workstreams, dependencies, joint events, sync prep. Goes deeper than peer-digest. |
| [**meeting-notes**](.claude/skills/meeting-notes/) | `/meeting-notes` | Meeting capture & action extraction | Auto-extract action items as GitHub Issues. Templates for 1:1s, syncs, recurring insights. |
| [**git-sync**](.claude/skills/git-sync/) | `/git-sync` | Automated git workflow | Pull, commit, push with standardized messages. Force-push prevention, conflict detection. |

---

## How Skills Work Together

Skills compose into a natural operating rhythm:

```
                    ┌─────────────────────────────────────────┐
                    │           OPERATING RHYTHM               │
                    └─────────────────────────────────────────┘

  DAILY             /standup ──────────────────► team pulse

  WEEKLY            /weekly-status ────────────► per-person view
                    /issue-triage ─────────────► board hygiene
                    /epic-health ──────────────► initiative health
                    /x-sp ─────────────────────► cross-team sync

  BIWEEKLY          /1on1-prep ────────────────► meeting prep

  MONTHLY           /mbr ──────────────────────► leadership package
                    /budget-ops ───────────────► financial status
                    /sp-health ────────────────► strategic view

  FRIDAY            /peer-digest ──────────────► cross-team update

  EVENTS            /event-ops ────────────────► logistics
                    /hack-status ──────────────► hackathon pipeline
                    /event-countdown ──────────► readiness check

  AD HOC            /meeting-notes ────────────► capture & extract
                    /escalations ──────────────► track upward items
                    /decision ─────────────────► document decisions
                    /content-pipeline ─────────► content status
```

**Composition chains:**
- `/standup` data feeds into `/weekly-status` which feeds into `/mbr`
- `/epic-health` aggregates into `/sp-health` which feeds MBR narrative
- `/1on1-prep` combines with `/meeting-notes` for a full meeting cycle
- `/event-countdown` feeds `/event-ops` for deeper logistics
- `/escalations` tracks items surfaced in any other skill

---

## Using Skills with GitHub Copilot

These skills are built on the [Agent Skills](https://agentskills.io) open standard, which is supported by **both Claude Code and GitHub Copilot**.

### With Copilot Coding Agent

The Copilot coding agent (GA for all paid Copilot plans) can use these skills autonomously:

1. Place skills in `.github/skills/` or `.claude/skills/` in your repo
2. Assign a GitHub Issue to Copilot (just like assigning to a team member)
3. Copilot reads the relevant skill and executes autonomously
4. A draft PR is opened with the results

**Example:** Create an issue titled "Generate this month's MBR package" and assign it to Copilot. The agent will find the `/mbr` skill, pull issue data via `gh` CLI, and generate the MBR as a markdown file in a PR.

### With Copilot Chat (VS Code / GitHub.com)

In Copilot Chat, reference skills by name:

```
@workspace Use the epic-health skill to score all our type:initiative epics
```

### With Copilot in the CLI

```bash
gh copilot suggest "use the standup skill to generate today's standup"
```

---

## Using Skills with Copilot Studio Agents

If your team has access to **Copilot Studio**, you can create custom agents that leverage these skills:

### Low-Code Agent Setup

1. **Create a new agent** in Copilot Studio
2. **Add knowledge sources** — point to this repo or upload individual SKILL.md files as knowledge
3. **Define topics** — map each skill to a Copilot Studio topic:
   - Topic: "Generate standup" → triggers standup skill instructions
   - Topic: "MBR prep" → triggers mbr skill instructions
   - Topic: "Triage board" → triggers issue-triage skill instructions
4. **Connect actions** — use Power Automate flows or MCP connectors to execute `gh` CLI commands
5. **Deploy to Teams** — team members invoke the agent in Microsoft Teams

### Pro-Code Agent Setup (Microsoft Foundry)

For deeper integration, use the [Microsoft Agent Framework](https://devblogs.microsoft.com/foundry/introducing-microsoft-agent-framework-the-open-source-engine-for-agentic-ai-apps/) to build agents that:
- Read SKILL.md files as instructions
- Use the GitHub MCP server for issue/board operations
- Use Work IQ MCP servers for Teams/Calendar/Mail integration
- Run autonomously on Azure compute

### Sharing Skills as Knowledge

Each SKILL.md file is a self-contained instruction set. You can:
- Upload them as **Copilot Studio knowledge articles**
- Reference them in **Copilot custom instructions** (`.github/copilot-instructions.md`)
- Embed them in **Power Platform AI Builder** prompts
- Share them as templates in your org's **Copilot Extensions** catalog

---

## MCP Servers That Extend These Skills

[Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers give AI agents real-time access to external tools and data. These servers pair well with the skills in this repo:

### Recommended MCP Servers

| Server | What It Does | Skills It Extends |
|--------|-------------|------------------|
| [**GitHub MCP Server**](https://github.com/github/github-mcp-server) | Full CRUD on issues, PRs, projects, discussions | All skills — this is the primary data source |
| [**GitHub Projects V2 MCP**](https://github.com/Arclio/github-projects-mcp) | Direct project board management via natural language | epic-health, issue-triage, weekly-status |
| [**Microsoft Learn MCP**](https://learn.microsoft.com/en-us/azure/) | Search and fetch official Microsoft documentation | content-pipeline (technical accuracy), research |
| [**Work IQ Calendar**](https://learn.microsoft.com/en-us/microsoft-agent-365/) | Create/update calendar events, suggest meeting times | event-ops (scheduling), meeting-notes |
| [**Work IQ Teams**](https://learn.microsoft.com/en-us/microsoft-agent-365/) | Post messages, create chats, channel operations | peer-digest (delivery), standup (distribution) |
| [**Work IQ Mail**](https://learn.microsoft.com/en-us/microsoft-agent-365/) | Send/search emails, reply-all | escalation-tracker (follow-ups), mbr (distribution) |
| [**Work IQ SharePoint**](https://learn.microsoft.com/en-us/microsoft-agent-365/) | Upload files, search, manage lists | budget-ops (PO docs), content-pipeline (assets) |

### Setting Up MCP Servers

For **Claude Code**, configure in `.claude/settings.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "ghcr.io/github/github-mcp-server"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>" }
    },
    "microsoft-learn": {
      "command": "npx",
      "args": ["-y", "@anthropic/microsoft-learn-mcp"]
    }
  }
}
```

For **Copilot**, MCP servers are configured at the organization level in your Copilot settings or via `.github/copilot-mcp.json`.

> **Note:** Microsoft Work IQ MCP servers require an M365 Copilot license and are currently in preview. Check [Work IQ documentation](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) for availability.

---

## The Agent Skills Standard

These skills follow the [Agent Skills](https://agentskills.io) open specification — a portable format for giving AI agents new capabilities.

### Why This Matters

The same SKILL.md file works across **26+ platforms**:

| Platform | Status | How It Uses Skills |
|----------|--------|--------------------|
| **Claude Code** | Supported | `.claude/skills/` directory |
| **GitHub Copilot** | Supported | `.github/skills/` or `.claude/skills/` |
| **VS Code** (agent mode) | Supported | Reads from either directory |
| **Cursor** | Supported | `.cursor/skills/` or `.claude/skills/` |
| **Codex CLI** (OpenAI) | Supported | Standard skill directories |
| **Gemini CLI** | Supported | Standard skill directories |
| **Copilot Studio** | Via knowledge upload | Upload SKILL.md as knowledge article |

### Skill File Structure

```
.claude/skills/my-skill/
  SKILL.md           # Required — YAML frontmatter + markdown instructions
  scripts/           # Optional — helper scripts the skill can reference
  references/        # Optional — detailed docs, API references
  assets/            # Optional — templates, sample files
```

### SKILL.md Format

```yaml
---
name: my-skill
description: One-line description of what this skill does and when to trigger it
---

# My Skill

Instructions for the AI agent...
```

The `description` field is what the AI reads to decide whether to activate the skill. Write it for the model, not for humans.

---

## Setting Up Your Repo

### Prerequisites

- A GitHub repo with Issues enabled
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated
- An AI coding tool (Claude Code, Copilot, Cursor, etc.)

### Recommended Label Structure

These skills expect a consistent label taxonomy. Configure these labels in your repo:

```
Priority:
  p0                    — Critical / blocking
  p1                    — High priority
  p2                    — Normal priority

Ownership:
  owner:[name]          — Primary owner (one per team member)

Function:
  func:events           — Event management
  func:hackathon        — Hackathon programs
  func:content          — Content production
  func:gtm              — Go-to-market
  func:ops              — Operations
  func:data             — Data & analytics
  func:partnership      — Partner coordination

Solution Play:
  sp:[play-name]        — Your solution plays
  sp:cross-solution     — Cross-play items

Status:
  status:blocked        — Blocked on something
  status:at-risk        — At risk of missing target
  status:waiting-external — Waiting on external party

Type:
  type:initiative       — Epic / strategic initiative
  type:task             — Individual work item
  type:decision-needed  — Requires a decision

Stakeholder:
  stakeholder:[name]    — Visibility for specific leaders

Event:
  event:[name]          — Specific event or program
```

### GitHub Project Board Setup

For maximum skill effectiveness, create project boards per team member:

1. **[Name] — [Domain]** board per person (e.g., "Mindy — Events & Partner Execution")
2. **Leadership View** board for initiative-level roll-up
3. **Events Calendar** board for timeline visualization
4. **Cross-Solution Portfolio** board for cross-team items

---

## Customization Guide

Every skill is designed to be customized. Here's how:

### 1. Labels

Search and replace label prefixes in SKILL.md files:

```bash
# If your team uses "area:" instead of "func:"
find .claude/skills -name "SKILL.md" -exec sed -i '' 's/func:/area:/g' {} +
```

### 2. Team Members

Replace placeholder names with your actual team:

```bash
# In 1on1-prep, weekly-status, standup, etc.
# Replace [Team Lead] with actual names
```

### 3. Event Portfolio

Update event labels and known events in `event-ops`, `event-countdown`, and `hackathon-tracker`.

### 4. Reporting Cadence

Adjust the operating rhythm in skills like `mbr` (monthly), `weekly-status` (weekly), `standup` (daily) to match your team's cadence.

### 5. Decision Rights

Update the escalation chains in `escalation-tracker` and `budget-ops` to match your org's approval hierarchy.

---

## FAQ

**Q: Do I need Claude Code to use these skills?**
No. These skills work with any tool that supports the [Agent Skills](https://agentskills.io) standard — including GitHub Copilot, Cursor, Codex CLI, and Gemini CLI.

**Q: Can I use these skills in Copilot Chat without the coding agent?**
Yes. Reference the skill content in your Copilot custom instructions (`.github/copilot-instructions.md`) or ask Copilot Chat to follow the instructions in a specific SKILL.md file.

**Q: Do I need all 18 skills?**
No. Start with what you need. Most teams begin with `/standup`, `/weekly-status`, and `/epic-health`, then add more as their workflow matures.

**Q: Can I contribute new skills?**
Yes. PRs are welcome. Follow the SKILL.md format documented above, keep skills generic (no team-specific names), and include a Gotchas section.

**Q: How do these skills interact with MCP servers?**
Skills provide the instructions (what to do). MCP servers provide the connections (how to access data). Together, they give AI agents both knowledge and capability. For example, the `/mbr` skill tells the agent how to structure a Monthly Business Review, and the GitHub MCP server gives it real-time access to your issues and project boards.

**Q: Can Copilot Studio agents use these skills?**
Yes. Upload SKILL.md files as knowledge articles in Copilot Studio, or point your agent's knowledge source to this repo. See [Using Skills with Copilot Studio Agents](#using-skills-with-copilot-studio-agents) for details.

---

## Contributing

These skills are built from real operational patterns. If you improve a skill or create a new one for your team, contributions are welcome:

1. Fork this repo
2. Create your skill in `.claude/skills/[skill-name]/SKILL.md`
3. Follow the [Agent Skills specification](https://agentskills.io/specification)
4. Include a Gotchas section with real operational tips
5. Keep it generic — no team-specific names or repo references
6. Submit a PR

---

## Resources

- [Agent Skills Specification](https://agentskills.io/specification) — The open standard these skills follow
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code) — Claude Code skill authoring guide
- [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) — Copilot's skill documentation
- [GitHub MCP Server](https://github.com/github/github-mcp-server) — Official GitHub MCP server
- [Microsoft Work IQ](https://learn.microsoft.com/en-us/microsoft-agent-365/tooling-servers-overview) — Enterprise MCP servers for M365
- [awesome-agent-skills](https://github.com/skillmatic-ai/awesome-agent-skills) — Community skill directory

---

*Built by the AI Apps & Agents Skilling team. Maintained by Victor (AI-CoS).*
*For questions or feedback, reach out to Aaron Stark.*
