# Solution Marketing Skills for Claude Code

A curated collection of Claude Code skills for Microsoft Solution Marketing teams that manage work through GitHub.

Built from real operational patterns running a 4-person team managing 175+ issues, 30+ epics, 5 concurrent hackathon programs, monthly MBRs, and cross-solution coordination — all through GitHub Issues and Project boards.

## Who This Is For

- **Solution Play Leads** managing skilling, events, hackathons, and content programs
- **Marketing Managers** coordinating across vendors, partners, and cross-org stakeholders
- **Team Leads** using GitHub as their primary work management platform
- **Anyone at Microsoft** who wants AI-assisted project management for marketing operations

## Quick Start

### Option 1: Copy individual skills

Browse the `.claude/skills/` directory and copy any skill folder into your repo's `.claude/skills/` directory.

### Option 2: Clone the full set

```bash
# Clone this repo
git clone https://github.com/akiyaani2/solution-marketing-skills.git

# Copy all skills to your repo
cp -r solution-marketing-skills/.claude/skills/* your-repo/.claude/skills/
```

### Option 3: Reference as a submodule

```bash
git submodule add https://github.com/akiyaani2/solution-marketing-skills.git .claude/shared-skills
```

## Skills Overview

### Reporting & Communication (6 skills)

| Skill | Command | What It Does |
|-------|---------|-------------|
| **standup** | `/standup` | Daily async standup from GitHub activity — yesterday/today/blockers per person |
| **weekly-status** | `/weekly-status [name]` | Weekly status per team member from project boards |
| **mbr** | `/mbr` | Monthly Business Review package — metrics, narrative, wins, risks |
| **peer-digest** | `/peer-digest` | Cross-team Friday update for peer solution play leads |
| **1on1-prep** | `/1on1-prep [name]` | 1:1 meeting prep with open issues, blockers, FIRE framework |
| **escalation-tracker** | `/escalations` | Track items escalated to leadership with SLA monitoring |

### Project Management (5 skills)

| Skill | Command | What It Does |
|-------|---------|-------------|
| **epic-health** | `/epic-health` | Score epics on completion %, blocked items, staleness, RAPID |
| **issue-triage** | `/issue-triage` | Bulk triage — P0 cascade, stale detection, orphan reparenting, label hygiene |
| **solution-play-health** | `/sp-health` | Score solution plays on strategic health (layer above epic-health) |
| **event-countdown** | `/event-countdown` | Days until key events with readiness percentages |
| **decision** | `/decision "description"` | Create formatted decision records (TD-NNNN) with RAPID roles |

### Events & Programs (3 skills)

| Skill | Command | What It Does |
|-------|---------|-------------|
| **event-ops** | `/event-ops` | Full event lifecycle — workback schedules, vendor checklists, readiness scoring |
| **hackathon-tracker** | `/hack-status` | Track hackathons end-to-end — registration, judging, outcomes, cross-hack comparison |
| **content-pipeline** | `/content-pipeline` | Content production tracking — blogs, episodes, abstracts, social, competitive intel |

### Operations (4 skills)

| Skill | Command | What It Does |
|-------|---------|-------------|
| **budget-ops** | `/budget-ops` | PO tracking, MDF utilization, vendor management, budget reports |
| **cross-solution-sync** | `/x-sp` | Cross-solution coordination — shared workstreams, dependencies, joint events |
| **meeting-notes** | `/meeting-notes` | Capture notes, auto-extract action items, create GitHub Issues |
| **git-sync** | `/git-sync` | Automated git pull/commit/push with standardized messages |

## How These Skills Work Together

```
Daily:    /standup → quick team pulse
Weekly:   /weekly-status → per-person board view
          /issue-triage → board hygiene
          /epic-health → initiative health
          /x-sp → cross-team coordination
Biweekly: /1on1-prep → meeting prep
Monthly:  /mbr → leadership package
          /budget-ops → financial status
          /sp-health → strategic view
Friday:   /peer-digest → cross-team update
Events:   /event-ops → logistics management
          /hack-status → hackathon pipeline
          /event-countdown → readiness check
Ad hoc:   /meeting-notes → capture & extract
          /escalations → track upward items
          /decision → document decisions
          /content-pipeline → content status
```

## Customization

Each skill is a self-contained folder with a `SKILL.md` file. To customize for your team:

1. **Labels** — Update label prefixes (`owner:`, `func:`, `sp:`, `status:`) to match your repo's conventions
2. **Team members** — Replace team member names and roles in skill templates
3. **Event names** — Update event labels and known events for your portfolio
4. **Decision rights** — Adjust approval chains and escalation paths
5. **Reporting format** — Modify output templates to match your stakeholder preferences

## Prerequisites

- A GitHub repo with Issues enabled
- GitHub CLI (`gh`) installed and authenticated
- Claude Code CLI with access to your repo
- Labels configured for ownership, function, priority, and status

### Recommended Label Structure

```
owner:[name]          — Primary owner (your team members)
func:[area]           — Function area (events, hackathon, content, gtm, ops)
sp:[play]             — Solution play (your plays)
status:[state]        — Workflow status (blocked, at-risk, waiting-external)
type:[type]           — Issue type (initiative, task, decision-needed)
stakeholder:[name]    — Stakeholder visibility
event:[name]          — Specific events
p0 / p1 / p2          — Priority levels
```

## Contributing

These skills are built from real operational patterns. If you find improvements or build new skills for your team, PRs are welcome.

## Credits

Built by the AI Apps & Agents Skilling team using Claude Code. Skills are based on 3 months of operational experience managing a Microsoft marketing team through GitHub.

---

*For questions or feedback, reach out to Aaron Stark.*
