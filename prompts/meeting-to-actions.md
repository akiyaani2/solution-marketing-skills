# Meeting Notes to Action Items

**Category:** Meeting & Communication
**Best for:** Program managers, team leads, anyone who runs meetings
**Works in:** Copilot Chat, Claude.ai, ChatGPT, any AI assistant

---

## The Prompt

> Extract all action items from these meeting notes. For each action item, provide:
> - **Action:** What specifically needs to be done (start with a verb)
> - **Owner:** Who is responsible (use names mentioned, or "[unassigned]" if unclear)
> - **Due date:** When it's due (use dates mentioned, or "[needs date]" if unclear)
> - **Priority:** High / Medium / Low based on context (urgency language, blocking others, leadership attention)
> - **Context:** One sentence on why this matters or what it relates to
>
> Also flag:
> - Any decisions that were made (so they can be documented)
> - Any open questions that were NOT resolved
> - Any items that seem like they need escalation or approval
>
> Meeting notes:
>
> [Paste your meeting notes or transcript here]

## Example

**Input:**
Meeting notes from weekly team sync, March 12:

talked about the partner event next month. Sarah said she'd handle the speaker list but needs the budget approved first. John mentioned the venue contract is stuck in legal — been there 2 weeks. We agreed to cap attendance at 200. Marketing collateral needs to be done by March 25 but nobody volunteered. Someone should check if the streaming platform license renewed. Also the Q2 report is due to leadership on the 20th — Maria is drafting but needs data from analytics team by Friday.

**Output:**

### Action Items

| # | Action | Owner | Due | Priority | Context |
|---|--------|-------|-----|----------|---------|
| 1 | Finalize speaker list for partner event | Sarah | [needs date] | High | Blocked until budget is approved |
| 2 | Escalate venue contract stuck in legal review | John | Mar 14 | High | Has been stalled 2 weeks; event is next month |
| 3 | Get budget approval for partner event | [unassigned] | ASAP | High | Blocking speaker list and likely other event logistics |
| 4 | Create marketing collateral for partner event | [unassigned] | Mar 25 | Medium | No owner volunteered — needs assignment |
| 5 | Verify streaming platform license renewal status | [unassigned] | Mar 15 | Medium | Risk of lapsed license before event |
| 6 | Send analytics data to Maria for Q2 report | Analytics team | Mar 15 (Fri) | High | Blocking leadership deliverable due Mar 20 |
| 7 | Complete Q2 leadership report draft | Maria | Mar 20 | High | Leadership deadline |

### Decisions Made
- Attendance capped at 200 for the partner event

### Open Questions
- Who owns the marketing collateral? Needs assignment.
- Who is responsible for getting budget approved?
- Is the streaming platform license still active?

### Escalation Flags
- Venue contract in legal for 2 weeks with event next month — may need leadership nudge

## Variations

- **For GitHub Issues:** Add: "Format each action item as a GitHub issue with a title (imperative sentence), body (context + acceptance criteria), and suggested labels."
- **For team standup:** Add: "Group action items by person so each team member can see their list at a glance."
- **Shorter version:** "Extract action items from these notes. Table format: Action | Owner | Due Date. Flag anything unassigned or overdue."

## Tips for Better Results

- Paste raw notes exactly as-is — do not clean them up first. The AI handles messy input well.
- If working from a transcript, include speaker names so the AI can assign owners correctly
- After getting results, review the "[unassigned]" items immediately — those are your follow-up list
