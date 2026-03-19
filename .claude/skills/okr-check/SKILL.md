---
name: okr-check
description: Check any work item, initiative, or proposal against your OKRs and strategic goals. Scores alignment, flags drift, and identifies orphaned work that doesn't connect to any objective.
---

# OKR Alignment Check

Score any piece of work against your team's objectives and key results. Surfaces misalignment before resources are committed.

## Trigger Phrases

- `/okr-check`
- `/okr-check #[issue-number]`
- `/okr-check "Build 2026 hackathon"`
- `does this align to our OKRs`
- `is this on strategy`
- `alignment check`

## What This Skill Does

### 1. Single Item Check (`/okr-check #[issue] or "initiative name"`)

Take an issue, initiative, or proposal and score it:

```markdown
## OKR Alignment: [Item Name]

### Alignment Score: [STRONG / MODERATE / WEAK / NONE]

### Maps To:
- **Objective**: [which objective this serves]
- **Key Result**: [which KR this moves]
- **Solution Play**: [which play this falls under]

### Alignment Evidence
- [How this work directly moves a KR]
- [Metric it impacts]

### Alignment Concerns
- [Where the connection is weak or indirect]
- [If it serves a goal not in the current OKRs]

### Verdict
[Is this worth doing right now given current priorities? If not, what should replace it?]
```

### 2. Batch Alignment Scan (`/okr-check --scan`)

Score all open P0 and P1 items against OKRs:

```bash
gh issue list --state open --label p0 --limit 50 --json number,title,labels
gh issue list --state open --label p1 --limit 100 --json number,title,labels
```

Output:
| # | Title | Priority | OKR Alignment | Score |
|---|-------|----------|--------------|-------|
| | | | | Strong/Moderate/Weak/None |

Flag items with **Weak** or **None** alignment for review.

### 3. OKR Coverage Check (`/okr-check --coverage`)

Reverse the scan — start from OKRs and check which ones have active work:

```markdown
## OKR Coverage — [Quarter]

| Objective | Key Result | Active Issues | Coverage |
|-----------|-----------|--------------|----------|
| [O1] | [KR1.1] | #N, #N, #N | STRONG |
| [O1] | [KR1.2] | #N | THIN |
| [O1] | [KR1.3] | (none) | NONE |
| [O2] | [KR2.1] | #N, #N | MODERATE |

### Uncovered KRs (No Active Work)
- [KR] — [recommendation: create issues, deprioritize KR, or reassign]

### Over-Indexed KRs (Too Much Work)
- [KR] — [N] issues assigned. Consider consolidating.

### Orphaned Work (Doesn't Map to Any KR)
- #[N] — [title] — [recommendation]
```

### 4. Strategic Drift Detection (`/okr-check --drift`)

Compare current work distribution against intended OKR weighting:

```markdown
## Strategic Drift Analysis

### Intended vs Actual Allocation
| Objective | Intended Weight | Actual Issues | Actual % | Drift |
|-----------|----------------|--------------|---------|-------|
| [O1] | 40% | [N] issues | [N]% | [+/-N]% |
| [O2] | 30% | [N] issues | [N]% | [+/-N]% |
| [O3] | 20% | [N] issues | [N]% | [+/-N]% |
| [O4] | 10% | [N] issues | [N]% | [+/-N]% |

### Drift Interpretation
- [Where we're over-investing relative to goals]
- [Where we're under-investing]
- [Recommendation]
```

## Setting Up OKRs

For this skill to work well, your OKRs should be documented in the repo. Recommended location:

```
operations/okrs/
  fy26-h2-okrs.md     # Current half-year OKRs
  fy27-okrs.md         # Next FY OKRs (when planning starts)
```

Or reference them in your CLAUDE.md / copilot-instructions.md so the AI has context.

## Gotchas

- This skill requires OKRs to exist and be accessible. If they're only in a PowerPoint somewhere, the scan can't work. Put them in the repo.
- Not everything needs to map to an OKR. Some work is operational hygiene (triage, docs, team processes). Flag it as "operational" rather than "unaligned."
- Alignment ≠ priority. Something can be strongly aligned but low priority because of timing or dependencies.
- Watch for "strategy theater" — work that technically maps to an OKR but doesn't actually move the key result.
- Re-run coverage checks quarterly when OKRs refresh.
- Combine with `/sp-health` for the full strategic picture.
