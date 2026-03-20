---
name: post-mortem
description: Runs a structured post-mortem after an event, program, or initiative completes — capture what worked, what didn't, root causes, and improvements for next time. Maintains a persistent log of all post-mortems for cross-event learning. For use after hackathons, events, campaigns, or any completed initiative.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
disable-model-invocation: true
---

# Post-Mortem

Structured retrospective for completed events, hackathons, campaigns, or initiatives. Captures learnings and stores them persistently so future planning benefits from past experience.

## How It Works

1. Pull data from the completed initiative (GitHub Issues, metrics if available)
2. Walk through the structured post-mortem framework
3. Generate the post-mortem document
4. Append key learnings to the persistent log
5. Create improvement issues for next iteration

## Persistent Memory

This skill maintains a log of all post-mortems. Check for previous learnings:

```bash
# Read previous post-mortems
cat insights/post-mortem-log.jsonl 2>/dev/null || echo "No previous post-mortems"

# Search for learnings from a specific program type
grep "hackathon" insights/post-mortem-log.jsonl 2>/dev/null
```

Before starting a new post-mortem, surface relevant past learnings:

```bash
# Find past post-mortems for similar events
grep -i "$ARGUMENTS" insights/post-mortem-log.jsonl 2>/dev/null | python3 -c "
import sys, json
for line in sys.stdin:
    pm = json.loads(line)
    print(f\"  {pm['date']} — {pm['name']}: {', '.join(pm.get('top_learnings', [])[:3])}\")
" 2>/dev/null || echo "No matching past post-mortems found"
```

## Post-Mortem Framework

### Step 1: Gather Data

Pull from GitHub Issues:

```bash
# Get all issues from the initiative's epic
gh issue list -R [OWNER]/[REPO] --state all --json number,title,labels,closedAt,state --limit 200
```

Key data points:
- Total issues created vs completed vs still open
- Timeline: planned dates vs actual dates
- Blocked items and their resolution
- Team member load distribution

### Step 2: What Worked

Identify successes. Be specific — "the event went well" is not useful. "Registration hit 150% of target because the partner co-marketing email drove 3x normal signup rate" is useful.

For each success:
- **What**: Specific thing that worked
- **Why**: Root cause of the success (not just luck)
- **Repeatable?**: Can we do this again? Yes / Maybe / One-time

### Step 3: What Didn't Work

Identify failures and near-misses. No blame — focus on systems, not people.

For each issue:
- **What**: Specific thing that failed or underperformed
- **Impact**: How bad was it? (Critical / Significant / Minor / Near-miss)
- **Root Cause**: Why did this happen? (Use 5 Whys if needed)
- **Preventable?**: Could we have seen this coming? Yes / Maybe / No

### Step 4: Metrics Review

Compare planned vs actual:

| Metric | Target | Actual | Delta | Assessment |
|--------|--------|--------|-------|-----------|
| Registrations | | | | |
| Submissions/Completions | | | | |
| Attendance | | | | |
| Engagement score | | | | |
| Budget spend | | | | |
| NPS/Satisfaction | | | | |

### Step 5: Timeline Review

| Milestone | Planned Date | Actual Date | Delta | Notes |
|-----------|-------------|-------------|-------|-------|
| Kickoff | | | | |
| Partner lock | | | | |
| Content lock | | | | |
| Registration open | | | | |
| Event live | | | | |
| Post-event wrap | | | | |

### Step 6: Improvements for Next Time

For each improvement:
- **Change**: What we'd do differently
- **Category**: Process / Content / Timeline / Team / Partner / Platform
- **Effort**: Low / Medium / High
- **Impact**: Low / Medium / High
- **Create Issue?**: Yes / No (if yes, tag for next iteration)

### Step 7: Key Learnings (Top 3-5)

Distill everything into the top learnings. These go into the persistent log and should be surfaced before planning the next similar event.

## Output Format

```markdown
## Post-Mortem: [Initiative Name]

**Date**: [date]
**Type**: [hackathon / event / campaign / program]
**Owner**: [name]
**Duration**: [start] to [end]
**Epic**: #[number]

---

### Executive Summary
[3-5 sentences: what was this, did it succeed, key takeaway]

### What Worked
| # | Success | Why It Worked | Repeatable? |
|---|---------|-------------|-------------|

### What Didn't Work
| # | Issue | Impact | Root Cause | Preventable? |
|---|-------|--------|-----------|-------------|

### Metrics
| Metric | Target | Actual | Delta | Assessment |
|--------|--------|--------|-------|-----------|

### Timeline Accuracy
| Milestone | Planned | Actual | Delta |
|-----------|---------|--------|-------|

### Improvements for Next Time
| # | Change | Category | Effort | Impact | Issue Created? |
|---|--------|----------|--------|--------|---------------|

### Key Learnings
1. [Most important learning]
2. [Second learning]
3. [Third learning]

### Team Recognition
- [Callout for people who went above and beyond]
```

## Saving the Post-Mortem

1. **Full document**: Save to `operations/meetings/post-mortem-[initiative]-YYYY-MM-DD.md`

2. **Persistent log** (for cross-event learning):
   ```bash
   echo '{"date":"[DATE]","name":"[NAME]","type":"[TYPE]","top_learnings":["...","...","..."],"metrics":{"target":N,"actual":N},"improvements":["...","..."]}' >> insights/post-mortem-log.jsonl
   ```

3. **Improvement issues**: For each "Create Issue? Yes" item, create a GitHub Issue labeled with the next iteration's event label.

## Composition

- Before starting: read past post-mortems from the log to surface recurring themes
- After completing: `/okr-check` the learnings against current OKRs
- Improvement items: create issues that `/event-ops` or `/hack-status` will pick up in future planning
- Metrics: feed into `/mbr` for the monthly narrative
- Patterns across post-mortems: feed into `/competitive-scan` if competitor comparison is relevant

## Gotchas

- Do the post-mortem within 2 weeks of the event ending. After that, details fade and the learning value drops sharply.
- "Everything went fine" is never the right answer. There are always improvements, even for successful events.
- Don't skip the metrics section even if data is incomplete. Note what data you DON'T have — that itself is a learning (improve data collection next time).
- Root cause analysis should go at least 3 levels deep. "We were late" → "Why?" → "Content wasn't ready" → "Why?" → "No content lock deadline was set" → there's your improvement.
- The persistent log is the real value. Individual post-mortems are useful once. The accumulated log of learnings across dozens of events is transformative.
- Team recognition matters. Post-mortems that only focus on failures create a culture where no one wants to do them.
