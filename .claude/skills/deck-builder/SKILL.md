---
name: deck-builder
description: Builds PowerPoint slide outlines and content from bullet points, data, or a brief. Generates structured slide-by-slide content ready to paste into PowerPoint or Copilot. Use when creating presentations, decks, pitch materials, MBR slides, or stakeholder briefings.
allowed-tools: Read, Write, Edit, Bash
---

# Deck Builder

Turns rough ideas, bullet points, or data into structured PowerPoint slide content. Outputs slide-by-slide outlines with titles, body content, speaker notes, and visual suggestions — ready to paste into PowerPoint or feed to Copilot for formatting.

## Trigger Phrases

- `/deck-builder`
- `/deck-builder "MBR for March"`
- `/deck-builder --template stakeholder-brief`
- `build me a deck on [topic]`
- `create slides for [meeting/event]`
- `turn this into a presentation`

## How It Works

1. User provides: topic, audience, key points (or a data source)
2. Skill generates: slide-by-slide outline with content
3. User pastes into PowerPoint or feeds to Copilot for design

This skill does NOT create .pptx files directly. It creates the **content and structure** that goes into slides. Your team can then:
- Paste into PowerPoint manually
- Give to Copilot in PowerPoint to auto-format
- Use a corporate template and fill in the generated content

## Deck Templates

### Stakeholder Briefing (5-7 slides)

```
Slide 1: TITLE
  Title: [Initiative/Program Name]
  Subtitle: Briefing for [Audience] | [Date]
  Notes: [Context for presenter]

Slide 2: EXECUTIVE SUMMARY
  Title: At a Glance
  Body:
    - Status: [Green/Yellow/Red]
    - Key metric: [number with context]
    - Key metric: [number with context]
    - Key decision needed: [if any]
  Notes: [Lead with the headline — what does leadership need to know?]

Slide 3: PROGRESS
  Title: What We've Accomplished
  Body:
    - [Win 1 with metric]
    - [Win 2 with metric]
    - [Win 3 with metric]
  Visual: Bar chart or timeline showing progress
  Notes: [Celebrate wins first — frame positively]

Slide 4: CURRENT STATE
  Title: Where We Are Now
  Body:
    - In Progress: [items]
    - Blocked: [items with owners]
    - Coming Next: [items]
  Visual: Kanban or pipeline view
  Notes: [Be honest about blockers — leadership respects transparency]

Slide 5: RISKS & ASKS
  Title: What We Need
  Body:
    - Risk 1: [description] → Ask: [what you need]
    - Risk 2: [description] → Ask: [what you need]
  Notes: [Every risk should have a proposed mitigation and a clear ask]

Slide 6: NEXT STEPS
  Title: Looking Ahead
  Body:
    - [Next milestone with date]
    - [Next milestone with date]
    - [Decision point with date]
  Notes: [End with forward momentum, not problems]

Slide 7: APPENDIX (optional)
  Title: Supporting Data
  Body: [Detailed metrics, comparison tables, backup slides]
```

### Monthly Business Review (8-10 slides)

```
Slide 1: TITLE — MBR [Month Year]
Slide 2: EXECUTIVE SUMMARY — 3-5 key headlines
Slide 3: WINS — What shipped/completed this month
Slide 4: BY THE NUMBERS — Key metrics with MoM trends
Slide 5: BY TEAM MEMBER — What each person delivered
Slide 6: PROGRAM DEEP-DIVE — Spotlight on 1-2 programs
Slide 7: PIPELINE — What's coming next month
Slide 8: RISKS & BLOCKERS — What needs leadership attention
Slide 9: ASKS — Budget, approvals, decisions needed
Slide 10: APPENDIX — Detailed data tables
```

### Event/Program One-Pager (2-3 slides)

```
Slide 1: OVERVIEW
  Title: [Event/Program Name]
  Body:
    - What: [1-line description]
    - When: [date(s)]
    - Who: [audience]
    - Goal: [target metric]
    - Status: [Green/Yellow/Red]

Slide 2: DETAILS
  Body:
    - Program structure
    - Partners involved
    - Budget summary
    - Timeline

Slide 3: ASK (if needed)
  Body:
    - What we need from leadership
    - Decision deadline
```

## Content Generation Rules

1. **One idea per slide.** If a slide has 3 unrelated points, split it.
2. **Titles are headlines, not labels.** "Registration Hit 150% of Target" not "Registration Update."
3. **Body bullets: 3-5 per slide max.** Each under 15 words.
4. **Speaker notes for every slide.** This is where the detail goes.
5. **Visual suggestions for data slides.** "Bar chart showing X" or "Timeline of Y."
6. **Audience-appropriate language.** VP+ sees strategy and metrics. Team sees tasks and details.

## Audience Presets

| Audience | Tone | Focus | Slide Count |
|----------|------|-------|-------------|
| VP/Director | Strategic, metric-driven | Outcomes, risks, asks | 5-7 |
| Skip-level (VP+) | Executive, headline-only | Impact, alignment, decisions | 3-5 |
| Team members | Tactical, detail-rich | Tasks, timelines, owners | 8-12 |
| Partners | Professional, collaborative | Shared goals, joint metrics | 5-7 |
| Peers (cross-team leads) | Casual, cross-team | Shared items, dependencies | 3-5 |

## Gotchas

- This skill creates content, not .pptx files. The output is structured text meant to be pasted into PowerPoint or Copilot.
- Always specify the audience before generating. A deck for a CVP looks very different from a deck for the team.
- "Appendix" slides are where you put the data your VP might ask about. Always have backup data ready.
- If using Copilot in PowerPoint: give it the generated outline and say "create slides from this outline using [template name]."
- Speaker notes are as important as slide content. The slides should be clean; the notes carry the narrative.
- For data-heavy decks, provide the actual numbers. Placeholder brackets ("[X%]") make slides useless.
