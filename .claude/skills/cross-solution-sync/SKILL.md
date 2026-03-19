---
name: cross-solution-sync
description: Cross-team coordination for matrix organizations — track shared workstreams, dependencies, joint events, and per-peer alignment. Generate sync prep notes and visibility reports for peer teams.
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/x-sp"
  - "/cross-sync"
  - "/x-sp --peer-name"
  - "cross-solution status"
  - "what do peer teams need to know"
---

# Cross-Solution Sync

Structured coordination layer for teams operating in a matrix organization. Tracks shared workstreams, inter-team dependencies, joint events, and provides per-peer views showing what you need from them and what they need from you.

## Trigger Phrases

- `/x-sp` — Full cross-team coordination dashboard
- `/cross-sync` — Same as above (alias)
- `/x-sp --[peer-name]` — Focused view for a specific peer team/person
- `/x-sp --portfolio` — Cross-team portfolio board view
- `/x-sp --prep [peer-name]` — Generate sync prep notes for upcoming meeting
- `/x-sp --digest` — Generate peer visibility digest (e.g., weekly update)

## What This Skill Does

### 1. Cross-Team Dashboard (`/x-sp`)

Pull all cross-team issues and generate a coordination view.

```bash
# Cross-team labeled issues
gh issue list --state open --label "sp:cross-solution" --json number,title,labels,assignees --limit 100

# Issues tagged with peer team contributors
gh issue list --state open --label "contributor:[peer-label]" --json number,title,labels --limit 50
```

**Output format:**

```
## Cross-Team Status — [Date]

### Shared Workstreams
| # | Title | Teams Involved | Status | Next Action | Owner |
|---|-------|---------------|--------|-------------|-------|

### [Peer Team A] Items
| # | Title | Status | What They Need From Us | What We Need From Them |
|---|-------|--------|----------------------|----------------------|

### [Peer Team B] Items
| # | Title | Status | What They Need From Us | What We Need From Them |
|---|-------|--------|----------------------|----------------------|

### Joint Events
| Event | Date | Our Role | Their Role | Shared Tasks |
|-------|------|---------|-----------|-------------|

### Cross-Team Dependencies
| Our Item | Depends On | Owning Team | Status | Risk |
|----------|-----------|-------------|--------|------|

### Blocked by External Teams
| # | Title | Blocked By | Days Blocked | Escalation Needed? |
|---|-------|-----------|-------------|-------------------|
```

### 2. Per-Peer View (`/x-sp --[peer-name]`)

Focused view for a specific peer team or person:

```bash
# Their labeled items
gh issue list --state open --label "contributor:[peer-label]" --json number,title,labels,assignees,body --limit 50

# Cross-solution items involving them
gh issue list --state open --label "sp:cross-solution" --json number,title,labels,body --limit 100 | jq '[.[] | select(.labels[].name | contains("[peer-label]"))]'
```

**Output format:**

```
## [Peer Name / Team] — Coordination View

### Open Items That Touch Both Teams
| # | Title | Status | Our Owner | Their Owner |
|---|-------|--------|-----------|-------------|

### What They Are Waiting On From Us
| # | Title | Due | Our Owner | Days Waiting |
|---|-------|-----|-----------|-------------|

### What We Are Waiting On From Them
| # | Title | Due | Status | Days Waiting |
|---|-------|-----|--------|-------------|

### Upcoming Shared Deadlines
| Date | Item | Joint Action Needed |
|------|------|-------------------|

### Suggested Talking Points for Next Sync
1. [item requiring alignment or decision]
2. [item we should proactively share]
3. [item where we need their input]
4. [FYI that affects their work]
```

### 3. Cross-Team Portfolio View (`/x-sp --portfolio`)

If you have a cross-team project board, pull its current state:

```bash
# Pull from cross-team project board (replace with your project ID)
gh api graphql -f query='query {
  node(id: "[YOUR_PROJECT_V2_ID]") {
    ... on ProjectV2 {
      title
      items(first: 100) {
        nodes {
          content {
            ... on Issue {
              number
              title
              state
              labels(first: 10) { nodes { name } }
              assignees(first: 5) { nodes { login } }
            }
          }
          fieldValues(first: 10) {
            nodes {
              ... on ProjectV2ItemFieldSingleSelectValue { name field { ... on ProjectV2SingleSelectField { name } } }
            }
          }
        }
      }
    }
  }
}'
```

**Output format:**

```
## Cross-Team Portfolio — [Date]

### By Status
| Status | Count | Items |
|--------|-------|-------|
| On Track | [N] | #1, #2, #3 |
| Needs Attention | [N] | #4, #5 |
| Blocked | [N] | #6 |

### By Team
| Team | Items | On Track | At Risk | Blocked |
|------|-------|----------|---------|---------|
| [Team A] | [N] | [N] | [N] | [N] |
| [Team B] | [N] | [N] | [N] | [N] |
| [Team C] | [N] | [N] | [N] | [N] |
```

### 4. Sync Prep Notes (`/x-sp --prep [peer-name]`)

Generate prep notes for an upcoming cross-team sync meeting:

```
## Sync Prep — [Your Team] + [Peer Team]
Date: [next sync date]

### Changes Since Last Sync
- [items completed, moved, or unblocked since last meeting]
- [new cross-team items created]
- [scope or timeline changes]

### Items Needing Their Input
| # | Title | What We Need | Priority |
|---|-------|-------------|----------|
| [N] | [title] | [specific ask] | [P0/P1/P2] |

### Items They Asked About Last Time
| # | Title | Current Status | Update |
|---|-------|---------------|--------|

### New Risks or Opportunities
- [cross-team risk that surfaced since last sync]
- [opportunity for collaboration]

### Recommended Ask / Offer
- **Ask**: [what we need from them]
- **Offer**: [what we can share or help with]

### Agenda Suggestion
1. [5 min] Status updates on shared items
2. [5 min] Review blocked items
3. [5 min] New items / requests
4. [5 min] Upcoming deadlines and alignment
```

### 5. Peer Visibility Digest (`/x-sp --digest`)

Generate a formatted update suitable for sharing with peer teams (via email, Teams, Slack):

```
## [Your Team] Update — Week of [Date]

### Completed This Week
- [item] — impact on shared workstreams: [note]

### In Progress
- [item] — ETA: [date] — peer teams affected: [teams]

### Blocked
- [item] — blocked by: [team/dependency] — help needed: [specific ask]

### Upcoming (Next 2 Weeks)
- [item] — peer teams should know: [context]

### Heads Up
- [proactive flag about something that may affect peer teams]

---
*[Your team name] | [Date] | [Reporting period]*
```

## Peer Relationship Configuration

Configure your peer teams by maintaining labels or a configuration reference:

| Peer | Label | Touchpoints | Sync Cadence |
|------|-------|------------|-------------|
| [Peer A name] | `contributor:[label-a]` | [shared workstreams] | Weekly |
| [Peer B name] | `contributor:[label-b]` | [shared workstreams] | Biweekly |
| [Peer C name] | `contributor:[label-c]` | [shared workstreams] | Monthly |

## Gotchas

- **Peer teams are real people, not automation targets.** Never auto-send communications to peer teams. This skill prepares drafts and generates visibility — a human must review and send all cross-team communications.
- **Cross-team commitments require your team lead's judgment.** Never auto-commit resources, timelines, or deliverables on behalf of your team. Surface the request and let the team lead decide.
- **Label conventions must be consistent.** If you use `contributor:heather` but someone else tags issues with `team:data-platform`, your queries will miss items. Standardize and document your cross-team label scheme.
- **Dependencies are often implicit, not labeled.** Two teams may have a dependency that is not captured in any issue label — it lives in someone's head or a meeting note. During sync prep, explicitly ask: "Are there dependencies we haven't tracked?"
- **Peer digests should be concise and action-oriented.** Peers do not want a full dump of your team's status. They want: (1) what affects them, (2) what they need to do, (3) what is coming next. Ruthlessly filter to only cross-team-relevant items.
- **Sync meetings are expensive.** If the prep reveals no open items, no blockers, and no upcoming shared deadlines, recommend canceling the sync or making it async. Do not hold meetings for the sake of the cadence.
- **Matrix organizations have invisible information flows.** Your peer may share information with your shared manager before telling you. Check for recent decisions or direction changes before generating sync prep, so you are not blindsided.
- **Cross-team project boards can have duplicate entries.** An issue may appear in your board AND the cross-team board as separate items. When updating status, make sure both are in sync.
