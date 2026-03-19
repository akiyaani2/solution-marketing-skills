---
name: solution-play-health
description: Strategic health scoring for solution plays — one layer above epic-health. Aggregates epic scores into play-level assessments with quarter-over-quarter comparison.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/sp-health"
  - "/solution-play-health"
  - "/sp-health --[play-name]"
  - "how are the solution plays doing"
  - "solution play status"
---

# Solution Play Health

Strategic health scoring for solution plays. One level above `/epic-health` — aggregates individual epic scores into play-level assessments and provides quarter-over-quarter comparison.

## Usage

```
/sp-health                    # All solution plays dashboard
/sp-health --[play-name]      # Deep dive on a specific play
/sp-health --compare Q3 Q4    # Quarter-over-quarter comparison
```

## What This Skill Does

### 1. All-Play Dashboard (`/sp-health`)

Scores every solution play and produces a single strategic view. A "solution play" is any strategic focus area tracked with an `sp:` label prefix in your repo.

### 2. Per-Play Deep Dive (`/sp-health --[play-name]`)

For a specific solution play:
- All epics tagged with that play's `sp:` label
- Per-epic health score (using `/epic-health` scoring)
- Key wins this quarter
- Key risks and blockers
- Next-quarter planning item status
- Actionable recommendations

### 3. Quarter-Over-Quarter Comparison

Show health trends across quarters — which plays improved, which degraded, and which initiatives shifted timelines.

## Scoring Model

Each solution play is scored on 5 dimensions:

| Dimension | Weight | 🟢 Green | 🟡 Yellow | 🔴 Red |
|-----------|--------|----------|-----------|--------|
| **Epic completion rate** | 30% | >70% of epics on track | 40-70% on track | <40% on track |
| **P0 item count** | 25% | 0-2 open P0s | 3-5 open P0s | >5 open P0s |
| **Blocked/at-risk items** | 20% | 0 blocked | 1-2 blocked | >2 blocked |
| **Freshness** | 15% | All epics updated <7 days | Some 7-14 days stale | Any >14 days stale |
| **Next-quarter planning** | 10% | Planning started | Not started but on calendar | No planning artifacts |

### Composite Score

```
Play Health = (
  (epic_completion_rate * 0.30) +
  (p0_score * 0.25) +
  (blocked_score * 0.20) +
  (freshness_score * 0.15) +
  (planning_score * 0.10)
)

Where:
- epic_completion_rate: Average health score of all epics in this play (from /epic-health)
- p0_score:            100 - (p0_count * 15), min 0
                       0 P0s = 100, 2 = 70, 5 = 25, 7+ = 0
- blocked_score:       100 - (blocked_count * 25), min 0
                       0 blocked = 100, 2 = 50, 4+ = 0
- freshness_score:     100 if all epics updated <7 days
                       Deduct 15 per epic stale >7 days
                       Deduct 30 per epic stale >14 days
- planning_score:      100 if next-quarter planning issues exist and are in progress
                       50 if planning issues exist but not started
                       0 if no planning artifacts found
```

## Implementation Steps

### 1. Identify solution plays

Solution plays are identified by `sp:` label prefixes. List all unique `sp:*` labels:

```bash
gh label list --repo OWNER/REPO \
  --json name \
  | jq '[.[].name | select(startswith("sp:"))]'
```

### 2. For each play, fetch tagged epics

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "type:initiative,sp:[play-name]" \
  --json number,title,labels,updatedAt,body \
  --limit 100
```

### 3. Score each epic using the epic-health formula

Apply the `/epic-health` scoring formula to each epic. Average the scores for the play's epic completion rate.

### 4. Count P0s per play

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "p0,sp:[play-name]" \
  --json number,title \
  --limit 100
```

### 5. Count blocked items per play

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "status:blocked,sp:[play-name]" \
  --json number,title \
  --limit 100
```

### 6. Check freshness

From the epic list in step 2, evaluate `updatedAt` timestamps against today's date.

### 7. Check next-quarter planning

Look for issues with next-quarter labels (e.g., `quarter:Q4` when currently in Q3):

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --label "sp:[play-name],quarter:[next-quarter]" \
  --json number,title,labels \
  --limit 50
```

## Output Format

### All-Play Dashboard

```markdown
# Solution Play Health — [Date]

## Dashboard

| Solution Play | Health | Epics | P0s | Blocked | Completion | Freshness | Planning | Trend |
|--------------|--------|-------|-----|---------|------------|-----------|----------|-------|
| [Play A] | 🟢 82 | 5 | 1 | 0 | 78% | All fresh | Started | ↑ |
| [Play B] | 🟡 58 | 3 | 4 | 2 | 52% | 1 stale | Not started | → |
| [Play C] | 🔴 31 | 4 | 6 | 3 | 35% | 2 stale | None | ↓ |
| [Cross-Solution] | 🟡 65 | 2 | 2 | 1 | 60% | Fresh | Started | ↑ |

---

## Key Findings

### Wins
- [Play A]: 3 epics moved to green since last check
- [Cross-Solution]: Joint event epic completed

### Risks
- [Play C]: 6 open P0s — cascade triage recommended
- [Play B]: No next-quarter planning started

### Recommendations
1. Run `/triage --p0-only` on [Play C] to reduce P0 count
2. Create FY[XX] planning epic for [Play B]
3. Schedule cross-solution sync for joint blockers
```

### Per-Play Deep Dive

```markdown
# [Play Name] — Deep Dive

**Health Score:** 58/100 🟡
**Trend:** → (stable)

## Epics

| # | Epic | Health | Completion | Blocked | Owner |
|---|------|--------|------------|---------|-------|
| #NN | [Title] | 🟢 85 | 80% | 0 | [name] |
| #NN | [Title] | 🟡 55 | 50% | 1 | [name] |
| #NN | [Title] | 🔴 28 | 20% | 2 | [name] |

## Key Wins This Quarter
- [Specific completed milestones]

## Key Risks
- [Specific blockers or at-risk items]

## Next-Quarter Status
| Item | Status | Notes |
|------|--------|-------|
| Planning epic created | Yes/No | |
| Budget allocated | Yes/No | |
| Stakeholder alignment | Yes/No | |

## Recommendations
1. [Actionable recommendation]
2. [Actionable recommendation]
```

### Quarter-Over-Quarter Comparison

```markdown
# Solution Play Health — Q[N] vs Q[N+1]

| Play | Q[N] Score | Q[N+1] Score | Delta | Assessment |
|------|-----------|-------------|-------|------------|
| [Play A] | 🟡 55 | 🟢 82 | +27 | Strong improvement |
| [Play B] | 🟢 75 | 🟡 58 | -17 | Degraded — P0 spike |
| [Play C] | 🔴 30 | 🔴 31 | +1 | Flat — needs intervention |

## Movement Details

### Improved
- [Play A]: Completed 3 epics, resolved all blockers

### Degraded
- [Play B]: 4 new P0s from [event], planning stalled

### Initiatives That Shifted
| Initiative | Original Quarter | New Quarter | Reason |
|-----------|-----------------|-------------|--------|
| [Title] | Q[N] | Q[N+1] | [Reason] |
```

## Gotchas

- **Solution play health is strategic.** This output feeds into MBR narratives and stakeholder briefings. Frame findings in terms of business impact, not just numbers.
- **Cross-solution plays are shared.** The `sp:cross-solution` label (or equivalent) applies to items spanning multiple plays. These items may be scored under multiple plays — deduplicate when aggregating.
- **Peer-owned plays have limited visibility.** If your team only sees cross-solution issues for peer-led plays, your health scoring reflects your team's view, not the full play health. Note this in the output.
- **Next-quarter planning weight is low (10%) early in the quarter** but becomes critical in the final month. Consider adjusting weight dynamically based on quarter progress.
- **This skill composes with `/epic-health`.** Run `/epic-health` first if you need fresh per-epic scores, then `/sp-health` to aggregate.
- **Label consistency is critical.** If epics are missing their `sp:` labels, they will not appear in play-level aggregation. Run `/triage --labels` first to fix any gaps.

## Related Skills

- `/epic-health` — Per-epic health scoring (feeds into play-level aggregation)
- `/mbr` — Monthly Business Review consumes play health data
- `/triage` — Operational hygiene that drives health improvements
- `/peer-digest` — Cross-team visibility for peer play leads
