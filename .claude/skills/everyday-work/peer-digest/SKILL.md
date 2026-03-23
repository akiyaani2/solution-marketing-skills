---
name: peer-digest
description: Generates a Friday cross-team digest for peer solution play leads. Follows the Solutions & Partner Marketing Weekly Wrap-up format — what changed, what's blocked, what's next, links. Dual-output: Teams post + email-ready version.
allowed-tools: [Bash, Read, Grep, Glob]
---

# Peer Digest Generator

**Type:** Cross-Team Visibility
**Speed:** ~15 seconds
**Cadence:** Weekly (Fridays)
**Output:** Teams channel post + email version

## What This Skill Does

Generates a concise weekly update for peer leads on adjacent teams. Follows the format used in Microsoft Solutions & Partner Marketing — specifically the "Weekly Wrap-up" pattern: curated index of what moved, what's blocked, what's coming, and links for fast navigation.

Sections:
1. **What Changed This Week** — Progress + decisions (1–3 items)
2. **What's Coming Next Week** — Upcoming milestones (1–3 items)
3. **Cross-Team Heads-up** — Anything touching peer teams, shared dependencies
4. **Blockers / Asks** — What you need and from whom
5. **Links** — Recordings, decks, trackers (the fast-nav index)

## Triggers

```
/peer-digest            # Generate this week's digest
/peer-digest --email    # Email-ready format (for forwarding)
/peer-digest --brief    # 5-bullet version for a quick Teams post
```

## Data Sources

| Source | What It Contains | Use |
|--------|-----------------|-----|
| **SharePoint Lists / MS Lists** | Initiative status, completions, blockers | Primary — pull completed/at-risk items |
| **Planner / Tasks** | Cross-team task completions and upcoming milestones | Primary |
| **Teams channel** | Prior week's posts, meeting outcomes shared in channel | Context scan for qualitative items |
| **Calendar / meetings** | Meetings held this week with cross-team attendance | Source for "What changed" (decisions made) |
| **Manual input** | User provides highlights | Fallback |

> **Note:** If the team tracks cross-solution work with GitHub labels (e.g., `sp:cross-solution`), substitute GitHub queries filtered by those labels. GitHub is optional.

## Implementation

### Step 1: Determine Cross-Team Scope

Define which items are "peer-facing" vs. internal-only:
- Items with cross-team dependencies or shared owners
- Decisions made this week that affect adjacent teams
- Blockers requiring peer team action
- Upcoming milestones where peer teams have visibility or dependencies

### Step 2: Gather Per-Section Data

**What Changed:**
- Completed deliverables with external impact (shipped content, launched events, finalized decisions)
- Decisions made in cross-team meetings
- Status changes on shared initiatives

**What's Coming:**
- Milestones due next week that peers should know about
- Upcoming events, launches, or reviews requiring cross-team awareness

**Cross-Team Heads-up:**
- Dependencies on peer teams (what you need from them)
- Items where your team's work affects their roadmap
- Shared program updates (NVIDIA, GitHub, Build-related work)

**Blockers / Asks:**
- Specific asks from peer teams with owner and deadline
- Escalations that need peer visibility

**Links:**
- Meeting recordings from cross-team syncs this week
- Decks or documents referenced in the digest
- The team's initiative tracker (SharePoint List or Loop table link)

### Step 3: Format Output

**Teams post format:**

```markdown
📋 **Weekly Wrap-up — [Team Name] — Week of [Date]**

**What Changed**
- ✅ [Completed item or decision — outcome framing]
- ✅ [Completed item or decision]

**What's Coming Next Week**
- 📅 [Milestone / launch / event] — [date] — [owner]
- 📅 [Milestone]

**Cross-Team Heads-up**
- [Dependency or shared item] — touches @[peer team/lead]
- [Update relevant to adjacent teams]

**Blockers / Asks**
- Need [specific thing] from @[team/person] by [date]
- [Or: No asks this week]

**Links**
- [Meeting recap / recording]
- [Deck or doc]
- [Initiative tracker]
```

**Email format (`--email`):**

Same structure, plain text with hyperlinks. Subject line: `[Team Name] Weekly Wrap-up — [Month DD]`

**Brief format (`--brief`):**

```
[Team Name] | Week of [Date]
✅ Shipped: [item]
📅 Next: [item]
🔗 Asks: [item or None]
[Link to full tracker]
```

## Dual-Publish Pattern

Publish to both:
1. **Teams channel** — short, scannable post in the cross-solution or shared channel
2. **Email** — for discoverability and forwarding to leaders not in the channel

Keep the **source of truth** as the initiative tracker (SharePoint List or Loop table) so the digest links back to live data rather than being a static artifact.

## Gotchas

- **Peer digest is not your internal status.** Don't include internal team details, budget specifics, or pre-announcement strategy. Filter to cross-team-relevant items only.
- **Links are the most valuable part.** The Teams Wrap-up format that works in your org is a curated index. Prioritize fast navigation over prose.
- **Brevity over completeness.** Peer leads get digests from multiple teams. 5 bullets and 3 links is better than 15 bullets.
- **Decisions made in meetings often don't appear in trackers.** Scan the Teams channel and calendar for meeting outcomes that should be included.

## Related Skills

- `/weekly-status` — Internal version of the same week's data
- `/standup` — Daily signals that feed into the Friday digest
- `/stakeholder-brief` — Longer-form brief for stakeholder escalation
