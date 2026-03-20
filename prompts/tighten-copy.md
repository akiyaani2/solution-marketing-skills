# Tighten Copy to Half Length

**Category:** Review & Editing
**Best for:** Anyone who writes emails, docs, or decks that run too long
**Works in:** Copilot Chat, Claude.ai, ChatGPT, any AI assistant

---

## The Prompt

> Cut this text to approximately half its current length. Rules:
>
> - Preserve every key fact, number, and decision — cut filler, not substance
> - Remove hedging language ("I think," "it seems like," "potentially")
> - Combine redundant sentences
> - Use active voice throughout
> - Keep the original structure (headings, bullets) unless restructuring makes it shorter
> - Tone: [executive / casual / technical]
>
> Show me the tightened version, then list what you removed and why (so I can verify nothing important was cut).
>
> Text to tighten:
>
> [Paste your text here]

## Example

**Input:**
Tone: executive.

Text: "I wanted to reach out and provide an update on where we currently stand with the partner integration project. As you may recall, we kicked this off back in January and the original timeline had us completing the first phase by the end of March. I think we're generally tracking well against that timeline, although there have been some challenges along the way. The API integration is fully complete and has been tested, which is great news. The documentation is about 80% done — we still need to finish the authentication section and the troubleshooting guide. The main blocker right now is that we're still waiting on the partner to provide their production credentials, which we've been asking about for the past three weeks. If we don't get those credentials by March 20, we'll likely need to push the launch date back by at least two weeks. I'd recommend that someone from leadership reach out to their VP of Engineering to escalate this. Other than that, the team is feeling pretty good about the quality of the work and we're confident in the technical implementation."

**Output (tightened):**

Partner integration update: Phase 1 was due end of March and is on track with one risk.

**Complete:** API integration built and tested.
**In progress:** Documentation at 80% — authentication and troubleshooting sections remain.
**Blocked:** Partner has not provided production credentials despite three weeks of requests. If not received by March 20, launch slips two weeks.

**Ask:** Leadership escalation to partner's VP of Engineering to unblock credentials.

---

**What was removed:**
- "I wanted to reach out and provide an update" — filler opening (no information)
- "As you may recall, we kicked this off back in January" — backstory the audience already knows
- "I think we're generally tracking well although there have been some challenges" — hedging; replaced with direct status
- "which is great news" — editorializing
- "the team is feeling pretty good about the quality" — sentiment without substance

## Variations

- **For executive audience:** Add: "Maximum 100 words. Lead with the decision or ask. Use bullet points, not paragraphs."
- **For casual/team audience:** Add: "Keep a conversational tone. It's fine to be informal, just be concise."
- **For technical audience:** Add: "Keep all technical specifics (versions, endpoints, configurations). Cut business context and pleasantries."
- **Shorter version:** "Make this half as long. Keep all facts and numbers. Remove filler. Text: [paste text]"

## Tips for Better Results

- Always ask the AI to list what it removed — this is your safety net for catching accidentally deleted substance
- If the result is still too long, run it through the prompt again with "cut this by another 30%"
- Specify the audience; what counts as "filler" depends entirely on who is reading it
