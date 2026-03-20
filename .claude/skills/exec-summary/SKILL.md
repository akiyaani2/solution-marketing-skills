---
name: exec-summary
description: Generates executive summaries from detailed documents, meeting notes, or data. Distills complex information into 1-page or half-page leadership-ready summaries. Use when preparing briefs for leadership, condensing reports, or when someone asks for "the TL;DR" or "the bottom line."
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Executive Summary Generator

Takes any detailed input — long documents, meeting transcripts, data dumps, status reports — and distills it into a leadership-ready executive summary.

## Trigger Phrases

- `/exec-summary`
- `/exec-summary "Q3 program results"`
- `summarize this for leadership`
- `give me the executive summary`
- `TL;DR this for leadership`
- `bottom line on [topic]`

## Summary Formats

### Half-Page Summary (for email or Teams message)

```
SUBJECT: [Topic] — [Headline Takeaway]

Bottom line: [1 sentence — the single most important thing]

Key points:
• [Point 1 with metric]
• [Point 2 with metric]
• [Point 3 with metric]

Action needed: [What you need from the reader, or "FYI only"]

Timeline: [Next milestone or deadline]
```

### One-Page Summary (for documents or attachments)

```
# [Topic] — Executive Summary

## Bottom Line
[2-3 sentences. Lead with the conclusion, not the context.]

## Key Findings
1. **[Finding]**: [Evidence/metric]
2. **[Finding]**: [Evidence/metric]
3. **[Finding]**: [Evidence/metric]

## Risks
- [Risk 1]: [Mitigation]
- [Risk 2]: [Mitigation]

## Recommendation
[What you're recommending and why. Be specific.]

## Next Steps
| Action | Owner | By When |
|--------|-------|---------|

## Appendix Available
[Link or reference to the full document for those who want details]
```

### Pyramid Principle Structure

For complex topics, use the Minto Pyramid:

1. **Answer first**: Lead with the conclusion
2. **Key supporting arguments**: 3-5 grouped logically
3. **Evidence for each**: Data, examples, analysis
4. **Details available on request**: Don't bury the lead

## Writing Rules

1. **Lead with the answer.** "We should approve the $100K budget because..." not "This document examines the budget considerations for..."
2. **Quantify everything.** "Registrations are up" → "Registrations hit 2,400, up 45% from last quarter."
3. **No setup paragraphs.** Delete "As you know..." and "The purpose of this summary is..."
4. **Active voice.** "The team delivered 3 programs" not "3 programs were delivered by the team."
5. **One page max.** If it's longer, you haven't summarized — you've shortened.
6. **Specify the ask.** Every summary should end with what you need from the reader: approval, decision, FYI, input.

## Audience Calibration

| Audience Level | What They Want | What to Include | What to Skip |
|---------------|---------------|----------------|-------------|
| CVP+ | Impact, strategic alignment | Portfolio-level metrics, competitive context | Implementation details |
| Sr. Director | Program health, risks, asks | Program metrics, team performance, budget | Tactical task lists |
| Director | Execution detail, blockers | Per-person status, specific blockers, timelines | Strategic framing (he knows the strategy) |
| Peers (cross-team leads) | Cross-team impact | Shared dependencies, joint timelines | Your internal team details |

## Gotchas

- The hardest part of an executive summary is deciding what to LEAVE OUT. If you include everything, you've summarized nothing.
- "Recommendation" is not optional. Executives don't want a menu of options — they want your recommendation with supporting logic.
- If you can't state the bottom line in one sentence, you don't understand the topic well enough yet.
- Match length to audience seniority. CVP+ gets half a page. Director gets one page.
- Never start with "Background" or "Context." Start with the answer. Add context only if the reader needs it to understand the answer.
- Test your summary: cover the bottom line with your hand. Can someone get the key message from the rest? If yes, the bottom line is redundant. If no, the rest is redundant.
