---
name: data-to-slides
description: Transforms raw data (metrics, tables, Excel exports) into presentation-ready slide content with chart suggestions and narrative framing. Use when turning numbers into a story for leadership presentations, MBR slides, or program reviews.
allowed-tools: Read, Write, Edit, Bash
---

# Data to Slides

Takes raw metrics, tables, or data and transforms them into presentation-ready content with narrative framing, chart recommendations, and key takeaways. The bridge between your spreadsheet and your PowerPoint.

## Trigger Phrases

- `/data-to-slides`
- `/data-to-slides "registration-data.csv"`
- `turn this data into slides`
- `make this presentable`
- `I need to present these numbers`

## What This Skill Does

### Step 1: Understand the Data

Read the provided data and identify:
- What metrics are present
- What time periods are covered
- What comparisons are possible (MoM, YoY, vs target)
- What the story is (improving? declining? mixed?)

### Step 2: Identify the Narrative

Every dataset tells a story. Pick the right one:

| Data Pattern | Narrative Frame | Slide Headline Style |
|-------------|----------------|---------------------|
| Metric exceeds target | Win / celebration | "Registration Hit 150% of Target" |
| Metric growing steadily | Momentum / trajectory | "Third Consecutive Month of Growth" |
| Metric below target | Gap + plan | "Registration Gap: Root Cause + Recovery Plan" |
| Mixed results | Balanced / selective | "Strong Q3 Pipeline Despite Event Delays" |
| New program, first data | Baseline + potential | "First Results: 400 Signups in Week One" |

### Step 3: Generate Slide Content

For each key data point, produce:

```markdown
## Slide: [Headline — not a label, a takeaway]

**Chart type:** [Bar / Line / Pie / Table / Comparison]
**Data to show:**
| [columns] |
| [data rows] |

**Key callout:** [The one number or trend that matters most]

**Speaker note:** [The narrative behind the data — what does it mean and why should leadership care?]
```

### Chart Selection Guide

| Data Type | Best Chart | When to Use |
|-----------|-----------|-------------|
| Over time (monthly, weekly) | Line chart | Show trends and velocity |
| Categories compared | Horizontal bar chart | Registration by country, budget by category |
| Part of whole | Pie/donut (use sparingly) | Only when showing true proportions |
| Target vs actual | Bullet chart or bar + reference line | Budget tracking, goal progress |
| Before/after | Side-by-side bars | MoM comparison, A/B results |
| Funnel stages | Funnel diagram or stacked bar | Registration → completion pipeline |
| Single big number | KPI card (large text + context) | Headline metrics |

### Data Storytelling Rules

1. **One metric per slide.** Don't cram 5 charts on one slide.
2. **Headline = takeaway, not label.** "Revenue Up 23% QoQ" not "Q3 Revenue."
3. **Always include context.** A number alone is meaningless. Compare to: target, last period, competitor, or benchmark.
4. **Round aggressively.** Leadership doesn't need "2,387 registrations." They need "~2,400 registrations (45% above target)."
5. **Color codes meaning.** Green = good/above target. Red = attention needed. Don't use random colors.
6. **Source your data.** Small text at the bottom: "Source: [Platform name], as of [date]."

## Gotchas

- Raw data tables are not presentations. If you're showing more than 5 rows and 4 columns, it's an appendix slide.
- Charts without a narrative are just decoration. Every chart needs a "so what?" in the speaker notes.
- Avoid "chart junk" — 3D effects, unnecessary gridlines, decorative elements. Clean and simple.
- If you have to explain the chart, the chart is wrong. Redesign it.
- Always show your data with the right comparison. "2,400 registrations" is meaningless. "2,400 registrations vs 1,650 target (+45%)" tells a story.
- For MBR data: lead with the metric that makes the strongest case for your programs. Don't lead with a metric you missed.
