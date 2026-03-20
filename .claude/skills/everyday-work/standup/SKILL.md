---
name: standup
description: Generates daily async standup from recent GitHub activity. Shows yesterday/today/blockers per person using owner labels.
allowed-tools: [Bash, Read, Grep, Glob]
---

# Standup Generator

**Type:** Daily Operations
**Speed:** ~15 seconds

## What This Skill Does

Generates an async standup by pulling recent GitHub activity for each team member:
1. **Yesterday** -- Issues closed or updated in last 24 hours
2. **Today** -- In-progress items grouped by owner
3. **Blockers** -- Items flagged as blocked
4. **Heads Up** -- Deadlines within the next 48 hours

## Triggers

```
/standup              # Full team standup
/standup [name]       # Single person's status
/standup --brief      # One-liner per person
```

## Prerequisites

- GitHub CLI (`gh`) authenticated with repo access
- Issues use `owner:[name]` labels to assign ownership
- Blocked items use `status:blocked` label
- Repository name set as `REPO` variable (e.g., `org/repo`)

## Implementation

When invoked, run these queries and assemble the output.

### Step 1: Set Variables

```bash
REPO="[org]/[repo]"
YESTERDAY=$(date -v-1d +%Y-%m-%d 2>/dev/null || date -d "yesterday" +%Y-%m-%d)
TODAY=$(date +%Y-%m-%d)
```

### Step 2: Query Recently Closed Issues (24h)

```bash
gh issue list --repo "$REPO" \
  --state closed \
  --search "closed:>=${YESTERDAY}" \
  --json number,title,labels,closedAt \
  --limit 50
```

### Step 3: Query In-Progress Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --json number,title,labels,updatedAt \
  --limit 100 \
  | jq '[.[] | select(.labels[].name | startswith("owner:"))]'
```

### Step 4: Query Blocked Issues

```bash
gh issue list --repo "$REPO" \
  --state open \
  --label "status:blocked" \
  --json number,title,labels,updatedAt
```

### Step 5: Group by Owner

Parse the `owner:[name]` label from each issue and group results by person.

### Step 6: Check Upcoming Deadlines

Look for issues with due dates in the next 48 hours. Due dates may live in issue bodies, milestone dates, or custom project fields.

## Output Format (Full)

```markdown
# Team Standup -- [Date]

## [Person A]
**Yesterday:**
- Closed #XX -- [Title]
- Updated #XX -- [Title]

**Today:**
- #XX -- [Title] (In Progress)
- #XX -- [Title] (In Progress)

**Blockers:** None

---

## [Person B]
**Yesterday:**
- Closed #XX -- [Title]

**Today:**
- #XX -- [Title] (In Progress)

**Blockers:**
- #XX -- Waiting on [dependency]

---

## Deadlines (Next 48h)
| Due | Issue | Owner |
|-----|-------|-------|
| Tomorrow | #XX -- [Title] | [Name] |
| [Date] | #XX -- [Title] | [Name] |

## Team Blockers
| Issue | Owner | Blocker | Days Blocked |
|-------|-------|---------|--------------|
| #XX | [Name] | [Description] | 3 |

---
*Generated [timestamp]*
```

## Output Format (Brief)

When `--brief` is passed, compress to one line per person:

```markdown
Team Standup -- [Date]

[Person A]: 2 closed, 3 in progress, 0 blocked
[Person B]: 1 closed, 2 in progress, 1 blocked
[Person C]: 0 closed, 5 in progress, 0 blocked

Deadlines in 48h: 2 | Blockers: 1
```

## Output Format (Single Person)

When a name is passed (e.g., `/standup jane`), show only that person's section in the full format. Filter by `owner:[name]` label.

## Gotchas

- **Date command differs between macOS and Linux.** macOS uses `date -v-1d`, GNU/Linux uses `date -d "yesterday"`. The script should handle both.
- **`owner:` labels are the source of truth for grouping.** Issues without an `owner:` label will be omitted. If a person shows zero activity, check their label first.
- **The `closed:>=` search qualifier uses the GitHub search API**, which can have a slight indexing delay (up to a few minutes). Very recently closed issues may not appear.
- **jq must be installed** for JSON filtering. If not available, fall back to parsing JSON in the language of choice.
- **Large teams (10+) should use `--limit 100` or higher** on the in-progress query to avoid missing items.
- **"Updated" means any change** -- comment, label change, status change. It is noisy. Focus on meaningful updates (closed, reopened, label changes) rather than bot activity.
- **Weekend standups** should look back to Friday, not just 24 hours. Detect day-of-week and adjust the lookback window.

## Customization Points

| Setting | Default | Override |
|---------|---------|----------|
| Lookback window | 24 hours | Adjust for weekends/holidays |
| Owner label prefix | `owner:` | Change if your team uses `assigned:` or similar |
| Blocked label | `status:blocked` | Change to match your label scheme |
| Deadline source | Issue body / milestones | Add project custom fields if used |

## Related Skills

- `/weekly-status` -- Longer-horizon weekly roll-up
- `/1on1-prep` -- Deep prep for a single person's 1:1
- `/peer-digest` -- Cross-team Friday update
