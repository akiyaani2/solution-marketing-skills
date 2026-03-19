---
name: stakeholder-brief
description: Generates tailored stakeholder communications — adapts the same information for different audiences (VP, skip-level, peers, team, partners). Use when you need to communicate the same update to multiple stakeholders at different levels, or when someone says "I need to brief [leader] on this."
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Stakeholder Brief Generator

Takes a single piece of information (program update, decision, announcement) and generates communications tailored to each stakeholder level. Same facts, different framing.

## Trigger Phrases

- `/stakeholder-brief`
- `/stakeholder-brief "NVIDIA hackathon results"`
- `brief [leader] on [topic]`
- `I need to update everyone on [topic]`
- `how do I communicate this to [audience]`

## Multi-Audience Output

Given a single input, generate communications for each relevant audience:

### For VP / Director (Ashley, Vivek)
- **Format:** 3-5 bullet points or half-page summary
- **Frame:** Business impact, metrics, strategic alignment
- **Tone:** Direct, confident, data-backed
- **Include:** Wins, risks (with mitigation), specific asks
- **Exclude:** Implementation details, team dynamics

### For Skip-Level (Cyril, Alysa)
- **Format:** 2-3 sentences or 3 bullet points
- **Frame:** Portfolio impact, competitive positioning
- **Tone:** Strategic, concise
- **Include:** Headline metric, strategic significance
- **Exclude:** Everything else (they'll ask if curious)

### For Peers (Heather, Marco)
- **Format:** Casual update, 3-5 bullet points
- **Frame:** What affects them, shared opportunities
- **Tone:** Collaborative, collegial
- **Include:** Cross-team implications, upcoming dependencies
- **Exclude:** Your internal team details

### For Team (Mindy, Amy, Erica, Sallie)
- **Format:** Detailed update with action items
- **Frame:** What changed, what it means for their work
- **Tone:** Clear, supportive, actionable
- **Include:** Specific tasks, timeline changes, context
- **Exclude:** Leadership politics, budget details (unless relevant)

### For Partners (NVIDIA, Skillable, etc.)
- **Format:** Professional email or brief
- **Frame:** Shared success, mutual benefit
- **Tone:** Collaborative, appreciative
- **Include:** Joint metrics, next steps, their contributions
- **Exclude:** Internal challenges, budget concerns

## Output Template

```markdown
## Stakeholder Brief: [Topic]

### The Facts (Ground Truth)
[3-5 bullet points of the objective facts. This is what's true regardless of audience.]

---

### For [VP Name]
[Tailored communication — format: email, Teams message, or talking points]

### For [Skip-Level Name]
[Tailored communication — format: 2-3 line summary]

### For [Peer Names]
[Tailored communication — format: casual update]

### For Team
[Tailored communication — format: detailed with action items]

### For [Partner Name]
[Tailored communication — format: professional email]
```

## Gotchas

- The facts don't change between audiences. Only the framing, detail level, and emphasis change.
- Never contradict yourself across stakeholder levels. If your VP hears "on track" and your team hears "at risk," you have a trust problem.
- The skip-level communication is the hardest because it must be short AND meaningful. If you can't say it in 3 bullets, you don't understand it well enough.
- For partner communications, always clear with your manager before sending anything that implies commitment (budget, timeline, scope).
- "What they need to know" vs "what you want to tell them" are different lists. Optimize for what they need.
- If you're uncomfortable with any audience version, that's a signal the core message needs work.
