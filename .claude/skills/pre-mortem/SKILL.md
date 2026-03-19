---
name: pre-mortem
description: Run a structured pre-mortem on any initiative — "It's 6 months from now and this failed completely. Why?" Surfaces risks before they happen by reasoning backwards from failure.
---

# Pre-Mortem

Imagine the project has already failed. Now figure out why. This inverts the usual planning optimism by starting from failure and working backwards to identify the causes.

## Trigger Phrases

- `/pre-mortem`
- `/pre-mortem #[issue-number]`
- `/pre-mortem "Build 2026 hackathon"`
- `run a pre-mortem on [topic]`
- `why would this fail`
- `what kills this project`

## What This Skill Does

### The Pre-Mortem Framework

**Setup:** "It's [timeframe] from now. [Initiative] has failed completely. Not partially — completely. The team is doing a post-mortem. What happened?"

### Step 1: Generate Failure Scenarios (5-8)

For each scenario, describe:
- **What happened**: The specific failure event
- **Root cause**: Why it happened (go 2-3 levels deep with "why")
- **Warning signs**: What signals would have predicted this failure?
- **When it became inevitable**: The point of no return

### Step 2: Categorize by Type

| Category | Description |
|----------|-------------|
| **Execution failure** | Team didn't deliver on time or quality |
| **Strategy failure** | Wrong bet, wrong market, wrong approach |
| **Dependency failure** | External party didn't come through |
| **Resource failure** | Ran out of budget, people, or time |
| **Political failure** | Lost exec support, got deprioritized, reorg |
| **Market failure** | Competitor moved, customer needs changed |
| **Technical failure** | Platform broke, integration failed, scale issue |

### Step 3: Rate Each Scenario

| Scenario | Likelihood | Impact | Detectability | Risk Score |
|----------|-----------|--------|--------------|-----------|
| [scenario] | High/Med/Low | High/Med/Low | Easy/Hard/Invisible | L x I x D |

**Detectability** is key — the most dangerous failures are the ones you can't see coming.

### Step 4: Identify the Top 3 Kill Shots

The three scenarios most likely to actually happen. For each:
- **Prevention plan**: What would stop this from happening?
- **Detection plan**: How would you know it's happening early?
- **Mitigation plan**: If it happens anyway, what's the damage control?

### Step 5: Early Warning System

Create a simple monitoring checklist:

```
## Early Warning Checklist — [Initiative]
Review weekly starting [date]:

- [ ] [Warning sign 1] — if yes, trigger [action]
- [ ] [Warning sign 2] — if yes, trigger [action]
- [ ] [Warning sign 3] — if yes, trigger [action]
```

## Output Format

```markdown
## Pre-Mortem: [Initiative Name]

**Scenario**: It's [month/year]. [Initiative] has failed. Here's what happened.

### Failure Scenarios

#### 1. [Scenario Name] — [Category]
**What happened**: [description]
**Root cause**: [why → why → why]
**Warning signs**: [what you'd see first]
**Point of no return**: [when it was too late]
**Likelihood**: [H/M/L] | **Impact**: [H/M/L] | **Detectability**: [Easy/Hard/Invisible]

[Repeat for 5-8 scenarios]

### Top 3 Kill Shots

1. **[Scenario]**
   - Prevent: [action]
   - Detect: [signal]
   - Mitigate: [fallback]

2. ...
3. ...

### Early Warning Checklist
- [ ] [sign] → [action]
- [ ] [sign] → [action]

### The One Thing Most Likely To Kill This
[Single sentence — the honest answer]
```

## When To Use This

- **Before kickoff**: Run on any major initiative before committing resources
- **At milestones**: Re-run when entering a new phase (e.g., moving from planning to execution)
- **When things feel too smooth**: If nothing seems risky, you're probably missing something
- **Before leadership presentations**: Anticipate the questions they'll ask

## Gotchas

- People resist pre-mortems because they feel negative. Frame it as "making the plan stronger," not "predicting doom."
- The best pre-mortem insights come from the scenario you think is "impossible." Push past comfortable risks.
- Political failures (exec support, reorgs, budget cuts) are the hardest to discuss but often the most impactful. Don't skip them.
- Re-run the pre-mortem if significant context changes (new competitor, budget shift, team change).
- Combine with `/pressure-test` for a comprehensive challenge. Pressure-test finds holes in the plan; pre-mortem imagines how those holes become disasters.
