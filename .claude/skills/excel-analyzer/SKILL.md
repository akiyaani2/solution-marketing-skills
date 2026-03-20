---
name: excel-analyzer
description: Analyzes Excel spreadsheets and CSV data — summarizes key metrics, identifies trends, flags anomalies, and generates insights. Use when reviewing budget spreadsheets, registration data, program metrics, vendor reports, or any tabular data.
allowed-tools: Read, Write, Edit, Bash
---

# Excel & Data Analyzer

Takes Excel (.xlsx), CSV, or tabular data and produces analysis: summaries, trends, anomalies, and actionable insights. Designed for marketing team data — budgets, registrations, engagement metrics, vendor reports.

## Trigger Phrases

- `/excel-analyzer`
- `/excel-analyzer "budget-tracker.xlsx"`
- `analyze this spreadsheet`
- `what does this data tell me`
- `summarize this Excel file`
- `find trends in this data`

## What This Skill Does

### 1. Data Summary

For any spreadsheet or CSV:
- Row/column count
- Column names and data types
- Key statistics (sum, average, min, max, count) for numeric columns
- Missing data flagged
- Unique values for categorical columns

### 2. Trend Analysis

For time-series data (registrations, spend, metrics over time):
- Week-over-week or month-over-month changes
- Trend direction (increasing, decreasing, flat)
- Anomalies (sudden spikes or drops)
- Forecasting notes (if pattern is clear)

### 3. Budget Analysis (for budget/spend data)

```markdown
## Budget Analysis — [File Name]

### Summary
- Total budget: $[amount]
- Total spent: $[amount]
- Remaining: $[amount] ([%])
- Burn rate: $[amount/month]
- Projected end-of-period: [Over/Under by $X]

### By Category
| Category | Budget | Spent | Remaining | % Used |
|----------|--------|-------|-----------|--------|

### Flags
- [Over-budget items]
- [Under-spent items that may indicate stalled programs]
- [Large upcoming commitments]
```

### 4. Registration/Engagement Analysis (for program data)

```markdown
## Registration Analysis — [Program Name]

### Funnel
| Stage | Count | Conversion Rate |
|-------|-------|----------------|
| Registered | [N] | 100% |
| Started | [N] | [%] |
| Completed | [N] | [%] |
| Certified | [N] | [%] |

### Demographics
- Top geographies: [list]
- Top job titles: [list]
- Top industries: [list]

### Trends
- Registration velocity: [N/day or /week]
- Peak registration day: [date]
- Drop-off point: [stage where most people leave]

### Insights
1. [Actionable insight with data]
2. [Actionable insight with data]
```

### 5. Quick Comparison

For comparing two datasets (this month vs last month, our program vs benchmark):

```markdown
## Comparison: [Dataset A] vs [Dataset B]

| Metric | [A] | [B] | Delta | Assessment |
|--------|-----|-----|-------|-----------|
| [metric] | [value] | [value] | [+/-] | [Better/Worse/Same] |
```

## How To Provide Data

This skill can read:
- **Excel files** (.xlsx) — provide the file path
- **CSV files** (.csv) — provide the file path
- **Pasted tables** — paste directly into the conversation
- **Data descriptions** — "I have registration data with columns: name, email, date, status, country"

For Excel files, the skill will use Python/pandas if available, or parse the structure from the raw data.

## Gotchas

- Always look at the data before analyzing. "Garbage in, garbage out" — check for obvious errors first.
- Excel files often have hidden sheets, merged cells, or headers on row 3 instead of row 1. Call these out.
- Registrations ≠ attendees ≠ completions. Be precise about which metric you're analyzing.
- Budget data may be in different currencies or fiscal periods. Confirm the units.
- Trends from small datasets (< 30 data points) are unreliable. Flag when sample size is too small.
- Always provide insights, not just numbers. "Registrations are 2,400" is data. "Registrations hit 2,400 — 45% above target, driven by the partner co-marketing email" is an insight.
