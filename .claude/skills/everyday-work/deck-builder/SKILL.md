---
name: deck-builder
description: "Builds PowerPoint slide outlines and presentation content from bullet points, data, briefs, or raw notes. Use when the user wants to create slides, build a deck, prep a presentation, make a pitch, or turn any information into a visual format — even if they just say 'I need to present this' without mentioning slides. Also activate when someone shares meeting prep, status data, or program results and the output would be stronger as a deck than as a document."
allowed-tools: Read, Write, Edit, Bash
---

# Deck Builder

Turns rough ideas, bullet points, data, or briefs into structured slide content — ready to paste into PowerPoint or feed to Copilot for formatting. This skill creates content and structure, not .pptx files.

## When to Activate

Trigger on ANY of these signals, even if the user doesn't explicitly say "slides" or "deck":
- Direct: "build me a deck," "create slides," "make a presentation"
- Indirect: "I need to present this to [audience]," "turn this into something visual"
- Implied: user shares bullet points or data and mentions an upcoming meeting, review, or briefing
- Adjacent: "prep me for [meeting name]" when the meeting involves presenting

## Step 1: Route by Deck Type

Ask yourself: **What type of deck does the user need?**

| Signal | Template | Load From |
|--------|----------|-----------|
| Briefing, update, program review | Stakeholder Briefing (5-7 slides) | [references/templates.md](references/templates.md) |
| MBR, monthly review, business review | Monthly Business Review (8-10 slides) | [references/templates.md](references/templates.md) |
| Event overview, program pitch, one-pager | Event/Program One-Pager (2-3 slides) | [references/templates.md](references/templates.md) |
| None of the above | Custom / Freeform | [references/templates.md](references/templates.md) |

If unclear, ask: "Is this a leadership briefing, a monthly review, an event overview, or something else?"

## Step 2: Route by Audience

**Who is reading these slides?** This determines tone, depth, and slide count.

Load the full audience calibration from [references/audience-guide.md](references/audience-guide.md), then apply:

| Audience | Slide Count | Key Adjustment |
|----------|-------------|----------------|
| VP+ / Skip-level | 3-5 | Headlines only. Every slide answers "so what?" |
| Director / Sr. Director | 5-7 | Strategic + evidence. Include recommendation. |
| Team members | 8-12 | Tactical detail. Tasks, owners, timelines. |
| Partners | 5-7 | Mutual benefit framing. Shared metrics. |
| Peers | 3-5 | Only what affects them. Skip internal details. |

If the user hasn't specified the audience, ask. A deck for a VP looks completely different from a deck for the team — getting this wrong wastes the user's time.

## Step 3: Generate Content

Apply these rules to every slide:

1. **One idea per slide.** If a slide has 3 unrelated points, split it into 3 slides. This is the most common structural mistake.
2. **Titles are headlines, not labels.** "Registration Hit 150% of Target" not "Registration Update." Headlines tell the story even if the body is never read.
3. **Body bullets: 3-5 per slide max.** Each under 15 words. If a bullet needs a paragraph to explain, it belongs in speaker notes.
4. **Speaker notes for every slide.** This is where the presenter's narrative goes. The slides are the visual — the notes are the talk track.
5. **Visual suggestions for data slides.** Suggest specific chart types: "Bar chart comparing Q3 vs. Q4 registration," not just "add a chart."
6. **Quantify everything.** Replace "[X%]" placeholders with actual numbers from the user's input. Placeholder brackets make slides useless.

## Step 4: Deliver

Output format: Slide-by-slide markdown, following the template structure. Include:
- Slide number and type
- Title (headline format)
- Body bullets
- Visual suggestion (where applicable)
- Speaker notes

End with: "This is ready to paste into PowerPoint or feed to Copilot with your corporate template. Want me to adjust the depth, add slides, or change the audience level?"

## Reference Files

- **[references/templates.md](references/templates.md)** — Full slide-by-slide templates for all deck types
- **[references/audience-guide.md](references/audience-guide.md)** — Audience presets, tone calibration, and the "So What?" test

## Gotchas

- Always specify the audience before generating. Ask if the user hasn't.
- "Appendix" slides are where you put the data a VP might ask about. Always have backup slides ready.
- If using Copilot in PowerPoint: give it the generated outline and say "create slides from this outline using [template name]."
- Speaker notes are as important as slide content. The slides should be clean; the notes carry the narrative.
- For data-heavy decks, provide the actual numbers. Don't generate slides with empty brackets.

---

## Keywords

deck, slides, presentation, powerpoint, pptx, pitch, briefing, MBR, monthly review, business review, stakeholder update, event one-pager, copilot, present, slide outline, speaker notes, visual, build a deck, prep a deck, turn into slides, create a presentation
