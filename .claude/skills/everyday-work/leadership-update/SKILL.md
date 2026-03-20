---
name: leadership-update
description: "Generates strategic leadership updates that stay at the right altitude — leads with impact, includes competitive context, and filters out operational noise. Use when preparing weekly executive updates, leadership digests, or recurring status submissions. Also activate when someone shares raw notes or an issue list and says 'reframe this for leadership,' 'what should I include in the exec update,' or is preparing for a recurring leadership cadence — even if they don't call it a 'leadership update' explicitly."
allowed-tools: Read, Grep, Glob, Bash
---

# Leadership Update Generator

Transforms raw work data — issue lists, standup notes, weekly activity — into strategic leadership updates that land at the right altitude. Designed for recurring VP+ submissions where every word competes for attention across a portfolio of 10+ team updates.

## When to Activate

Trigger on ANY of these signals:
- Direct: "leadership update," "weekly update," "exec submission"
- Indirect: "reframe this for leadership," "what should I include," "make this executive-level"
- Implied: user shares raw notes and mentions it's for their VP, skip-level, or a leadership cadence
- Recurring: any mention of a weekly or biweekly leadership reporting cycle

## Step 1: Apply the Altitude Check

Before writing anything, filter the input. Load the full altitude framework from [references/altitude-check.md](references/altitude-check.md).

For every potential bullet, apply the three-question test:

1. **"Would my skip-level care about this?"** If your VP's VP wouldn't mention it, cut it.
2. **"Does this connect to a business outcome?"** "Updated 12 issues" fails. "Registration pipeline at 85% of annual target" passes.
3. **"Would this change anyone's decision?"** If knowing this wouldn't cause leadership to act differently, it's noise.

**Fail any one question = cut the bullet.** Move it to a team standup or 1:1 instead.

## Step 2: Route by Audience

| Audience | Altitude | Bullet Count | What They Need |
|----------|----------|-------------|---------------|
| VP+ | Portfolio level | 2-3 max | Is the portfolio healthy? Strategic shifts? |
| Sr. Director | Program level | 3-5 | Are key programs on track? What needs attention? |
| Director (manager) | Initiative level | 5-7 | What's moving, stuck, or needs a decision? |
| Peers | Dependency level | 2-3 | What affects their team? What to coordinate on? |

If the user hasn't specified the audience, ask. The same accomplishment is a full bullet for a Director, a sub-bullet for a VP, and not included at all for a CVP.

## Step 3: Frame Each Bullet

Every bullet needs a frame that makes the information meaningful. Load the full framework from [references/framing-patterns.md](references/framing-patterns.md). Choose the strongest frame available:

| Frame | When to Use | Structure |
|-------|------------|-----------|
| **Competitive** | You outperformed an alternative or competitor | [What we did] — [how it compares] |
| **Metric-Led** | You have numbers that beat a target | [Metric] — [vs. target/prior] — [what it means] |
| **Strategic** | Your work connects to an org priority | [Initiative headline] — [how this advances it] |

**Strongest approach:** Combine 2-3 frames in a single bullet when possible.

For each bullet, apply this structure:
1. **One-line headline** — strategic "what" (bolded)
2. **Why it matters** — business context in 1 sentence
3. **Evidence** — metric with comparison (vs. target, vs. last period, vs. competitor)

### Good vs. Bad Examples

**Bad (too operational):**
> - Closed 12 issues this week
> - Updated lab content
> - Had team meetings

**Good (right altitude, competitive frame):**
> **Hackathon registration pipeline on track for annual target**
> - Partner co-marketing registration opened; 400 signups in 48 hours (2x pace vs prior year)
> - Unified platform confirmed for Q4 flagship event

## Step 4: Assemble the Update

```
Week of [DATE]

[Team/Function] — [Name]

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
- [Only if strategic-level — omit section entirely if none]
```

### Formatting Rules
- **Depth:** 2 levels max (headline + one level of supporting bullets)
- **Length:** 3-5 strong bullets beats 10 weak ones, every time
- **Negative items:** Always "risk + mitigation," never "things went wrong"
- **Links:** Include dashboard or asset links for quick reference when useful
- **Light weeks:** "Steady execution — all programs tracking to plan" is a valid update. Don't pad.

## Reference Files

- **[references/altitude-check.md](references/altitude-check.md)** — Include/exclude table, the three-question altitude test, transformation examples, light-week protocol
- **[references/framing-patterns.md](references/framing-patterns.md)** — Competitive, metric-led, and strategic framing with examples, combination patterns, and negative-item framing

## Gotchas

- The most common mistake is including too much. Leadership reads 10+ updates weekly. Yours needs to be scannable in 30 seconds.
- Competitive context wins every time. "Our program outpaced the equivalent by 2x" gets remembered. "We had 400 signups" doesn't.
- Negative framing matters. "Risk: timeline slipped 1 week — mitigated by compressing review cycle; original deadline preserved" is professional. "We're late" is not.
- If you have nothing strategic to report, say so briefly. Padding with operational items to fill space actually undermines your credibility.
- Always submit, even during light weeks. Consistency builds trust. Silence creates uncertainty.

---

## Keywords

leadership update, weekly update, exec update, executive submission, leadership digest, weekly status, reframe for leadership, VP update, skip-level update, strategic update, what should I include, recurring update, weekly cadence, leadership reporting
