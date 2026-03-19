---
name: epic-health
description: Score all type:initiative epics on 5 health dimensions — completion %, blocked count, staleness, RAPID completeness, TBD count. Produces a weighted health score with green/yellow/red grading.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/epic-health"
  - "/epic-health --red-only"
  - "/epic-health #[number]"
  - "check epic health"
  - "are epics on track"
  - "epic status"
---

# Epic Health Scoring

Score every `type:initiative` epic on 5 health dimensions. Produces a weighted composite score (0-100) with green/yellow/red grading per epic and an aggregate summary.

## Usage

```
/epic-health              # All epics
/epic-health --red-only   # Only show unhealthy epics
/epic-health #24          # Specific epic by number
/epic-health [owner]      # Epics owned by a specific person
```

## Health Dimensions

| # | Dimension | Weight | What It Measures |
|---|-----------|--------|------------------|
| 1 | **Completion %** | 30% | Sub-issues done vs total |
| 2 | **Blocked count** | 25% | Items with `status:blocked` label |
| 3 | **Staleness** | 20% | Days since last update on epic or sub-issues |
| 4 | **RAPID completeness** | 15% | Are RAPID roles assigned in the issue body? |
| 5 | **TBD count** | 10% | Unresolved "TBD" / "TBA" placeholders in body |

## Health Grades

| Grade | Score | Criteria | Action |
|-------|-------|----------|--------|
| 🟢 **Green** | 75-100 | >70% complete, 0 blocked, updated <7 days | On track |
| 🟡 **Yellow** | 40-74 | 40-70% complete OR 1-2 blocked OR 7-14 days stale | Needs attention |
| 🔴 **Red** | 0-39 | <40% complete OR 3+ blocked OR >14 days stale | Escalate |

## Scoring Formula

```
Health Score = (
  (completion_pct * 0.30) +
  (blocked_score * 0.25) +
  (freshness_score * 0.20) +
  (rapid_score * 0.15) +
  (tbd_score * 0.10)
)

Where:
- completion_pct:  (closed_sub_issues / total_sub_issues) * 100
                   If no sub-issues, score = 0 (flag as "no breakdown")
- blocked_score:   100 - (blocked_count * 20), min 0
                   0 blocked = 100, 1 = 80, 2 = 60, 5+ = 0
- freshness_score: 100 - (days_since_update * 5), min 0
                   Updated today = 100, 7 days ago = 65, 20+ days = 0
- rapid_score:     100 if all 5 RAPID roles filled
                   50 if partial (some roles filled)
                   0 if no RAPID table found
- tbd_score:       100 - (tbd_count * 10), min 0
                   0 TBDs = 100, 5 = 50, 10+ = 0
```

## Implementation Steps

### 1. Fetch all epics

```bash
# Get all open epics (type:initiative)
gh issue list --repo OWNER/REPO \
  --state open \
  --label "type:initiative" \
  --json number,title,body,labels,updatedAt,assignees \
  --limit 100
```

If `--red-only` flag: run the full scoring, then filter output to only red epics.

If `#[number]` specified: fetch just that issue.

If `[owner]` specified: add `--label "owner:[name]"` to the query.

### 2. For each epic, calculate sub-issue completion

```bash
# Get the epic body and parse task list items
gh issue view [EPIC_NUMBER] --repo OWNER/REPO --json body

# Count patterns in the body:
#   "- [x]" or "- [X]" = completed
#   "- [ ]"            = incomplete
# Also check for "#NN" references to linked issues
```

For linked issues (referenced as `#NN` in the body), check their state:

```bash
gh issue view [SUB_ISSUE_NUMBER] --repo OWNER/REPO --json state
```

### 3. Check for blocked sub-issues

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "status:blocked" \
  --json number,title,labels
```

Cross-reference blocked issues against each epic's sub-issue list.

### 4. Parse RAPID table from epic body

Look for a markdown table containing RAPID roles:
- `Recommend`, `Agree`, `Perform`, `Input`, `Decide`
- Score: 100 if all filled, 50 if partial, 0 if missing

### 5. Count TBD occurrences

Search the epic body for case-insensitive occurrences of:
- "TBD", "TBA", "To be determined", "To be announced"

### 6. Calculate composite score and assign grade

Apply the weighted formula. Assign the health grade emoji.

## Output Format

```markdown
# Epic Health Report — [Date]

## Summary
| Health | Count | Epics |
|--------|-------|-------|
| 🟢 Green | X | #NN, #NN |
| 🟡 Yellow | X | #NN, #NN |
| 🔴 Red | X | #NN |

---

## 🔴 Red Epics (Needs Escalation)

### #NN — [Epic Title]
| Metric | Value | Detail |
|--------|-------|--------|
| Completion | 15% | 2/13 sub-issues done |
| Blocked | 3 | #NN, #NN, #NN |
| Last Update | 18 days | Stale |
| RAPID | Incomplete | Missing Decide role |
| TBDs | 8 | Scoping incomplete |

**Health Score:** 25/100 🔴
**Recommendation:** [Actionable next step]

---

## 🟡 Yellow Epics (Needs Attention)

### #NN — [Epic Title]
| Metric | Value | Detail |
|--------|-------|--------|
| Completion | 55% | 5/9 sub-issues done |
| Blocked | 1 | #NN |
| Last Update | 10 days | Slightly stale |
| RAPID | Complete | All roles assigned |
| TBDs | 2 | Budget, vendor TBD |

**Health Score:** 62/100 🟡
**Recommendation:** [Actionable next step]

---

## 🟢 Green Epics (On Track)

### #NN — [Epic Title]
| Metric | Value |
|--------|-------|
| Completion | 80% |
| Blocked | 0 |
| Last Update | 2 days |
| RAPID | Complete |
| TBDs | 0 |

**Health Score:** 92/100 🟢

---

## Trends (if previous data available)

| Epic | Last Check | This Check | Delta |
|------|-----------|-----------|-------|
| #NN | 🔴 20 | 🟡 45 | +25 |
| #NN | 🟡 60 | 🟢 78 | +18 |
```

## Auto-Label Option

When run with `--auto-label`, apply health labels to epics:

```bash
# Add new health label
gh issue edit [NUMBER] --repo OWNER/REPO --add-label "health:red"

# Remove old health labels
gh issue edit [NUMBER] --repo OWNER/REPO --remove-label "health:yellow,health:green"
```

## Gotchas

- **No sub-issues = score 0 for completion.** An epic without a task breakdown should be flagged as "needs breakdown" rather than scored as healthy. This is a common gap when epics are freshly created.
- **Staleness can be misleading.** An epic body might not be updated but its sub-issues could be active. Check sub-issue `updatedAt` timestamps too, not just the epic's own timestamp.
- **Pagination matters.** If your repo has 100+ open issues, `gh issue list` may truncate results. Use `--limit 200` or paginate with `--jq` filtering.
- **RAPID table format varies.** Teams may use different table formats. Look for role keywords (Recommend, Agree, Perform, Input, Decide) anywhere in the body, not just in a perfectly formatted table.
- **Duplicate project board entries.** An issue can appear multiple times on a project board. When counting, deduplicate by issue number.
- **TBD false positives.** The string "TBD" might appear in historical context ("was TBD, now confirmed"). Parse carefully and consider proximity to dates or names.

## Related Skills

- `/sp-health` — Aggregates epic health to the solution play level
- `/mbr` — Monthly roll-up includes epic health summary
- `/standup` — Daily view of blocked items across epics
- `/1on1-prep` — Shows a person's epic health for meeting prep
