---
name: talking-points
description: Generates structured talking points for meetings, presentations, leadership conversations, or stakeholder calls. Includes main points, supporting data, anticipated questions with answers, and things to avoid saying. Use before any important meeting or conversation where you need to be prepared.
allowed-tools: Read, Grep, Glob
---

# Talking Points Generator

Prepares you for any conversation with structured talking points, supporting data, anticipated Q&A, and landmines to avoid.

## Trigger Phrases

- `/talking-points`
- `/talking-points "1:1 with my manager about hackathon program"`
- `/talking-points --prep-for director`
- `prep me for my meeting with [name]`
- `what should I say about [topic]`
- `help me prepare for [meeting]`

## Talking Points Template

```markdown
## Talking Points: [Topic/Meeting]
**Meeting with:** [Name(s)] | **Date:** [Date]
**Your goal for this conversation:** [1 sentence — what do you want to walk out with?]

---

### Opening (30 seconds)
[How to start the conversation. Set context. Lead with the headline.]

### Main Points (3-5 max)

**1. [Point]**
- Key message: [1 sentence]
- Supporting data: [specific metric or fact]
- Transition to next point: [bridging phrase]

**2. [Point]**
- Key message: [1 sentence]
- Supporting data: [specific metric or fact]
- Transition: [bridge]

**3. [Point]**
- Key message: [1 sentence]
- Supporting data: [specific metric or fact]

### The Ask
[What you want from this conversation. Be specific. "I'd like your approval on X" or "I need your input on Y"]

### Anticipated Questions & Answers

**Q: [Question they're likely to ask]**
A: [Your prepared response with data]

**Q: [Question they're likely to ask]**
A: [Your prepared response with data]

**Q: [Tough question you hope they don't ask]**
A: [Your honest, prepared response]

### Landmines — Do NOT Say
- [Topic or phrase to avoid and why]
- [Commitment you shouldn't make]
- [Comparison that could backfire]

### Closing
[How to end the conversation. Summarize agreements. Confirm next steps.]
```

## Meeting-Type Presets

### Leadership Update (Director/VP)
- Lead with wins, then risks
- Frame everything in terms of business impact
- Have backup data ready but don't present it unless asked
- End with a specific ask or next step

### Skip-Level (VP+)
- 2-3 talking points max (they have 5 minutes of attention)
- Lead with the most strategic point
- Competitive context is valuable at this level
- Don't get into operational details unless asked

### Peer Sync (cross-team leads)
- Lead with what affects them
- Ask what they need from you
- Share upcoming conflicts or dependencies
- Keep it conversational, not formal

### Team Member 1:1
- Start by asking how they're doing
- Review their top 2-3 items
- Give specific feedback (FIRE framework)
- Ask what they need from you

### Partner Meeting (partner/vendor)
- Lead with shared goals and mutual benefit
- Quantify what you've delivered together
- Propose next steps that benefit both sides
- Never commit budget or resources without checking first

## Gotchas

- 3 talking points is better than 7. You won't remember 7.
- The "Anticipated Q&A" section is where the real prep value is. Think about the uncomfortable questions.
- "Landmines" matter. Knowing what NOT to say is as important as knowing what to say.
- For leadership meetings: have your ask ready BEFORE the meeting. Don't figure it out mid-conversation.
- If you're nervous about a meeting, rehearse the opening and the ask. The middle takes care of itself.
- Write talking points the day before, not 5 minutes before. Fresh eyes catch gaps.
