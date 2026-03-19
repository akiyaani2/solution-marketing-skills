---
name: budget-ops
description: Budget and vendor operations management — PO tracking, co-marketing fund utilization, spend-vs-plan analysis, vendor lifecycle views, and leadership budget reports for solution marketing teams
allowed-tools: [Bash, Read, Write, Edit]
trigger_phrases:
  - "/budget-ops"
  - "/budget-status"
  - "/budget-ops --po-status"
  - "/budget-ops --vendor"
  - "/budget-ops --report"
  - "budget status"
  - "PO status"
  - "where are we on budget"
---

# Budget Operations

Track and manage all budget, purchase order, vendor financial operations, and co-marketing fund utilization. Provides dashboards for team leads and formatted reports for leadership.

## Trigger Phrases

- `/budget-ops` — Full budget dashboard
- `/budget-status` — Same as above (alias)
- `/budget-ops --po-status` — PO lifecycle tracker
- `/budget-ops --co-marketing` — Co-marketing / MDF fund utilization
- `/budget-ops --vendor [name]` — Per-vendor financial summary
- `/budget-ops --report` — Formatted budget report for leadership
- `/budget-ops --forecast` — Spend forecast for remaining quarter/year

## What This Skill Does

### 1. Budget Dashboard (`/budget-ops`)

Pull all budget-related issues and generate a financial status view.

```bash
# Budget and operations issues
gh issue list --state open --label "func:ops" --json number,title,labels,body --limit 100

# Vendor-related issues
gh issue list --state open --search "PO OR budget OR vendor OR invoice" --json number,title,labels,body --limit 100
```

**Output format:**

```
## Budget Status — [Date]

### Active Purchase Orders
| PO # | Vendor | Amount | Status | Owner | Days in Status |
|------|--------|--------|--------|-------|---------------|

### Co-Marketing Fund Utilization
| Partner | Allocated | Committed | Spent | Remaining | Deadline |
|---------|-----------|-----------|-------|-----------|----------|

### Budget Summary
| Category | Budget | Committed | Spent | Available | % Used |
|----------|--------|-----------|-------|-----------|--------|
| Events | $[N] | $[N] | $[N] | $[N] | [X]% |
| Content | $[N] | $[N] | $[N] | $[N] | [X]% |
| Vendors | $[N] | $[N] | $[N] | $[N] | [X]% |
| Paid Media | $[N] | $[N] | $[N] | $[N] | [X]% |
| **Total** | **$[N]** | **$[N]** | **$[N]** | **$[N]** | **[X]%** |

### Budget Risks
- [items over budget or approaching limit]
- [POs pending approval beyond expected timeline]
- [Co-marketing fund deadlines approaching]

### Pending Approvals
| Item | Amount | Approver | Submitted | Days Waiting |
|------|--------|----------|-----------|-------------|
```

### 2. PO Lifecycle Tracker (`/budget-ops --po-status`)

Track purchase orders through their lifecycle:

**PO stages:**

```
Draft → Submitted → Approved → Active → Invoiced → Closed
```

```bash
# Find PO-related issues
gh issue list --state all --search "PO OR purchase order" --label "func:ops" --json number,title,labels,body,state --limit 100
```

**Output per PO:**

```
### PO-[Number]: [Vendor] — [Description]
| Field | Value |
|-------|-------|
| Amount | $[N] |
| Status | [stage] |
| Submitted | [date] |
| Approved By | [name] |
| Approval Date | [date] |
| Expiration | [date] |
| Days in Current Stage | [N] |
| % Invoiced | [X]% |

Notes: [any flags or context]
```

**Approval chain reference (customize to your org):**

| PO Amount | Approval Path | Expected Turnaround |
|-----------|--------------|-------------------|
| < $10K | Auto-approved by team lead | 1-2 days |
| $10K - $25K | Team lead approval | 3-5 days |
| $25K - $100K | Team lead + Director | 5-10 days |
| > $100K | Director + VP | 10-20 days |

### 3. Co-Marketing Fund Tracker (`/budget-ops --co-marketing`)

Track co-marketing funds (MDF, joint marketing budgets, partner contributions):

```
## Co-Marketing Fund Status — [Date]

### [Partner Name] Fund
| Field | Value |
|-------|-------|
| Total Allocation | $[N] |
| Fiscal Year | [FY] |
| Expiration | [date] |

#### Commitments
| Program | Amount | Status | Claim Deadline |
|---------|--------|--------|---------------|
| [program 1] | $[N] | Committed | [date] |
| [program 2] | $[N] | Spent | [date] |
| [program 3] | $[N] | Planned | [date] |

#### Fund Health
- Allocated: $[N]
- Committed: $[N] ([X]%)
- Spent: $[N] ([X]%)
- Remaining: $[N]
- Days Until Expiration: [N]
- Burn Rate: $[N]/month
- Projected Balance at Expiration: $[N]
```

**Co-marketing fund approval chain (customize to your org):**

| Decision | Approver |
|----------|----------|
| Fund allocation plan | Team lead + Partner lead |
| Individual program commitment | Team lead |
| Claim submission | Finance / Ops coordinator |
| Fund reallocation | Director approval |

### 4. Vendor Status View (`/budget-ops --vendor [name]`)

Per-vendor financial summary:

```
## Vendor Summary — [Vendor Name]

### Financial Overview
| Field | Value |
|-------|-------|
| Active POs | [N] totaling $[N] |
| YTD Spend | $[N] |
| SOW Status | [active / expiring / expired] |
| SOW Expiration | [date] |
| Key Contact | [name, email] |

### Active Purchase Orders
| PO # | Description | Amount | Invoiced | Remaining |
|------|------------|--------|----------|-----------|

### Upcoming Renewals / Expirations
| Item | Date | Action Needed |
|------|------|--------------|

### Performance Notes
- [delivery quality, timeliness, issues]
```

### 5. Budget Report for Leadership (`/budget-ops --report`)

Formatted budget summary suitable for director/VP review:

```
## Budget Report — [Period]
Prepared for: [leadership name/title]
As of: [date]

### Executive Summary
[2-3 sentence summary of budget health, key risks, and actions needed]

### YTD Spend by Category
| Category | Budget | Actual | Variance | Notes |
|----------|--------|--------|----------|-------|
| Events | $[N] | $[N] | [+/-$N] | [context] |
| Content | $[N] | $[N] | [+/-$N] | [context] |
| Vendors | $[N] | $[N] | [+/-$N] | [context] |
| Paid Media | $[N] | $[N] | [+/-$N] | [context] |
| Co-Marketing | $[N] | $[N] | [+/-$N] | [context] |
| **Total** | **$[N]** | **$[N]** | **[+/-$N]** | |

### Major Commitments Next Quarter
| Item | Amount | Status | Risk Level |
|------|--------|--------|-----------|

### Items Requiring Attention
1. [item needing approval or decision]
2. [budget risk or overspend area]
3. [vendor issue or renewal]

### Recommendations
1. [recommendation with financial impact]
2. [recommendation with financial impact]
```

### 6. Spend Forecast (`/budget-ops --forecast`)

Project spend through end of quarter/fiscal year:

```
## Spend Forecast — Through [End of Period]

### Projection Method
Based on: [committed POs + planned activities + historical run rate]

### Quarterly Forecast
| Quarter | Budget | Projected Spend | Projected Variance |
|---------|--------|----------------|-------------------|
| [Q current] | $[N] | $[N] | [+/-$N] |
| [Q next] | $[N] | $[N] | [+/-$N] |

### Risk Scenarios
| Scenario | Impact | Probability | Mitigation |
|----------|--------|------------|-----------|
| [risk 1] | +$[N] | [H/M/L] | [action] |
| [risk 2] | +$[N] | [H/M/L] | [action] |

### Recommended Actions
- [action to stay on budget]
- [action to optimize spend]
```

## Gotchas

- **Budget data is almost never in a structured database within GitHub.** It lives scattered across issue bodies, comments, spreadsheets, and procurement systems. This skill aggregates from multiple text sources — expect gaps and prompt the user to fill them.
- **PO numbers from your procurement system may not appear in GitHub Issues.** If the team tracks POs externally, GitHub issues serve as a coordination layer, not the system of record. Reference the external system in issue bodies.
- **Co-marketing funds have their own approval chain separate from your regular budget.** Do not conflate MDF/co-marketing approvals with internal PO approvals. They often go through different stakeholders and have different timelines.
- **Co-marketing funds typically expire at fiscal year end.** Unspent funds are forfeited. Track fund expiration dates aggressively — "use it or lose it" is real. Start surfacing remaining balances 90 days before expiration.
- **Budget scope changes happen.** If program scope is reduced (e.g., target cut from 40K to 10K participants), the associated budget may need right-sizing. Always flag when a budget was sized for a different scope than current plans.
- **Ops coordinators may not have access to your project management tools.** If your budget coordinator works from spreadsheets or exports, ensure budget reports can be exported to formats they can consume (Excel, CSV, PDF).
- **Approval timelines vary wildly by amount.** A $5K PO might take 2 days; a $100K PO might take 3 weeks. Factor approval lead time into event and program workback schedules, not just the budget timeline.
- **Variance is only meaningful with context.** Reporting "$15K over budget" is not useful without explaining why and whether it is recoverable. Always pair variance numbers with root cause and recommended action in leadership reports.
- **Fiscal quarters may not align with calendar quarters.** Many organizations run July-June fiscal years. Always clarify which fiscal period you are reporting against and use the org's fiscal calendar, not calendar year.
