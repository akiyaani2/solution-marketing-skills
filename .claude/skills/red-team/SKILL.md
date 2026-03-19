---
name: red-team
description: Takes the competitor's perspective — if you were AWS or GCP, how would you counter this program? Use to stress-test positioning, identify defensive gaps, and sharpen competitive strategy.
allowed-tools: [Read]
---

# Red Team

Switch sides. Think like AWS, GCP, or any competitor. How would they counter your program, steal your audience, or exploit your gaps?

## Trigger Phrases

- `/red-team`
- `/red-team --aws "our hackathon program"`
- `/red-team --gcp "our certification strategy"`
- `red team this from AWS perspective`
- `what would Google do`
- `how would a competitor beat this`

## What This Skill Does

### The Red Team Exercise

**Role:** You are now the competitor's strategy team. Your job is to beat [the user's program/initiative].

### Step 1: Intelligence Gathering

What does the competitor already know about your program?
- Public announcements and blog posts
- Event registrations and speaker lineups
- Partner relationships (visible)
- Job postings (signals strategic direction)
- Community buzz and developer sentiment

### Step 2: Competitive Counter-Moves

Generate 5-7 specific moves the competitor could make:

```markdown
| # | Counter-Move | Difficulty | Impact on You | Timeline |
|---|-------------|-----------|--------------|----------|
| 1 | [specific action] | Easy/Med/Hard | High/Med/Low | [when] |
| 2 | ... | | | |
```

### Step 3: Your Vulnerabilities

What weaknesses would a smart competitor exploit?

- **Positioning gaps**: Where your messaging is weak or claims are unsubstantiated
- **Experience gaps**: Where the competitor's developer experience is genuinely better
- **Speed gaps**: Where they can move faster (fewer approvals, more resources)
- **Partner gaps**: Relationships they have that you don't
- **Community gaps**: Where their developer community is stronger

### Step 4: Asymmetric Advantages

What can YOU do that the competitor CAN'T easily replicate?

- Unique partnerships
- Platform advantages
- Data or distribution advantages
- Organizational strengths
- Timing advantages

### Step 5: Defensive Recommendations

For each vulnerability, recommend a specific defense:

```markdown
| Vulnerability | Competitor Exploit | Your Defense | Priority |
|-------------|-------------------|-------------|----------|
| [gap] | [what they'd do] | [what you should do] | P0/P1/P2 |
```

## Output Format

```markdown
## Red Team Analysis: [Your Program] vs [Competitor]

### Competitor's Likely Assessment of You
[2-3 sentences — how they'd describe your program's strengths and weaknesses]

### Top 5 Counter-Moves They'd Make
1. **[Move]**: [description, timeline, impact]
2. ...

### Your Exposed Vulnerabilities
1. [vulnerability + evidence]

### Your Asymmetric Advantages
1. [advantage they can't easily copy]

### Defensive Playbook
| Priority | Action | Blocks Which Counter-Move |
|----------|--------|--------------------------|

### The Honest Question
[One uncomfortable question this analysis raises that the team should discuss]
```

## When To Use This

- **Before program launches**: Red-team the launch plan
- **Before competitive positioning**: Test your claims from the other side
- **Quarterly strategy reviews**: What changed in the competitive landscape?
- **After competitor announcements**: "They just launched X — what does that mean for us?"

## Gotchas

- The value of red-teaming is proportional to how uncomfortable the findings are. If everything looks fine, you're not trying hard enough.
- Don't dismiss competitor strengths because "our product is better." Developers choose ecosystems for many reasons beyond product quality.
- Red-teaming is not pessimism — it's preparation. The goal is to find and fix gaps before competitors exploit them.
- Pair with `/competitive-scan` for data-grounded analysis. Red-team adds the strategic reasoning layer.
- Be specific. "AWS might do something similar" is useless. "AWS could launch a free GPU hackathon program using SageMaker Studio Lab by Q4" is actionable.
