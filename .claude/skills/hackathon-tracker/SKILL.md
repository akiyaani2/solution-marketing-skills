---
name: hackathon-tracker
description: End-to-end hackathon program tracking — registration pipeline, submission tracking, judging status, cross-hack comparison, outcomes analysis, and funnel metrics for solution marketing hackathon programs
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/hack-status"
  - "/hack-status [hack-name]"
  - "/hack-status --all"
  - "hackathon pipeline"
  - "hack status"
---

# Hackathon Tracker

End-to-end hackathon program management. Tracks the lifecycle from planning through registration, execution, judging, and outcomes analysis. Supports both one-off and recurring hackathon programs.

## Trigger Phrases

- `/hack-status` — Pipeline dashboard for all active hacks
- `/hack-status [hack-name]` — Deep dive on a specific hackathon
- `/hack-status --all` — All hacks including completed
- `/hack-status --registrations` — Registration funnel across all hacks
- `/hack-status --compare` — Cross-hack comparison table
- `/hack-status --judging` — Judging status for hacks in that phase
- `/hack-status --outcomes [hack-name]` — Post-hack outcomes report

## What This Skill Does

### 1. Hack Pipeline Dashboard (`/hack-status` or `/hack-status --all`)

Pull all hackathon issues and generate a pipeline view showing each hack's progress through stages.

```bash
# All hackathon issues
gh issue list --state open --label "func:hackathon" --json number,title,labels,assignees,body --limit 200

# Include recently closed hacks for context
gh issue list --state closed --label "func:hackathon" --json number,title,labels,closedAt --limit 50
```

**Output format per hack:**

```
## [Hack Name] — [Status Icon]
Owner: [name] | Epic: #[N] | Event Date: [date] | Days Out: [N]

Pipeline:
  Planning      [##########] 100%  (5/5 tasks)
  Registration  [####------]  40%  (2/5 tasks)
  Content       [##--------]  20%  (1/5 tasks)
  Execution     [----------]   0%  (0/3 tasks)
  Judging       [----------]   0%  (0/2 tasks)
  Outcomes      [----------]   0%  (0/2 tasks)

Blocked: [list] | At-Risk: [list]
```

**Pipeline stages defined:**

| Stage | Key Milestones | Typical Duration |
|-------|---------------|-----------------|
| **Planning** | Goals set, platform chosen, partners confirmed, budget approved | 4-8 weeks |
| **Registration** | Page live, promo launched, targets set, tracking active | 3-6 weeks |
| **Content** | Challenges designed, resources published, mentor guides ready | 2-4 weeks |
| **Execution** | Hack live, support active, submissions flowing | 1-5 days (or ongoing) |
| **Judging** | Panel confirmed, criteria published, scoring complete | 1-3 weeks |
| **Outcomes** | Winners announced, metrics compiled, recap published | 1-2 weeks |

### 2. Individual Hack Deep-Dive (`/hack-status [hack-name]`)

For a specific hack, show comprehensive status:

```bash
# Get the parent epic
gh issue view [EPIC_NUMBER] --json number,title,body,labels,assignees,state

# Get all child issues (via body links or label)
gh issue list --state all --label "event:[hack-label]" --json number,title,labels,assignees,state,body --limit 100
```

**Deep-dive output:**

```
## [Hack Name] — Full Status

### Overview
| Field | Value |
|-------|-------|
| Epic | #[N] |
| Owner | [name] |
| Platform | [platform name] |
| Format | [virtual / in-person / hybrid] |
| Duration | [dates] |
| Partner | [partner name, if applicable] |
| Budget Status | [approved / pending] |

### Task Breakdown
| # | Task | Owner | Status | Due |
|---|------|-------|--------|-----|

### Registration Numbers
| Metric | Value |
|--------|-------|
| Target | [N] |
| Registered | [N] |
| Conversion to Target | [X]% |

### Partner Status
- [ ] Agreement signed
- [ ] Co-marketing approved
- [ ] Resources provisioned
- [ ] Branding approved

### Platform Readiness
- [ ] Platform provisioned
- [ ] Challenges loaded
- [ ] Test submissions verified
- [ ] Judging workflow configured

### Content / Challenge Readiness
- [ ] Challenge descriptions finalized
- [ ] Starter kits / boilerplate ready
- [ ] Documentation and resources published
- [ ] Mentor guide distributed

### Judging
- [ ] Panel recruited ([N]/[target] judges)
- [ ] Criteria published
- [ ] Scoring rubric configured
- [ ] Winner notification plan ready
```

### 3. Registration Funnel Tracking (`/hack-status --registrations`)

Aggregate registration data across all active hacks.

```
## Registration Funnel — [Date]

| Hack | Target | Registered | Started | Submitted | Completion Rate |
|------|--------|-----------|---------|-----------|----------------|
| [name] | [N] | [N] ([X]%) | [N] | [N] | [X]% |
| [name] | [N] | [N] ([X]%) | [N] | [N] | [X]% |

### Funnel Health
- [hack]: On track (registered > 60% of target with 3+ weeks remaining)
- [hack]: Needs boost (registered < 40% of target with < 3 weeks remaining)
- [hack]: At risk (registered < 20% of target with < 2 weeks remaining)

### Recommended Actions
- [hack]: Increase promotion cadence — send reminder campaign
- [hack]: Consider extending deadline or lowering target
```

### 4. Cross-Hack Comparison (`/hack-status --compare`)

Compare all active hacks on key dimensions:

```
## Cross-Hack Comparison — [Date]

| Dimension | [Hack 1] | [Hack 2] | [Hack 3] |
|-----------|----------|----------|----------|
| Owner | [name] | [name] | [name] |
| Quarter | [Q] | [Q] | [Q] |
| Format | virtual | in-person | hybrid |
| Partner Funded? | Yes | No | Partial |
| Platform | [name] | [name] | [name] |
| Reg. Target | [N] | [N] | [N] |
| Reg. Actual | [N] | [N] | [N] |
| Readiness % | [X]% | [X]% | [X]% |
| Budget | $[N] | $[N] | $[N] |
| Status | On track | At risk | Planning |
```

### 5. Judging Status (`/hack-status --judging`)

For hacks currently in or approaching judging phase:

```
## Judging Status — [Date]

### [Hack Name]
| Field | Value |
|-------|-------|
| Submissions Received | [N] |
| Judging Panel | [N]/[target] confirmed |
| Criteria Published | Yes / No |
| Scoring Method | [rubric / ranking / elimination] |
| Judging Deadline | [date] |
| Winners Announced | Yes / No |

#### Judges
| Name | Title | Status |
|------|-------|--------|
| [name] | [title] | Confirmed / Pending |

#### Scoring Criteria
| Criterion | Weight |
|-----------|--------|
| Innovation | [X]% |
| Technical Implementation | [X]% |
| Business Impact | [X]% |
| Presentation Quality | [X]% |
```

### 6. Outcomes Report (`/hack-status --outcomes [hack-name]`)

Post-hack analysis template for completed hacks:

```
## Outcomes Report — [Hack Name]

### Funnel
| Stage | Count | Conversion |
|-------|-------|-----------|
| Registered | [N] | 100% (baseline) |
| Started | [N] | [X]% |
| Submitted | [N] | [X]% |
| Qualified | [N] | [X]% |

### Demographics
- Geography: [breakdown]
- Company Size: [breakdown]
- Role: [breakdown]

### Technology Usage
| Technology / Service | Projects Using | % of Total |
|---------------------|---------------|-----------|
| [service] | [N] | [X]% |

### Partner Outcomes (if applicable)
- Partner satisfaction: [rating]
- Co-marketing impressions: [N]
- Leads generated for partner: [N]

### Certifications / Badges Earned
| Credential | Count |
|-----------|-------|
| [cert name] | [N] |

### Lessons Learned
1. [What worked well]
2. [What to improve]
3. [What to stop doing]

### Recommendations for Next Iteration
1. [recommendation]
2. [recommendation]
```

## Gotchas

- **Registration numbers are rarely in structured fields.** They appear in issue comments, bodies, or external systems. Parse issue bodies for patterns like `Registered: [N]` or `Target: [N]`. Expect to find nothing and prompt the user for manual input.
- **Recurring vs. one-off hacks need different tracking.** A recurring program (e.g., monthly virtual hacks) has aggregate metrics across instances. A one-off hack (e.g., an annual challenge) has a single funnel. Clarify which you are looking at.
- **Low issue activity does not mean low hack activity.** Hackathon coordinators often work in external platforms (registration tools, challenge platforms, Slack channels) and update GitHub issues infrequently. Ask before assuming a hack is stalled.
- **Scope changes happen mid-program.** Registration targets, prize budgets, and platform choices can shift after planning is complete. Always check the most recent comments on the epic for scope adjustments, not just the original issue body.
- **Judging timelines slip the most.** Budget 50% more time than planned for judging. Judges are volunteers with competing priorities. Build in buffer and have backup judges identified.
- **Completion rate is the real metric, not registration count.** A hack with 500 registrations and 50 submissions (10% completion) is weaker than 200 registrations and 100 submissions (50% completion). Always report the full funnel, not just top-line registration.
- **Post-hack outcomes collection has a 7-day window.** After 7 days, participant engagement drops sharply. Survey responses, testimonials, and case study leads must be captured within the first week post-hack.
- **Partner-funded hacks have reporting obligations.** If a hack is co-funded (MDF, sponsorship), there are usually contractual reporting requirements. Flag these in the outcomes phase so the team does not miss partner report deadlines.
