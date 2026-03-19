---
name: pressure-test
description: Challenges any plan, proposal, or decision — find the holes, ask the hard questions, surface risks, identify what could go wrong. For use when you need a devil's advocate before committing to a direction.
allowed-tools: [Read]
---

# Pressure Test

Be the devil's advocate. Take any plan, proposal, initiative, or decision and systematically challenge it. Find what's weak, what's missing, and what could fail.

## Trigger Phrases

- `/pressure-test`
- `/pressure-test #[issue-number]`
- `challenge this plan`
- `what am I missing`
- `poke holes in this`
- `devil's advocate on [topic]`

## What This Skill Does

When invoked, take the user's plan/proposal/idea and run it through these 7 challenge lenses:

### 1. Assumption Audit

List every assumption the plan makes. For each one, rate:
- **Confidence**: High / Medium / Low / Untested
- **Impact if wrong**: Critical / Significant / Minor
- **Evidence**: What data supports this assumption?

Flag any assumption that is both Low-confidence AND Critical-impact.

### 2. Resource Reality Check

- Does this plan account for who actually does the work?
- Are the same people assigned to multiple concurrent priorities?
- Is there slack for the unexpected?
- What happens if a key person is unavailable for 2 weeks?

### 3. Timeline Stress Test

- What are the hard deadlines vs. soft targets?
- Which dependencies are sequential vs. parallelizable?
- What's the critical path? What's the float?
- If the start slips 2 weeks, does the plan still work?
- Are there external dependencies with no SLA?

### 4. Stakeholder Blindspots

- Who needs to approve this that hasn't been consulted?
- Who will be affected that hasn't been informed?
- Who has veto power that isn't in the plan?
- What would [your manager / skip-level / exec sponsor] push back on?

### 5. Competitive Counter-Move

- If your competitor saw this plan, what would they do?
- Does this plan create a defensible advantage or just match the market?
- Is there a faster/cheaper/more impactful alternative you haven't considered?

### 6. Failure Modes

- What's the most likely way this fails?
- What's the most catastrophic way this fails?
- What's the sneaky failure mode nobody's thinking about?
- If this partially succeeds, is partial success still valuable?

### 7. Opportunity Cost

- What are you NOT doing by committing to this?
- Is there something with higher ROI that this displaces?
- Could the same resources produce more impact elsewhere?

## Output Format

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

## How To Use This Well

- **Before committing**: Run this on any proposal before presenting to leadership
- **Before escalating**: Pressure-test your escalation before it reaches your manager
- **During planning**: Challenge FY plans, event designs, program strategies
- **On someone else's plan**: "Pressure-test this proposal from [peer/team]"

## Gotchas

- This skill is most valuable when you're emotionally invested in the plan. That's exactly when you need it most.
- Don't skip the "What's Actually Strong" section. Pure criticism without acknowledging strengths is useless.
- If you ask for a pressure test and disagree with the findings, that's data — explain why and the skill will adjust.
- Combine with `/pre-mortem` for an even deeper challenge.
- The goal is to make the plan better, not to kill it. Recommend fixes, not just problems.
