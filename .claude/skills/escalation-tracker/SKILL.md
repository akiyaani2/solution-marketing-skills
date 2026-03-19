---
name: escalation-tracker
description: Tracks items escalated to leadership with SLA monitoring. Dashboard of pending, responded, and resolved escalations. Creates new escalations with options and recommendations.
allowed-tools: [Bash, Read, Grep, Glob]
disable-model-invocation: true
---

# Escalation Tracker

**Type:** Governance & Accountability
**Speed:** ~20 seconds

## What This Skill Does

Tracks all upward escalations to leadership with SLA awareness. Ensures items sent up the chain get responses and outcomes recorded. Nothing escalated should ever get lost.

1. **Dashboard** -- Pending, responded, resolved escalations at a glance
2. **SLA Monitoring** -- Configurable expected response times by leadership tier
3. **Create Escalation** -- Structured template with options and recommendation
4. **Aging Alerts** -- Flag items past their expected response window

## Triggers

```
/escalations                # Full escalation dashboard
/escalations --pending      # Only pending items (awaiting response)
/escalations --overdue      # Only items past SLA
/escalations --create       # Create a new escalation
/escalations --to [name]    # Filter by escalation target
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use `status:waiting-approval` label for pending escalations
- Issues use `stakeholder:[name]` labels to track who it was escalated to
- Repository name set as `REPO` variable

## Implementation

### Step 1: Query Pending Escalations

```bash
REPO="[org]/[repo]"

# Get all issues waiting on leadership approval
gh issue list --repo "$REPO" \
  --state open \
  --label "status:waiting-approval" \
  --json number,title,labels,updatedAt,createdAt \
  --limit 50
```

### Step 2: Query by Stakeholder (Optional Filter)

```bash
# Filter to specific person
gh issue list --repo "$REPO" \
  --state open \
  --label "status:waiting-approval" \
  --label "stakeholder:[name]" \
  --json number,title,labels,updatedAt,createdAt \
  --limit 20
```

### Step 3: Search for Escalation Patterns in Comments

```bash
# Search for escalation language in issue bodies and comments
gh search issues --repo "$REPO" \
  --state open \
  "escalated to" \
  --json number,title,updatedAt \
  --limit 20

gh search issues --repo "$REPO" \
  --state open \
  "waiting on approval" \
  --json number,title,updatedAt \
  --limit 20
```

### Step 4: Query Recently Resolved Escalations (Last 30 Days)

```bash
THIRTY_DAYS_AGO=$(date -v-30d +%Y-%m-%d 2>/dev/null || date -d "30 days ago" +%Y-%m-%d)

gh issue list --repo "$REPO" \
  --state closed \
  --label "status:waiting-approval" \
  --search "closed:>=${THIRTY_DAYS_AGO}" \
  --json number,title,labels,closedAt \
  --limit 20
```

### Step 5: Calculate SLA Status

For each pending item, compute days since escalation and compare against the expected response time for the target stakeholder.

## Output Format (Dashboard)

```markdown
## Escalation Status -- [Date]

### Pending (Awaiting Response)
| # | Title | Escalated To | Date Sent | Days Pending | SLA Status |
|---|-------|-------------|-----------|-------------|-----------|
| XX | [Title] | [Name/Title] | [Date] | X days | On Track / Approaching / OVERDUE |

### Responded (Needs Your Action)
| # | Title | Response From | Response Date | Action Needed |
|---|-------|-------------|---------------|--------------|
| XX | [Title] | [Name] | [Date] | [What to do next] |

### Resolved (Last 30 Days)
| # | Title | Resolved By | Resolution | Days to Resolve |
|---|-------|-----------|-----------|----------------|
| XX | [Title] | [Name] | [Outcome] | X days |

### SLA Summary
| Escalation Target | Pending | On Track | Overdue | Avg Resolution (30d) |
|-------------------|---------|----------|---------|---------------------|
| [Manager] | X | X | X | X days |
| [Senior Director] | X | X | X | X days |
| [VP] | X | X | X | X days |

---
*Generated [timestamp]*
```

## Output Format (Pending Only)

When `--pending` is passed, show only the Pending table with additional context:

```markdown
## Pending Escalations -- [Date]

**Total:** X items | **Overdue:** X items | **Oldest:** X days

| # | Title | To | Sent | Days | SLA | Risk |
|---|-------|----|------|------|-----|------|
| XX | [Title] | [Name] | [Date] | X | OVERDUE | [Why this matters] |
| XX | [Title] | [Name] | [Date] | X | On Track | -- |

### Recommended Actions
- **#XX**: Follow up with [Name] -- past SLA by X days
- **#XX**: On track, no action needed
```

## SLA Configuration

Define expected response times by leadership tier. Adjust these to match your organization's norms:

| Escalation Target | Role Level | Expected Response | Alert Threshold |
|-------------------|-----------|------------------|-----------------|
| [Direct Manager] | Manager/Sr. Director | 2-3 business days | After 3 days |
| [Skip-Level] | Director/Sr. Director | 3-5 business days | After 5 days |
| [VP] | VP | 5-7 business days | After 7 days |
| [CVP / SVP] | CVP+ | 7-14 business days | After 14 days |
| [External Partner] | External | Varies | After 10 days |

**SLA Status Logic:**
- **On Track:** Within expected response window
- **Approaching:** Within 1 day of SLA threshold
- **OVERDUE:** Past SLA threshold -- requires follow-up action

## Create Escalation Template

When `/escalations --create` is invoked, generate this structured template as a GitHub issue comment or new issue:

```markdown
## ESCALATION

**What:** [1-line summary of what needs a decision]
**Why escalating:** [Trigger -- budget threshold, external stakeholder impact, strategy change, policy exception, etc.]
**Escalated to:** [Name and role]
**Date sent:** [Today]
**Response needed by:** [Date based on SLA]

### Options

**Option A: [Name]**
- Pros: [Benefits]
- Cons: [Risks/costs]
- Impact: [What happens if we choose this]

**Option B: [Name]**
- Pros: [Benefits]
- Cons: [Risks/costs]
- Impact: [What happens if we choose this]

**Option C: Do Nothing**
- Consequence: [What happens if we don't decide]

### Recommendation
[Option X] because [reasoning tied to team/org priorities]

### Time Sensitivity
[Urgent / This Week / Can Wait 2 Weeks]
Why: [What happens if delayed]

### Related Issues
- #XX -- [Context]
- #XX -- [Context]
```

After creating, apply labels: `status:waiting-approval` + `stakeholder:[name]`

## Decision Rights Reference

Configure this table for your team's approval chain:

| Decision Type | First Escalation | Second Escalation |
|--------------|-----------------|-------------------|
| Budget < [threshold] | Auto-approved | -- |
| Budget > [threshold] | [Team Lead] to [Manager] | [Manager] to [VP] |
| External commitments | [Team Lead] to [Manager] | -- |
| Strategy/direction changes | [Team Lead] to [Manager] | [Manager] to [VP] |
| Vendor contracts < [threshold] | [Team Lead] (approves) | -- |
| Vendor contracts > [threshold] | [Team Lead] to [Manager] | [Manager] to [VP] |
| Headcount / hiring | [Team Lead] to [Manager] | Up the chain |
| Partner co-marketing funds | [Team Lead] to [Manager] | [Manager] to [VP/CVP] |

## Gotchas

- **Not all escalations happen in GitHub.** Teams, email, and Slack conversations often contain escalation decisions that never make it into issues. This skill only tracks what is in GitHub. Encourage the team to log escalations as issue comments for traceability.
- **"Waiting on approval" and "blocked" are different things.** A blocked issue is stuck on a dependency. An escalation is a conscious decision to send something up the chain. Don't conflate them.
- **Response time varies by decision type, not just person.** A VP might respond to a budget request in 1 day but take 2 weeks on a strategic review. Factor decision type into SLA expectations.
- **Don't create duplicate escalation issues.** Before creating a new escalation, search for existing open items on the same topic.
- **The `stakeholder:` label is your primary filter.** If your team doesn't have stakeholder labels, fall back to searching issue text for names. But labels are far more reliable.
- **Overdue doesn't always mean "dropped."** Sometimes leadership is deliberately taking time to consult others. An overdue alert is a prompt to check in, not to assume failure.
- **Close the loop.** When an escalation gets resolved, update the issue with the outcome and close it. Unresolved escalation issues accumulate and create noise.
- **Track resolution patterns over time.** If a particular stakeholder is consistently slow, that is useful data for the team lead to raise in their own 1:1.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Escalation label | `status:waiting-approval` | Match your label scheme |
| Stakeholder label prefix | `stakeholder:` | Match your label scheme |
| SLA thresholds | See table above | Adjust per your org's norms |
| Resolution lookback | 30 days | Adjust for reporting cadence |
| Escalation template | Options + recommendation | Simplify for low-stakes decisions |

## Related Skills

- `/1on1-prep --upward` -- Escalations feed directly into upward 1:1 prep
- `/mbr` -- Blocked/escalated items appear in MBR package
- `/weekly-status` -- Escalation status as a discussion point
