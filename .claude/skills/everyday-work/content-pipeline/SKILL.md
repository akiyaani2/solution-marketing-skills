---
name: content-pipeline
description: Content production pipeline management — blogs, video episodes, session abstracts, social campaigns, competitive intel. Tracks from ideation through publication with stale content detection and per-channel views.
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/content-pipeline"
  - "/content-status"
  - "/content-pipeline --blog"
  - "/content-pipeline --video"
  - "/content-pipeline --social"
  - "content status"
  - "what content is in flight"
---

# Content Pipeline

Track all content production from ideation through publication. Covers blogs, video episodes, session abstracts, social campaigns, and competitive analysis. Surfaces stale content and provides per-channel views.

## Trigger Phrases

- `/content-pipeline` — Full content dashboard across all channels
- `/content-status` — Same as above (alias)
- `/content-pipeline --blog` — Blog content only
- `/content-pipeline --video` — Video/episode content only
- `/content-pipeline --social` — Social media campaigns
- `/content-pipeline --sessions [event]` — Session abstracts for a specific event
- `/content-pipeline --competitive` — Competitive intelligence content
- `/content-pipeline --stale` — Show only stale items (no update in 14+ days)

## What This Skill Does

### 1. Content Dashboard (`/content-pipeline`)

Pull all content-related issues and show pipeline status across all stages.

```bash
# All content issues
gh issue list --state open --label "func:content" --json number,title,labels,assignees,updatedAt --limit 200

# Recently published (closed in last 30 days)
gh issue list --state closed --label "func:content" --json number,title,labels,closedAt --limit 50 --search "closed:>$(date -v-30d '+%Y-%m-%d' 2>/dev/null || date -d '30 days ago' '+%Y-%m-%d')"
```

**Pipeline stages:**

| Stage | Label or Indicator | Description |
|-------|-------------------|-------------|
| **Ideation** | Status: Backlog | Concept proposed, not yet started |
| **In Production** | Status: In Progress | Actively being written/recorded/designed |
| **In Review** | Status: In Review | With reviewer(s), awaiting feedback |
| **Published** | Status: Done (closed) | Live and distributed |

**Output format:**

```
## Content Pipeline — [Date]

### In Production ([N] items)
| # | Title | Type | Owner | Due | Days in Stage |
|---|-------|------|-------|-----|--------------|

### In Review ([N] items)
| # | Title | Reviewer | Days in Review | Blocked? |
|---|-------|----------|----------------|----------|

### Ideation / Backlog ([N] items)
| # | Title | Owner | Priority | Channel |
|---|-------|-------|----------|---------|

### Recently Published (Last 30 Days)
| # | Title | Published | Channel | Owner |
|---|-------|-----------|---------|-------|

### Stale Content (No Update 14+ Days)
| # | Title | Last Updated | Days Stale | Owner |
|---|-------|-------------|-----------|-------|
```

### 2. Blog Pipeline (`/content-pipeline --blog`)

Filter to blog-specific content:

```bash
gh issue list --state open --label "func:content" --search "blog" --json number,title,labels,assignees,updatedAt --limit 100
```

**Per blog post, track:**
- Title / working title
- Author
- Target publication date
- Review status (draft, technical review, editorial review, approved)
- Target platform/channel (corporate blog, community blog, Medium, etc.)
- SEO keywords (if tracked)
- Related event or campaign

### 3. Video / Episode Pipeline (`/content-pipeline --video`)

Track video content production:

```bash
gh issue list --state open --label "func:content" --search "video OR episode OR show OR reactor" --json number,title,labels,body --limit 100
```

**Per episode/video, track:**

```
### [Episode Title]
| Field | Value |
|-------|-------|
| Series | [show name] |
| Guest(s) | [confirmed / pending] |
| Script/Outline | [draft / reviewed / final] |
| Recording Date | [date] |
| Air Date | [date] |
| Show Notes | [draft / published] |
| Social Promotion | [planned / posted] |
```

### 4. Social Campaigns (`/content-pipeline --social`)

Track social media content and campaigns:

```bash
gh issue list --state open --label "func:content" --search "social OR linkedin OR twitter" --json number,title,labels,body --limit 100
```

**Output format:**

```
### Social Content — [Date]

#### Planned Posts
| # | Topic | Platform | Scheduled | Tied To |
|---|-------|----------|-----------|---------|

#### Published This Week
| # | Topic | Platform | Date | Engagement |
|---|-------|----------|------|-----------|

#### Campaigns Active
| Campaign | Platform(s) | Duration | Posts Planned | Posts Published |
|----------|------------|----------|--------------|----------------|
```

### 5. Session Abstracts (`/content-pipeline --sessions [event]`)

Track session submission status for conferences and events:

```bash
gh issue list --state open --label "event:[event-label]" --search "session OR abstract OR CFP" --json number,title,labels,assignees --limit 100
```

**Output format:**

```
### Session Abstracts — [Event Name]

| # | Session Title | Speaker | Status | Track |
|---|--------------|---------|--------|-------|

#### Summary
- Submitted: [N]
- Accepted: [N]
- In Review: [N]
- Rejected: [N]
- Demo Required: [N] (of accepted)
```

### 6. Competitive Intelligence (`/content-pipeline --competitive`)

Track competitive analysis content status:

```bash
gh issue list --state open --label "func:content" --search "competitive OR competitor OR AWS OR GCP" --json number,title,labels,updatedAt --limit 50
```

**Output format:**

```
### Competitive Intel Status — [Date]

| Area | Last Updated | Status | Owner |
|------|-------------|--------|-------|
| Competitor A analysis | [date] | [current / stale / outdated] | [name] |
| Competitor B analysis | [date] | [current / stale / outdated] | [name] |

#### Staleness Thresholds
- Current: Updated within 30 days
- Stale: 30-60 days since update
- Outdated: 60+ days since update (flag for refresh)
```

### 7. Stale Content Detection (`/content-pipeline --stale`)

Surface content items that have not been updated recently:

```bash
# Find open content issues not updated in 14+ days
STALE_DATE=$(date -v-14d '+%Y-%m-%d' 2>/dev/null || date -d '14 days ago' '+%Y-%m-%d')
gh issue list --state open --label "func:content" --json number,title,assignees,updatedAt --limit 200 | jq "[.[] | select(.updatedAt < \"${STALE_DATE}\")]"
```

**Staleness tiers:**

| Tier | Days Without Update | Action |
|------|-------------------|--------|
| Attention | 14-21 days | Nudge owner, check if blocked |
| Warning | 21-30 days | Escalate to team lead, reassess priority |
| Critical | 30+ days | Close or reassign — content may be outdated |

## Content Types Reference

| Type | Typical Pipeline | Review Cycle | Notes |
|------|-----------------|-------------|-------|
| Blog post | 2-4 weeks | 1 technical + 1 editorial | SEO keywords matter |
| Video episode | 3-6 weeks | Script review + post-production | Guest scheduling is the bottleneck |
| Session abstract | 1-2 weeks per draft | Event-specific CFP review | Deadlines are hard — no extensions |
| Social post | 1-3 days | Quick approval | Tie to events/launches for max impact |
| Competitive brief | 2-4 weeks | Internal SME review | Refresh quarterly minimum |
| Case study | 4-8 weeks | Customer legal review | Longest lead time — start early |

## Gotchas

- **Content items may not have due dates.** Blogs and social posts are often "when ready" rather than hard-dated. Prioritize by staleness and event tie-ins rather than strict due dates.
- **Review cycles are where content stalls.** Track days-in-review separately from days-in-production. A blog post in review for 10+ days needs a nudge; for 20+ days, reassign the reviewer.
- **Video production has external dependencies.** Guest scheduling, recording studio availability, and post-production queues are outside your team's control. Build 2x buffer into video timelines.
- **Social content has a very short shelf life.** A social post planned 3 weeks ago about a trending topic is probably no longer relevant. Review social backlog weekly and aggressively close stale items.
- **Session abstract deadlines are immovable.** Conference CFP deadlines do not extend. If a session abstract is not submitted by the deadline, it is dead. Treat CFP deadlines as P0 regardless of other priorities.
- **Competitive intel goes stale faster than you think.** Cloud providers ship weekly. A competitive comparison older than 60 days is likely inaccurate on at least one major point. Build quarterly refresh cycles into the pipeline.
- **Content is often tracked across multiple systems.** Your CMS, social scheduling tool, video platform, and GitHub issues may all have different views of the same content. Designate GitHub issues as the source of truth for status, even if drafts live elsewhere.
- **Published is not the end of the pipeline.** Distribution, amplification, and performance tracking are post-publish activities. Consider adding a "Distributed" stage for high-priority content to ensure it gets amplified after going live.
