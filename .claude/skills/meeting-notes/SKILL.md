---
name: meeting-notes
description: Meeting capture with auto-extraction of action items as GitHub Issues. Templates for 1:1s, team syncs, leadership prep, and recurring insights sessions. Organized file output by meeting type and date.
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/meeting-notes"
  - "/meeting-notes --1on1 [name]"
  - "/meeting-notes --team-sync"
  - "/meeting-notes --leadership-prep"
  - "capture meeting notes"
  - "meeting notes from"
---

# Meeting Notes

Capture notes from any meeting, structure them into a consistent format, extract action items, and optionally create GitHub Issues for follow-ups. Supports multiple meeting types with specialized templates.

## Trigger Phrases

- `/meeting-notes` — General meeting notes capture
- `/meeting-notes --1on1 [name]` — 1:1 meeting template
- `/meeting-notes --team-sync` — Team sync / standup template
- `/meeting-notes --leadership-prep` — Leadership meeting prep template
- `/meeting-notes --insights` — Insights / research session template
- `/meeting-notes --external [org]` — External / partner meeting template

## What This Skill Does

### 1. General Meeting Notes (`/meeting-notes`)

Capture unstructured notes and produce a structured document with extracted action items.

**Input:** The user provides raw notes (stream-of-consciousness, bullet points, or dictation).

**Output:**

```markdown
## Meeting Notes — [Meeting Name]
**Date**: [YYYY-MM-DD]
**Attendees**: [names]
**Type**: [1:1 / team sync / stakeholder / external / leadership]

### Key Discussion Points
1. [topic — 2-3 sentence summary]
2. [topic — 2-3 sentence summary]

### Decisions Made
- **[Decision ID]**: [decision statement] — Decided by: [name]
  - Context: [why this decision was made]
  - Impact: [what changes as a result]

### Action Items
| # | Action | Owner | Due | Priority | Issue Created? |
|---|--------|-------|-----|----------|---------------|
| 1 | [specific, actionable task] | [name] | [date] | [P0/P1/P2] | -- |
| 2 | [specific, actionable task] | [name] | [date] | [P0/P1/P2] | -- |

### Follow-ups for Next Meeting
- [item to revisit]
- [item pending external input]

### Parking Lot (Not Actionable Yet)
- [idea or topic to revisit later]
```

**Auto-create Issues for action items:**

```bash
# For each action item, offer to create a GitHub Issue
gh issue create \
  --title "[Meeting] [Action item text]" \
  --body "**Source:** Meeting notes from [date]
**Meeting:** [meeting name]
**Assigned in meeting to:** [name]
**Due:** [date]

## Context
[Relevant context from the discussion]

## Definition of Done
- [ ] [Specific completion criteria]" \
  --label "owner:[name],type:task" \
  --assignee "[github-username]"
```

### 2. 1:1 Meeting Notes (`/meeting-notes --1on1 [name]`)

Specialized template for 1:1 conversations:

```markdown
## 1:1 Notes — [Your Name] + [Their Name]
**Date**: [YYYY-MM-DD]
**Cadence**: [weekly / biweekly / monthly]

### Their Updates
- [what they shared about their work]
- [wins or completions to acknowledge]
- [challenges or blockers they raised]

### Your Updates
- [context or decisions you shared]
- [organizational updates relevant to them]

### Feedback Exchange
- **Recognition**: [specific positive feedback given]
- **Growth Area**: [constructive feedback or coaching point]
- **Feedback Received**: [what they shared with you]

### Their Asks / Requests
| Request | Priority | Your Response | Follow-up Needed? |
|---------|----------|--------------|------------------|
| [request] | [H/M/L] | [approved / exploring / declined] | [Y/N] |

### Career / Development (if discussed)
- [development goals, training, stretch assignments]

### Agreed Next Steps
| Action | Owner | Due |
|--------|-------|-----|
| [action] | [name] | [date] |

### Notes for Next 1:1
- [items to follow up on next time]
```

### 3. Team Sync Notes (`/meeting-notes --team-sync`)

Template for team standup or sync meetings:

```markdown
## Team Sync — [Date]
**Attendees**: [names]
**Duration**: [N] minutes

### Per-Person Updates

#### [Person 1]
- **Completed**: [items finished since last sync]
- **In Progress**: [current focus areas]
- **Blocked**: [blockers needing team help]
- **Next**: [upcoming priorities]

#### [Person 2]
- **Completed**: [items]
- **In Progress**: [items]
- **Blocked**: [items]
- **Next**: [items]

### Team-Wide Items
- [announcements]
- [cross-team dependencies surfaced]
- [calendar conflicts or upcoming deadlines]

### Blockers Requiring Escalation
| Blocker | Owner | Blocked Since | Escalation Path |
|---------|-------|-------------|----------------|

### Action Items
| Action | Owner | Due |
|--------|-------|-----|
```

### 4. Leadership Prep Notes (`/meeting-notes --leadership-prep`)

Template for preparing leadership meetings (MBR, QBR, director/VP briefings):

```markdown
## Leadership Prep — [Meeting Name]
**Date**: [YYYY-MM-DD]
**Audience**: [leadership names and titles]
**Format**: [presentation / discussion / review]

### Narrative Themes
1. **[Theme]**: [talking points — frame positively, lead with impact]
2. **[Theme]**: [talking points — data-driven, concise]

### Wins to Highlight
- [win with specific metrics] — Impact: [why it matters to leadership]
- [win with specific metrics] — Impact: [why it matters to leadership]

### Risks to Surface
| Risk | Impact | Mitigation | Ask |
|------|--------|-----------|-----|
| [risk] | [H/M/L] | [what you are doing] | [what you need from them] |

### Asks / Decisions Needed
1. [specific ask with clear decision criteria]
2. [specific ask with options and recommendation]

### Data Gaps
- [metrics you need but do not have]
- [information you are still gathering]

### Anticipated Questions and Prepared Answers
| Likely Question | Prepared Answer |
|----------------|----------------|
| [question] | [concise answer with data] |

### Post-Meeting Follow-ups
- [items to send after the meeting]
```

### 5. Insights Session Notes (`/meeting-notes --insights`)

Template for research, competitive, or trend-analysis sessions:

```markdown
## Insights Session — [Topic]
**Date**: [YYYY-MM-DD]
**Source**: [internal research / industry report / competitive scan / customer feedback]

### Key Insights
1. **[Insight Title]**: [description]
   - **Implication for our work**: [how this affects programs, content, strategy]
   - **Recommended Action**: [what to do about it]
   - **Urgency**: [act now / plan for next quarter / monitor]

2. **[Insight Title]**: [description]
   - **Implication**: [impact]
   - **Action**: [recommendation]
   - **Urgency**: [timeline]

### Market Signals
- [trend or data point worth tracking]
- [emerging pattern]

### Competitive Moves
| Competitor | Move | Our Position | Action Needed |
|-----------|------|-------------|--------------|
| [name] | [what they did] | [where we stand] | [response] |

### Content Ideas Generated
| Idea | Channel | Priority | Owner |
|------|---------|----------|-------|
| [blog topic] | Blog | [H/M/L] | [name] |
| [video idea] | Video | [H/M/L] | [name] |
| [hack theme] | Hackathon | [H/M/L] | [name] |

### Action Items
| Action | Owner | Due |
|--------|-------|-----|
```

### 6. External / Partner Meeting (`/meeting-notes --external [org]`)

Template for meetings with partners, vendors, or external stakeholders:

```markdown
## External Meeting — [Organization Name]
**Date**: [YYYY-MM-DD]
**Our Attendees**: [names]
**Their Attendees**: [names, titles]
**Purpose**: [topic / agenda]

### Discussion Summary
[Structured summary of key points discussed]

### Commitments Made
| Commitment | By Whom | To Whom | Due | Notes |
|-----------|---------|---------|-----|-------|

### Commitments Received
| Commitment | From Whom | Due | Follow-up |
|-----------|----------|-----|-----------|

### Items Requiring Internal Review
- [anything discussed that needs internal alignment before acting]

### Next Steps
| Action | Owner | Due |
|--------|-------|-----|

### Internal-Only Notes (Do Not Share)
- [candid assessment of meeting]
- [strategic observations]
- [concerns or risks]
```

## File Organization

Save notes to your meetings directory with consistent naming:

```
operations/meetings/1on1-[name]-YYYY-MM-DD.md
operations/meetings/team-sync-YYYY-MM-DD.md
operations/meetings/leadership-prep-YYYY-MM-DD.md
operations/meetings/insights-[topic]-YYYY-MM-DD.md
operations/meetings/external-[org]-YYYY-MM-DD.md
operations/meetings/[custom-meeting]-YYYY-MM-DD.md
```

```bash
# Create the file
MEETING_TYPE="team-sync"
MEETING_DATE=$(date '+%Y-%m-%d')
FILE_PATH="operations/meetings/${MEETING_TYPE}-${MEETING_DATE}.md"

# Check if directory exists
mkdir -p operations/meetings
```

## Meeting Cadence Reference (Customize)

| Meeting | Frequency | Template | Prep Skill |
|---------|-----------|----------|-----------|
| Team Sync | Weekly | `--team-sync` | `/standup` |
| 1:1 with Reports | Biweekly | `--1on1 [name]` | `/1on1-prep [name]` |
| 1:1 with Manager | Weekly | `--1on1 [name]` | `/1on1-prep [name]` |
| Leadership Review | Monthly | `--leadership-prep` | `/mbr` |
| Peer Team Sync | Weekly/Biweekly | General | `/x-sp --prep` |
| Insights Session | Weekly/As-needed | `--insights` | N/A |
| External/Partner | As-needed | `--external [org]` | N/A |

## Gotchas

- **Raw notes from meetings are often stream-of-consciousness.** The user may dump an unstructured wall of text. Your job is to parse it into the structured template — identify action items, decisions, and topics even when they are not explicitly labeled.
- **Action items must be specific and actionable.** "Follow up on budget" is not an action item. "Send revised budget proposal to [name] by Friday" is. Transform vague items into specific ones, and ask the user to confirm if you are unsure about the owner or due date.
- **1:1 notes are private.** Do not post 1:1 content to GitHub Issues or public channels. Save to the meetings directory only. Action items can become Issues, but strip personal/sensitive context from the issue body.
- **Decisions made in meetings should be formalized.** If a significant decision was made, flag it for the user to create a formal decision record (e.g., via a `/decision` skill). Meeting notes are not the system of record for decisions.
- **External meeting notes have two audiences.** The "Internal-Only Notes" section is for your team's eyes only. If the notes will be shared with the external party, strip that section first. Always clarify with the user before sharing externally.
- **Meeting notes lose value exponentially over time.** Notes captured same-day retain 90% of context. Notes captured next-day retain 60%. Notes captured a week later are mostly reconstructed. Encourage same-day capture.
- **Not every meeting needs notes.** Quick check-ins, status pings, and informal chats do not need formal documentation. Reserve structured meeting notes for meetings where decisions are made, action items are assigned, or information needs to be shared with non-attendees.
- **The auto-create-issues feature should be opt-in.** Present the action items table and ask the user which items should become GitHub Issues. Do not auto-create issues for every action item — some are too small, some are already tracked, and some are delegated verbally.
