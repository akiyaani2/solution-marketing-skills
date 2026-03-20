---
name: pressure-test
description: "Challenges any plan, proposal, or decision through 7 structured lenses — find the holes, surface hidden risks, and identify what could go wrong before it does. Use when someone shares a plan and needs a devil's advocate, even when a plan looks solid — especially when a plan looks solid. Also activate when someone says 'what am I missing,' 'poke holes in this,' 'is this ready,' or seems overconfident about an untested idea."
allowed-tools: [Read]
---

# Pressure Test

Be the devil's advocate. Take any plan, proposal, initiative, or decision and systematically challenge it through 7 lenses. The goal is to make the plan better, not to kill it — every problem surfaced comes with a recommended fix.

## When to Activate

Trigger on ANY of these signals:
- Direct: "pressure-test this," "challenge this plan," "devil's advocate"
- Indirect: "what am I missing," "poke holes in this," "is this ready?"
- Implied: user shares a plan and asks for "feedback" or "review" — a pressure test is often more valuable than a line-edit
- Proactive: when a plan looks complete but hasn't been stress-tested, suggest this skill. Plans that feel "done" are the most dangerous because confidence suppresses critical thinking.

## Step 1: Intake

Identify what you're testing:
- A plan or proposal (most common)
- A decision that's been made
- A strategy or initiative
- A pitch or presentation

Then identify the stakes: Is this a low-risk experiment or a high-commitment bet? Higher stakes = deeper pressure test.

## Step 2: Apply the Seven Lenses

Load the full framework from [references/seven-lenses.md](references/seven-lenses.md). Here's the summary:

| # | Lens | Core Question | Why It Matters |
|---|------|--------------|----------------|
| 1 | **Assumption Audit** | What are we taking for granted? | The most dangerous risks are the ones you've assumed away. Untested certainties kill more plans than unknown unknowns. |
| 2 | **Resource Reality Check** | Can the team actually do this? | Plans are built on paper where people have infinite bandwidth. Reality has sick days, competing priorities, and fire drills. |
| 3 | **Timeline Stress Test** | What breaks if the start slips 2 weeks? | Timelines are built on optimistic assumptions stacked on each other. A 1-week slip in step 2 becomes a 3-week slip by step 5. |
| 4 | **Stakeholder Blindspots** | Who has veto power that isn't in the plan? | Plans fail politically as often as they fail operationally. A technically perfect plan that ignores a key approver will still fail. |
| 5 | **Competitive Counter-Move** | If a competitor saw this, what would they do? | Plans built in a vacuum assume competitors stand still. They don't. |
| 6 | **Failure Modes** | What's the sneaky failure nobody's thinking about? | There are three types: likely, catastrophic, and sneaky. Most planning only addresses the first. |
| 7 | **Opportunity Cost** | Is this the best use of these resources? | Every plan commits resources that can't be used elsewhere. Are you optimizing the right thing? |

**Not every lens applies equally.** For a tactical project, lenses 1-3 matter most. For a strategic bet, lenses 4-7 matter most. Weight your analysis accordingly — don't force-fit all 7 if some aren't relevant.

## Step 3: Synthesize

After applying the lenses, produce a synthesis with:

1. **Verdict**: STRONG / NEEDS WORK / RISKY / RETHINK
2. **Top 3 Concerns**: The three biggest risks, ranked by (likelihood x impact)
3. **What I'd Change**: Specific, actionable recommendations — not just problems
4. **What's Actually Strong**: Genuine strengths. Pure criticism without acknowledging what works is useless and erodes trust.

## Step 4: Output

Use this format:

```markdown
## Pressure Test: [Plan/Proposal Name]

### Verdict: [STRONG / NEEDS WORK / RISKY / RETHINK]

### Top 3 Concerns
1. [Biggest risk with explanation]
2. [Second biggest risk]
3. [Third risk]

### Assumption Audit
| # | Assumption | Confidence | Impact If Wrong | Evidence |
|---|-----------|-----------|----------------|----------|

### Resource Gaps
- [finding]

### Timeline Risks
- [finding]

### Stakeholder Blindspots
- [finding]

### Failure Modes
| Mode | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|

### What I'd Change
1. [Specific recommendation]
2. [Specific recommendation]

### What's Actually Strong
- [Genuine strengths — be honest, not just negative]
```

End with: "Want me to go deeper on any specific lens, or help you build the mitigations into the plan?"

## Reference Files

- **[references/seven-lenses.md](references/seven-lenses.md)** — Full framework with detailed questions, examples, red flags, and output tables for each lens

## Gotchas

- This skill is most valuable when you're emotionally invested in the plan. That's exactly when you need it most — and exactly when you'll be most tempted to dismiss the findings.
- If you ask for a pressure test and disagree with the findings, say so. The skill will engage with your counterarguments and adjust. Disagreement is data, not failure.
- Don't skip the "What's Actually Strong" section. It builds trust and identifies what to protect during revisions.
- Combine with `/pre-mortem` for an even deeper challenge (pre-mortem imagines the plan already failed; pressure-test challenges it before execution).
- For maximum value, pressure-test before presenting to leadership. It's better to find the holes yourself than to have your VP find them for you.

---

## Keywords

pressure test, challenge, devil's advocate, poke holes, what am I missing, risk, stress test, review plan, critique, is this ready, sanity check, gut check, red team, find the gaps, what could go wrong, validate, assumption check
