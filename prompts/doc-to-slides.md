# Document to Slide Outline

**Category:** Presentation
**Best for:** Anyone building a deck from existing written content
**Works in:** Copilot Chat, Claude.ai, ChatGPT, any AI assistant

---

## The Prompt

> Turn this document into a [5 / 7 / 10]-slide outline for a presentation to [audience]. For each slide, provide:
>
> 1. **Slide title** — Written as a headline that makes a claim or states a takeaway (not a label like "Overview" or "Background")
> 2. **3-4 bullet points** — Key content for the slide body. Each bullet should be one line, not a paragraph.
> 3. **Speaker notes** — 2-3 sentences of what the presenter should SAY (not read) for this slide
> 4. **Visual suggestion** — What chart, image, or layout would strengthen this slide
>
> Rules:
> - Every slide title should pass the "so what" test — if someone only reads titles, they should understand the full story
> - No slide should have more than 4 bullets
> - The final slide should be a clear call to action, not "Questions?"
>
> Document:
>
> [Paste your document text here]

## Example

**Input:**
5 slides for a director-level audience.

Document (abbreviated): "Our developer hackathon program has grown from 4 events in Q1 to 12 events in Q3. Registration has increased 3x. Partner co-funding now covers 40% of costs. Post-event Azure activation rates are 7.4%, which is 2x higher than webinar-driven activations. We're proposing to double the program in FY27 with three new partners and a dedicated platform..."

**Output:**

**Slide 1: Hackathons Are Our Highest-Converting Developer Channel**
- 12 events in Q3, up from 4 in Q1 (3x growth)
- 7.4% Azure activation rate — 2x higher than webinars
- Partner co-funding covers 40% of program cost
- NPS of 72 across all events

*Speaker notes:* Open with the activation comparison. This audience cares about cost-efficient pipeline, so lead with the 2x conversion stat before showing growth. Mention that this is the only channel where partners pay to participate.

*Visual:* Side-by-side bar chart comparing activation rates across channels (hackathons, webinars, labs, docs).

---

**Slide 2: Three Factors Driving Hackathon Growth**
- Hands-on format matches how developers actually learn
- GPU credit incentives attract advanced practitioners
- Partner co-investment creates shared accountability for outcomes

*Speaker notes:* Briefly explain WHY this works, not just that it works. The GPU credits point often surprises people — it is the single biggest driver of advanced-track registrations.

*Visual:* Three icons or pillars layout. Simple and clean.

---

*(continues through Slides 3-4 to...)*

**Slide 5: Approving the FY27 Expansion Unlocks $2M in Partner Co-Investment**
- Proposal: 24 events across 4 partners (up from 12 events, 1 partner)
- Projected 900+ Azure activations at $74 cost-per-activation
- Partner commitments contingent on FY27 confirmation by April 15
- Ask: Budget approval of $[amount] with partner offset of 45%

*Speaker notes:* This is the ask slide. Emphasize the time constraint — partners have committed but need confirmation by mid-April. Frame the budget as net-of-partner, not gross.

*Visual:* Simple table showing gross cost, partner offset, and net investment. Include the April 15 deadline prominently.

## Variations

- **For VP/executive audience:** Add: "Maximum 5 slides. Every title must state a business outcome. Remove any slide that is purely informational without a decision or insight."
- **For team/working audience:** Add: "You can go up to 10 slides. Include more detail and process slides. Add a timeline or roadmap slide."
- **Shorter version:** "Give me 5 slide titles (as headlines, not labels) that tell the story of this document. Each title should pass the 'so what' test. Document: [paste text]"

## Tips for Better Results

- Always specify your audience — a deck for your VP is structurally different from one for your team
- The best results come from documents that already have data; if your doc is all narrative, add key numbers before prompting
- Use the slide titles as your first quality check: read just the titles in order. If they tell a coherent story, the outline is strong.
