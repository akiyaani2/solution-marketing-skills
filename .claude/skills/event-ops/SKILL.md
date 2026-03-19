---
name: event-ops
description: Full event lifecycle management — logistics tracking, workback schedule generation, vendor coordination checklists, readiness scoring, and cross-event calendar views for solution marketing teams
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/event-ops"
  - "/event-ops [event-name]"
  - "/event-ops --workback"
  - "event logistics"
  - "event readiness"
  - "workback schedule"
---

# Event Operations

Manage the full lifecycle of marketing events from concept through post-event analysis. Covers logistics tracking, workback schedule generation, vendor coordination checklists, readiness scoring, and cross-event calendar views.

## Trigger Phrases

- `/event-ops` — Full event dashboard across all active events
- `/event-ops [event-name]` — Deep dive on a specific event
- `/event-ops --workback "[event]" "[YYYY-MM-DD]"` — Generate workback schedule
- `/event-ops --vendor [event]` — Vendor coordination checklist
- `/event-ops --calendar` — Cross-event calendar view
- `/event-ops --readiness [event]` — Readiness score for a specific event

## What This Skill Does

### 1. Event Dashboard (`/event-ops` or `/event-ops [event-name]`)

Pull all event-related issues and generate an operational dashboard.

**Discovery — find your event issues:**

```bash
# All open event issues (adjust repo owner/name)
gh issue list --state open --label "func:events" --json number,title,labels,assignees,milestone --limit 200

# Specific event (use your event label convention)
gh issue list --state open --label "event:[event-label]" --json number,title,labels,assignees --limit 100

# Include hackathon-type events
gh issue list --state open --label "func:hackathon" --json number,title,labels,assignees --limit 200
```

**Output format:**

```
## Event Dashboard — [Date]

### [Event Name]
| Field | Value |
|-------|-------|
| Event Date | [YYYY-MM-DD] |
| Days Out | [N] |
| Readiness | [X]% ([completed]/[total] tasks) |
| Owner | [name] |
| Budget Status | [approved / pending / at-risk] |

#### Tasks by Status
- Completed: [N]
- In Progress: [N]
- Blocked: [N] — [list blocked items]
- Not Started: [N]

#### Next 7 Days
| # | Task | Owner | Due |
|---|------|-------|-----|

#### Overdue
| # | Task | Owner | Was Due | Days Late |
|---|------|-------|---------|-----------|
```

### 2. Workback Schedule Generator (`/event-ops --workback "[event]" "[date]"`)

Given an event name and target date, generate a standard workback schedule. Milestones are calculated backwards from the event date.

**Default milestone template (configurable):**

| Milestone | Offset | Default Owner | Notes |
|-----------|--------|---------------|-------|
| Event concept and goals defined | -12 weeks | Team lead | Scope, audience, success metrics |
| Partner outreach and commitments locked | -10 weeks | Partner lead | Sponsorships, co-marketing, MDF |
| Budget approval | -10 weeks | Team lead | Route through approval chain |
| Content call-for-proposals | -8 weeks | Content lead | Session abstracts, demos, labs |
| Registration page live | -6 weeks | Event coordinator | Platform configured, tracking active |
| Content lock and final review | -4 weeks | All content owners | No new content after this date |
| Promotion campaign launch | -4 weeks | Marketing lead | Email, social, paid if applicable |
| Speaker and partner confirmation | -3 weeks | Event coordinator | Final headcount, logistics |
| Dry run and tech check | -1 week | All | Rehearsals, AV check, platform test |
| Event live | 0 | All | Execution day(s) |
| Post-event metrics and recap | +1 week | Analytics lead | Attendance, engagement, leads |

**To create GitHub Issues from the workback:**

```bash
# Calculate dates from event date and create issues
EVENT_DATE="2026-06-01"
EVENT_NAME="Annual Conference 2026"

# Example: Create the -12 week milestone
MILESTONE_DATE=$(date -v-12w -j -f "%Y-%m-%d" "$EVENT_DATE" "+%Y-%m-%d" 2>/dev/null || date -d "$EVENT_DATE - 12 weeks" "+%Y-%m-%d")

gh issue create \
  --title "[$EVENT_NAME] Event concept and goals defined" \
  --body "**Event:** $EVENT_NAME
**Event Date:** $EVENT_DATE
**Milestone Due:** $MILESTONE_DATE (-12 weeks)

## Checklist
- [ ] Define target audience and size
- [ ] Set success metrics (registrations, engagement, leads)
- [ ] Confirm event format (in-person, virtual, hybrid)
- [ ] Identify key stakeholders and approvers
- [ ] Draft budget estimate" \
  --label "func:events,type:task"
```

**Workback for different event types:**

| Event Type | Lead Time | Key Differences |
|------------|-----------|-----------------|
| Tentpole conference | 12-16 weeks | Session CFPs, booth logistics, large speaker roster |
| Hackathon | 8-12 weeks | Platform setup, challenge design, judging panel, prizes |
| Workshop / hands-on lab | 6-8 weeks | Lab environment provisioning, content testing |
| Webinar / virtual event | 4-6 weeks | Shorter cycle, platform booking, recording setup |
| Partner co-marketing event | 10-12 weeks | Joint approvals, co-branding, MDF paperwork |

### 3. Vendor Coordination Checklist (`/event-ops --vendor [event]`)

Generate vendor coordination checklists by event type.

**Hackathon vendor checklist:**
- [ ] Partner agreement signed (co-marketing, IP, sponsorship)
- [ ] Platform confirmed and provisioned (hackathon platform, cloud resources)
- [ ] Registration system configured and tested
- [ ] Challenge descriptions finalized and reviewed
- [ ] Judging criteria published
- [ ] Prize structure confirmed and funded
- [ ] Compute/cloud resources provisioned (GPU, sandbox environments)
- [ ] Post-hack data collection plan defined
- [ ] Participant communication templates ready
- [ ] Judging panel recruited and confirmed

**Tentpole conference vendor checklist:**
- [ ] Session abstracts submitted by deadline
- [ ] Speaker slots confirmed with event organizers
- [ ] Demo environments provisioned and tested
- [ ] Booth/activation materials designed and ordered
- [ ] Lab content reviewed and tested end-to-end
- [ ] Attendee swag and giveaways ordered (lead time: 6-8 weeks)
- [ ] Post-event follow-up campaign designed
- [ ] On-site staffing schedule confirmed
- [ ] AV and tech requirements submitted
- [ ] Contingency plan documented

**Workshop vendor checklist:**
- [ ] Lab platform provisioned (subscriptions, environments)
- [ ] Content tested by someone other than the author
- [ ] Instructor/facilitator confirmed
- [ ] Participant prerequisites documented
- [ ] Backup content prepared (if labs fail)
- [ ] Feedback survey configured
- [ ] Certification/badge integration set up (if applicable)

### 4. Cross-Event Calendar View (`/event-ops --calendar`)

Pull all events and generate a consolidated timeline.

```bash
# Pull all event-labeled issues
gh issue list --state open --label "func:events" --json number,title,labels,body --limit 200
gh issue list --state open --label "func:hackathon" --json number,title,labels,body --limit 200

# Also check closed recent events for context
gh issue list --state closed --label "func:events" --json number,title,labels,closedAt --limit 50
```

**Output format:**

```
## Event Calendar — [Current Quarter]

### [Month]
| Date | Event | Type | Owner | Readiness | Conflicts |
|------|-------|------|-------|-----------|-----------|

### Conflict Detection
- [Flag overlapping events]
- [Flag same-owner overload: >2 events in 2-week window]
- [Flag resource conflicts: shared vendors, platforms, budgets]
```

### 5. Readiness Scoring (`/event-ops --readiness [event]`)

Score event readiness on a 0-100 scale:

| Category | Weight | Scoring Criteria |
|----------|--------|-----------------|
| Content | 25% | Sessions submitted, demos ready, labs tested |
| Logistics | 25% | Venue/platform confirmed, AV, registration live |
| Speakers/Staff | 20% | Confirmed, prepped, backup identified |
| Budget | 15% | Approved, POs issued, no open risks |
| Promotion | 15% | Campaign launched, registration tracking to target |

**Readiness thresholds:**
- 90-100%: On track
- 70-89%: Needs attention — review gaps
- 50-69%: At risk — escalate to team lead
- Below 50%: Critical — requires leadership intervention

## Gotchas

- **Event dates in issue bodies are free text.** Look for patterns like `Due: YYYY-MM-DD`, `Event Date: YYYY-MM-DD`, or dates in the title. There is no guaranteed structured field — parse carefully.
- **Multi-day events need clear handling.** Use the first day as the anchor date for countdowns and workback calculations. Document the full date range in the issue body.
- **Workback schedules should account for your fiscal calendar.** Fiscal quarters may not align with calendar quarters. Adjust milestone offsets if holidays or fiscal boundaries fall in the workback window.
- **Co-marketing events have dual approval chains.** Your internal approval plus the partner's approval. Build 2-3 extra weeks into the workback for joint sign-off.
- **Content lock date vs. event date are different things.** The workback should target content lock, not the event date. Content lock is typically 2-4 weeks before the event.
- **Vendor lead times vary wildly.** Swag and printed materials need 6-8 weeks. Digital assets need 2-3 weeks. Cloud provisioning can take 1-4 weeks depending on the platform. Always confirm lead times early.
- **Registration numbers are lagging indicators.** A registration page going live at -6 weeks with zero sign-ups at -5 weeks is normal. Panic threshold is typically -3 weeks with less than 30% of target.
- **Post-event is where most teams drop the ball.** Build the +1 week recap into the workback as a hard milestone, not optional. Metrics collected within 7 days are 10x more actionable than metrics collected at 30 days.
