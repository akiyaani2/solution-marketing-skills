---
name: peer-digest
description: Generate a Friday cross-team digest for peer solution play leads. Covers what shipped, what's coming, cross-team heads up, blockers, and team health -- all in under 10 lines.
---

# Peer Digest Generator

**Type:** Cross-Team Visibility
**Speed:** ~15 seconds
**Cadence:** Weekly (Fridays)

## What This Skill Does

Generates a concise weekly update for peer leads on adjacent teams. The goal is to maintain cross-team visibility without creating meetings or long email threads.

Sections:
1. **What Shipped This Week** -- 1-3 completed items
2. **What's Coming Next Week** -- 1-3 upcoming items
3. **Cross-Team Heads Up** -- Anything touching peer teams
4. **Blockers / At-Risk** -- Items needing attention
5. **Team Health** -- Green/Yellow/Red signal

## Triggers

```
/peer-digest            # Generate this week's digest
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use labels for cross-team tracking (e.g., `sp:cross-solution`, `sp:[play-name]`)
- Status labels (`status:blocked`, `status:at-risk`) for risk items
- Repository name set as `REPO` variable

## Implementation

### Step 1: Set Variables

```bash
REPO="[org]/[repo]"
WEEK_AGO=$(date -v-7d +%Y-%m-%d 2>/dev/null || date -d "7 days ago" +%Y-%m-%d)
TODAY=$(date +%Y-%m-%d)
```

### Step 2: Query Closed Issues This Week

```bash
gh issue list --repo "$REPO" \
  --state closed \
  --search "closed:>=${WEEK_AGO}" \
  --json number,title,labels \
  --limit 20
```

### Step 3: Query Cross-Team Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "sp:cross-solution" \
  --json number,title,labels \
  --limit 10
```

Adjust the label to match your cross-team tracking scheme. Some teams use `cross-team`, `x-solution`, or `contributor:[peer-name]`.

### Step 4: Query Blocked / At-Risk Items

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "status:blocked" \
  --json number,title,labels \
  --limit 5

gh issue list --repo "$REPO" \
  --state open \
  --label "status:at-risk" \
  --json number,title,labels \
  --limit 5
```

### Step 5: Determine Team Health

Calculate health signal from the data:
- **Green:** No blockers, completions on track, no overdue items
- **Yellow:** 1-2 blockers or at-risk items, or below-average completions
- **Red:** 3+ blockers, critical items overdue, or capacity issue

### Step 6: Select Top Items

Pick the 1-3 most impactful items for each section. Prioritize:
- Items that touch peer teams (cross-solution labels)
- P0/P1 items over P2
- Items with external dependencies
- Upcoming events or deadlines

## Output Format

```markdown
## Skilling Team Update -- Week of [Date]

**For:** [Peer Lead A], [Peer Lead B]
**From:** [Team Lead]

---

### 1. What Shipped This Week
- [Completed item 1]
- [Completed item 2]

### 2. What's Coming Next Week
- [Upcoming item 1]
- [Upcoming item 2]

### 3. Cross-Team Heads Up
- [Item affecting peer teams -- be specific about what they need to know]
- Or "None this week"

### 4. Blockers / At-Risk
- [Blocked item with context]
- Or "All clear"

### 5. Team Health
Green -- All systems go
-- OR --
Yellow -- Some attention needed: [brief reason]
-- OR --
Red -- Needs intervention: [brief reason]

---

*Questions? Ping [Team Lead] or reply to this thread.*
```

## Rules

- **Keep it under 10 lines of actual content.** This is a scan-and-move-on artifact, not a detailed report.
- **Always include "Cross-Team Heads Up" even if empty.** Peers need to know you checked, not just that nothing came up.
- **Don't editorialize.** Facts and status, not opinions. Let peers draw their own conclusions.
- **Sign as the team lead, not the AI.** This goes to peers; it should read as a person-to-person update.
- **Present to team lead for review before sending.** Never auto-send -- always show the draft first.

## Gotchas

- **"Cross-Team Heads Up" is the most valuable section.** Peers don't need to know about your internal tasks -- they need to know what affects them. Over-invest in getting this section right.
- **If there's nothing cross-team, say so explicitly.** "None this week" is a valid and useful signal. Don't pad with irrelevant items.
- **Team Health should be honest.** A permanent "Green" signal erodes trust. If the team is stretched, say Yellow. Peers respect candor.
- **Timing matters.** Send Friday afternoon or Monday morning. Mid-week updates get lost.
- **Keep the recipient list small.** This is for 2-3 peer leads, not a broadcast. If leadership wants a copy, create a separate format.
- **Blockers that affect peers should be called out in BOTH sections 3 and 4.** A blocker is a blocker; a cross-team blocker is a call to action.
- **Don't include internal personnel issues or team dynamics.** Keep it professional and work-focused.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Recipient list | 2-3 peer leads | Add/remove as org changes |
| Cross-team label | `sp:cross-solution` | Match your labeling scheme |
| Content limit | 10 lines | Strict -- trim aggressively |
| Cadence | Weekly (Friday) | Biweekly if peers prefer |
| Delivery channel | Terminal for review | Teams, email, Slack, or GitHub Discussion |

## Related Skills

- `/weekly-status` -- Detailed status that feeds into the digest
- `/standup` -- Daily data that accumulates into weekly view
- `/mbr` -- Monthly leadership report (different audience, different depth)
