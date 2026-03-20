# Raw Data to Executive Insights

**Category:** Data & Analysis
**Best for:** Program managers, analysts, anyone presenting metrics to leadership
**Works in:** Copilot Chat, Claude.ai, ChatGPT, any AI assistant

---

## The Prompt

> I have raw data/metrics that I need to present to [audience level: VP / director / team]. Transform this data into an executive-ready narrative that includes:
>
> 1. **Headline insight** — The single most important takeaway, stated in one sentence
> 2. **Trend summary** — What's improving, declining, or flat (compare to prior period if data available)
> 3. **"So what" interpretation** — Why these numbers matter for the business. Connect metrics to outcomes.
> 4. **Risk flags** — Anything in the data that should concern the reader
> 5. **Recommended actions** — 2-3 specific next steps based on what the data shows
>
> Format for a [slide / email / document]. Use plain language, not jargon. Lead with insight, not methodology.
>
> Here is the data:
>
> [Paste your data, metrics, or numbers here]

## Example

**Input:**
Audience: VP. Format for a slide.

Data from our developer skilling programs, Q2 vs Q1:
- Hackathon registrations: 4,200 (Q2) vs 3,100 (Q1)
- Hackathon completions: 1,680 (Q2) vs 1,085 (Q1)
- Completion rate: 40% (Q2) vs 35% (Q1)
- Post-event Azure activations: 312 (Q2) vs 198 (Q1)
- Cost per activation: $87 (Q2) vs $142 (Q1)
- Partner co-funded events: 6 (Q2) vs 2 (Q1)
- NPS score: 72 (Q2) vs 68 (Q1)

**Output:**

**Headline:** Developer hackathons drove 57% more Azure activations in Q2 while cutting cost-per-activation by 39%.

**What's Working**
- Registration volume grew 35% QoQ, signaling strong demand for hands-on skilling formats
- Completion rates improved 5 points (35% to 40%), suggesting better content and engagement design
- Partner co-funded events tripled (2 to 6), reducing our net cost exposure

**The Business Impact**
- 312 new Azure activations at $87 each — the most cost-efficient acquisition channel in the skilling portfolio
- At current trajectory, Q3 projects to ~420 activations, putting us ahead of the annual target by one quarter

**Watch Closely**
- 60% of registrants still do not complete — there is significant upside in conversion optimization
- Partner co-funding is growing fast but concentrated with one partner; diversification needed

**Recommended Actions**
1. Invest in completion nudges (email sequences, mentorship) to push completion rate toward 50%
2. Expand partner co-funding model to 2-3 additional partners in Q3
3. Present this data to cross-solution peers as a repeatable playbook

## Variations

- **For VP audience:** Add: "Keep to one slide worth of content. Use the headline as the slide title. Bullet points only — no paragraphs. Include one recommended chart type."
- **For team audience:** Add: "Include more detail on what drove the changes. Add a 'what we learned' section. Tone can be more conversational."
- **Shorter version:** "Give me a 3-sentence executive summary of this data: the headline number, why it matters, and what to do next. Data: [paste data]"

## Tips for Better Results

- Always include a comparison period (last quarter, last year, target) — insights require context
- Name the audience explicitly; a VP summary differs dramatically from a team retrospective
- If you want a specific chart recommended, mention that you are building a slide deck
