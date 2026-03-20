---
name: exec-summary
description: "Distills complex information into leadership-ready executive summaries — from detailed documents, meeting notes, reports, or raw data. Use when someone needs 'the bottom line,' a TL;DR, a brief for leadership, or wants to condense anything into a concise format. Also activate when the user shares a long document and asks 'what should I tell my boss?' or needs to summarize before forwarding — even if they don't say 'executive summary' explicitly."
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Executive Summary Generator

Takes any detailed input — long documents, meeting transcripts, data dumps, status reports, email threads — and distills it into a leadership-ready executive summary using the Minto Pyramid Principle: answer first, evidence second, details on request.

## When to Activate

Trigger on ANY of these signals:
- Direct: "executive summary," "summarize for leadership," "exec brief"
- Indirect: "TL;DR this," "bottom line," "what's the headline?"
- Implied: user shares a long document and asks "what should I tell [leader]?" or "can you condense this?"
- Adjacent: "I need to forward this to my VP" — the user needs a summary layer on top of the raw content

## Step 1: Route by Format

Ask yourself: **How much space does the user have?**

| Signal | Format | Load From |
|--------|--------|-----------|
| Email body, Teams message, quick FYI | Half-Page (6-10 lines) | [references/formats.md](references/formats.md) |
| Document attachment, decision memo, pre-read | One-Page (250-400 words) | [references/formats.md](references/formats.md) |
| "Give me the TLDR," verbal briefing prep | Three-Bullet (3 sentences) | [references/formats.md](references/formats.md) |

If unclear, default to Half-Page — it works for most situations. If the user's audience is CVP+, default to Three-Bullet.

## Step 2: Route by Audience

**Who is the reader?** This determines what to include and what to cut.

| Audience | What They Want | What to Skip |
|----------|---------------|-------------|
| CVP+ | Impact, strategic alignment, portfolio view | Implementation details, team names, process |
| Sr. Director | Program health, risks, recommendation | Tactical task lists, individual ticket details |
| Director (your manager) | Execution detail, blockers, what they need to unblock | Strategic framing they already know |
| Peers | Cross-team impact, dependencies | Your internal team details |

If the user hasn't specified the audience, ask. A summary for a CVP and a summary for a peer are fundamentally different documents.

## Step 3: Apply the Pyramid Principle

Load the full framework from [references/pyramid-principle.md](references/pyramid-principle.md). The core rule:

**Lead with the answer. Support with arguments. Back with evidence. Details on request.**

```
              [ANSWER]           <- 1 sentence: the bottom line
            /    |    \
      [Arg 1] [Arg 2] [Arg 3]   <- 3-5 key findings (bolded headlines)
       / \      / \      / \
    [E1] [E2] [E3] [E4] [E5]    <- Metrics, data, examples
```

### Writing Rules (apply to all formats):
1. **Lead with the answer.** "We should approve the budget because..." not "This document examines the budget considerations..."
2. **Quantify everything.** "Registrations are up" becomes "Registrations hit 2,400, up 45% from last quarter."
3. **No setup paragraphs.** Delete "As you know..." and "The purpose of this summary is..."
4. **Active voice.** "The team delivered 3 programs" not "3 programs were delivered."
5. **Specify the ask.** Every summary ends with what you need: approval, decision, FYI, input.
6. **Recommendation is required.** Executives don't want a menu — they want your recommendation with supporting logic.

## Step 4: Deliver

Output the summary in the chosen format. Then offer:
- "Want me to adjust the length, change the format, or shift the audience level?"
- "Want me to add a recommendation section?" (if one wasn't included)
- "I can also generate this as a deck — say `/deck-builder` to convert."

## Reference Files

- **[references/formats.md](references/formats.md)** — Half-page, one-page, and three-bullet templates with rules and a selection guide
- **[references/pyramid-principle.md](references/pyramid-principle.md)** — Full explanation of the Minto Pyramid approach with examples and common mistakes

## Gotchas

- The hardest part is deciding what to LEAVE OUT. If you include everything, you've summarized nothing.
- If you can't state the bottom line in one sentence, you don't understand the topic well enough yet. Go back to the source material.
- Match length to audience seniority. CVP+ gets three bullets. Director gets one page.
- Never start with "Background" or "Context." Start with the answer. Add context only if the reader needs it to understand the answer.
- Test your summary: cover the bottom line with your hand. Can someone get the key message from the rest? If yes, the bottom line is redundant. If no, the rest needs work.

---

## Keywords

executive summary, exec summary, TL;DR, TLDR, bottom line, brief, summarize, condense, distill, leadership summary, what should I tell, headline, one-pager, half-page, pyramid principle, key takeaways, elevator pitch, synopsis
