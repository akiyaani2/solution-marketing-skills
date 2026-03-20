---
name: leadership-update
description: Generates weekly leadership update submissions in a strategic format — leads with the "what" and "why," stays at the right altitude, includes competitive context and business impact. Use when preparing weekly executive updates, leadership digests, or recurring status submissions.
allowed-tools: Read, Grep, Glob, Bash
---

# Weekly Leadership Update

Generate a weekly leadership update that stays at the right strategic altitude. Designed for recurring submissions to VP+ leadership.

## Trigger Phrases

- `/leadership-update`
- `prep the weekly update`
- `weekly leadership submission`
- `what should I include in the exec update`

## The Altitude Check

Before including ANY bullet, ask: *Does leadership really need to know this?*

| Include | Don't Include |
|---------|--------------|
| Strategic direction changes | Minor task completions |
| Program milestones with business impact | Individual ticket updates |
| Competitive positioning wins | Internal process changes |
| Partner milestones | Fire drills or temporary blockers |
| Metrics WITH business context | Raw numbers without narrative |
| Risks that affect direction | Risks already being handled |

## Strategic Framing

For each bullet, use this pattern:

1. **One-line headline** — what happened (strategic "what")
2. **Why it matters** — strategic goal it serves
3. **Evidence** — metric with comparison (vs target, vs last period, vs competitor)

**BAD — too executional:**
> - Closed 12 issues this week
> - Updated lab content
> - Had team meetings

**GOOD — right altitude:**
> **Hackathon registration pipeline on track for annual target**
> - Partner co-marketing registration opened at conference; 400 signups in first 48 hours (2x pace vs prior year)
> - Unified platform confirmed for Q4 flagship event — first cross-team hack platform

## Output Template

```
Week of [DATE]

[Your Team/Function] — [Your Name]

**Highlights:**
- [Strategic headline — what happened]
  - [Why it matters — metric, competitive context, business impact]
- [Strategic headline]
  - [Supporting context]

**In Motion:**
- [Key initiative] — [status with milestone context]

**Upcoming:**
- [Next milestone] — [date + why it matters]

**Escalations/Risks:**
- [Only if strategic-level — omit if none]
```

## Formatting Rules

- **Depth:** 2 levels max (headline + one level of bullets)
- **Length:** 3-5 strong bullets beats 10 weak ones
- **Tone:** Strategic, concise, leadership-appropriate
- **Links:** Include dashboard or asset links when useful for quick reference
- **Negative items:** Frame as "risk + mitigation," not "things went wrong"
- **Always submit.** Even during light weeks — short and sweet is fine

## Variations

**For VP-level audience:**
Lead with business impact and competitive context. Skip operational details entirely. 3 bullets max.

**For Director-level audience:**
Include program-level milestones and team velocity. Light on strategy, heavier on progress. 5 bullets OK.

**For cross-team peers:**
Focus only on what affects them. Dependencies, shared events, resource conflicts.

## Tips for Better Results

- Provide your raw notes or issue list and say "reframe these at leadership altitude"
- Mention the audience by level ("this goes to our VP") so the AI calibrates tone
- Include competitive benchmarks when you have them — "2x pace vs last year" is more compelling than "400 signups"
- If you're unsure about including something, leave it out. Brevity earns trust at leadership level.

## Gotchas

- The most common mistake is including too much. Leadership reads 10+ updates weekly. Yours needs to be scannable in 30 seconds.
- Competitive context wins. "Our program outpaced the competitor equivalent by 2x" gets remembered. "We had 400 signups" doesn't.
- Negative framing matters. "Risk: timeline slipped 1 week — mitigation: content lock preserved by compressing review cycle" is professional. "We're late" is not.
- If you have nothing strategic to report, say so briefly: "Steady execution this week; all programs tracking to plan." That's a perfectly valid update.
