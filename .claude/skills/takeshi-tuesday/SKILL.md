---
name: takeshi-tuesday
description: Captures weekly Takeshi Tuesday insights — market signals, competitive moves, content ideas, and action items. Maintains a persistent log so each session knows what was captured previously. Use when capturing weekly insights, market signals, or recurring intelligence sessions.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch
---

# Takeshi Tuesday

Capture and accumulate weekly insights from recurring intelligence sessions. Maintains persistent memory so each run builds on previous captures.

## How It Works

1. Capture this session's insights in structured format
2. Append to the persistent insights log
3. Compare against previous sessions to surface trends and deltas
4. Generate action items as GitHub Issues if requested

## Persistent Memory

This skill maintains a log of all captured sessions. Check for existing data:

```bash
# Read previous insights if they exist
cat insights/takeshi-tuesday-log.jsonl 2>/dev/null || echo "No previous sessions"
```

After capturing, append the new session:

```bash
# Append new session as a JSON line
echo '{"date":"YYYY-MM-DD","insights":[...],"actions":[...]}' >> insights/takeshi-tuesday-log.jsonl
```

## Capture Template

For each session, collect:

### Market Signals
Observations about the market, developer ecosystem, technology trends, or partner landscape.

For each signal:
- **Signal**: What was observed
- **Source**: Where this came from (conversation, article, data point)
- **Relevance**: How this affects our programs (High / Medium / Low)
- **Trend**: New / Accelerating / Stable / Declining

### Competitive Moves
Anything competitors (AWS, GCP, others) are doing that affects our space.

For each move:
- **Competitor**: Who
- **Move**: What they did or announced
- **Our Exposure**: How this affects us (Direct threat / Indirect / Opportunity)
- **Recommended Response**: What we should do about it

### Content Ideas Generated
Ideas for blogs, Reactor episodes, hackathon themes, session abstracts, or social content.

For each idea:
- **Type**: Blog / Reactor / Hack theme / Session / Social
- **Title/Topic**: Working title
- **Why Now**: What makes this timely
- **Owner**: Who should drive this

### Action Items
Concrete follow-ups from this session.

For each action:
- **Action**: What needs to happen
- **Owner**: Who does it
- **Due**: When
- **Create Issue?**: Yes / No (if yes, create with appropriate labels)

## Output Format

```markdown
## Takeshi Tuesday — [Date]

### Session Summary
[2-3 sentence overview of this week's key themes]

### Market Signals ([N] captured)
| # | Signal | Source | Relevance | Trend |
|---|--------|--------|-----------|-------|

### Competitive Moves ([N] captured)
| # | Competitor | Move | Our Exposure | Response |
|---|-----------|------|-------------|----------|

### Content Ideas ([N] generated)
| # | Type | Topic | Why Now | Owner |
|---|------|-------|---------|-------|

### Action Items ([N] items)
| # | Action | Owner | Due | Issue? |
|---|--------|-------|-----|--------|

### Delta From Last Session
[What changed since the last Takeshi Tuesday — new themes, resolved items, accelerating trends]
```

## Composition

This skill feeds into other skills:
- **Content ideas** → invoke `/content-pipeline` to add to production queue
- **Competitive moves** → invoke `/competitive-scan` for deeper analysis
- **Action items** → create GitHub Issues with `/issue-triage` compatible labels
- **Trends over time** → invoke `/okr-check` to verify alignment

## Saving the Session

After capture, save to two locations:

1. **Structured log** (for machine reading):
   ```bash
   # Append to JSONL log
   echo '{"date":"[DATE]","signals":[...],"competitive":[...],"content_ideas":[...],"actions":[...]}' >> insights/takeshi-tuesday-log.jsonl
   ```

2. **Readable notes** (for human review):
   Save the formatted output to `operations/meetings/takeshi-tuesday-YYYY-MM-DD.md`

## Gotchas

- Each session builds on the last. Always read the log first to detect trends and avoid duplicating previously captured signals.
- Market signals without a recommended response are just noise. Push for "so what?" on every signal.
- Content ideas are perishable. If an idea sits for 3+ weeks without being actioned, it's probably not worth pursuing.
- Competitive moves should be verified before acting on them. Use `/competitive-scan` for validation.
- The JSONL log format allows easy analysis: `cat insights/takeshi-tuesday-log.jsonl | python -m json.tool` to inspect.
- If the `insights/` directory doesn't exist, create it on first run.
