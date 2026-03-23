# Monthly MBR Data Package

**Cadence:** 8th of every month at 7:00 AM local time
**Platform:** Copilot Studio + Power Automate
**Output:** SharePoint document + Teams notification

---

## What This Does

Assembles the scaffolding for the Monthly Business Review package on the 8th of each month, giving the team lead a full week before the typical MBR meeting window (around the 19th). Instead of spending a day pulling data and formatting tables, the automation delivers a pre-built document with all available data filled in, explicit flags for what's missing, and narrative placeholders ready for human judgment.

The team lead reviews, fills in flagged gaps (especially metrics from Power BI, event platforms, and CRM), adds strategic commentary, and the package is ready.

**Key principle from Copilot research:** M365 data sources (SharePoint Lists, Planner) are auto-populated. Metrics from Power BI, event platforms, and external tools require manual input — the automation flags these gaps explicitly rather than leaving blank cells or fabricating numbers.

## Sample Output

A markdown file saved to SharePoint at `/MBR Reports/MBR-[YYYY-MM].md` with a Teams notification linking to it.

---

**Monthly Business Review — March 2026**
*AI Apps & Agents Skilling Team*

**[AUTO-POPULATED from SharePoint List]**

**Completed This Month**

| Owner | Completed | Key Highlights |
|-------|-----------|----------------|
| Mindy | 4 items | Build 2026 lab outline locked; partner brief v1 delivered |
| Amy | 3 items | Attribution model spec complete; dashboard Phase 1 live |
| Erica | 2 items | NVIDIA content brief delivered; hack registration open |

**In Progress**

| Initiative | Owner | Status | Target Date |
|------------|-------|--------|-------------|
| NVIDIA content co-design | Erica | 🟡 At Risk — 6-week delay | May 15 |
| Applied Skills attribution | Amy | 🟢 On Track | April 1 |

**Blocked / At-Risk**

| Initiative | Owner | Blocker | Days Stuck |
|------------|-------|---------|------------|
| Innovation Studio reg flow | Aaron | UX review pending | 12 |

---

**[REQUIRES MANUAL INPUT — Data not available from M365 sources]**

⚠️ **Key Metrics section needs values from external sources:**

| Metric | Source | Current Month | Last Month | Delta |
|--------|--------|--------------|------------|-------|
| Event registrations | Event platform | **[PASTE]** | **[PASTE]** | — |
| Certifications earned | Learn/Credly | **[PASTE]** | **[PASTE]** | — |
| Partner pipeline influence | CRM/Dynamics | **[PASTE]** | **[PASTE]** | — |
| Content pieces shipped | Manual count | **[PASTE]** | **[PASTE]** | — |

---

**[NARRATIVE PLACEHOLDER]**

Executive Summary:
> *[2–3 sentences: what the team delivered, key win, top risk. Frame in terms of outcomes and strategic impact.]*

Next Month Preview:
> *[Key milestones and focus areas for next month.]*

Asks / Escalations:
> *[What you need from leadership — decisions, resources, air cover.]*

---

## How to Build This

### Step 1: Power Automate — Recurrence Trigger

- **Trigger:** Recurrence
- **Interval:** 1 month
- **Frequency:** Month
- **Day of month:** 8
- **Start time:** 7:00 AM (team timezone)

### Step 2: Pull M365 Data

**Completed this month (SharePoint List):**
```
Filter: Status = "Complete" AND Modified >= [first day of prior month]
         AND Modified <= [last day of prior month]
Output: Title, Owner, Modified, Notes
Group by: Owner
```

**In-progress items (SharePoint List):**
```
Filter: Status = "In Progress" OR Status = "At Risk"
Output: Title, Owner, Status, Target_Date, Obstacles
```

**Blocked items (SharePoint List):**
```
Filter: Status = "Blocked" OR Obstacles != null AND Status != "Complete"
Output: Title, Owner, Obstacles, Target_Date, (today - Target_Date) as days_stuck
```

**Completed tasks (Planner):**
```
Filter: completedDateTime >= [first day of prior month]
Output: Title, Assignee, completedDateTime
Group by: Assignee
```

### Step 3: Build the Document

**Option A — Word document in SharePoint:**
- Action: Create file (SharePoint connector)
- Path: `/MBR Reports/MBR-[YYYY-MM].docx`
- Content: Formatted HTML with auto-populated tables + flagged gaps

**Option B — Markdown file in SharePoint:**
- Action: Create file
- Path: `/MBR Reports/MBR-[YYYY-MM].md`
- Content: Markdown with auto-populated tables + gap markers

> **SharePoint List grounding caveat:** If using Copilot Studio to generate the MBR document via the `mbr` skill, note that SharePoint List as a knowledge source is a staged rollout feature. The recommended pattern is: use Power Automate to pull structured data from Lists and write it into a SharePoint document — then the MBR skill grounds on that document plus any additional narrative context the user provides.

### Step 4: Notify Team Lead

- **Action:** Post message in Teams channel or send email
- **Message:**

```html
<b>📊 MBR Package Ready — [Month Year]</b><br><br>
The monthly MBR scaffold has been generated and saved to SharePoint.<br><br>
✅ <b>Auto-populated:</b> Completions, in-progress items, blockers (from initiative tracker)<br>
⚠️ <b>Needs your input:</b> Key metrics (event registrations, certifications, pipeline) — open the doc and fill in the flagged sections.<br><br>
📄 <a href="[SharePoint link]">Open MBR Draft</a><br><br>
<i>Target review date: [Month 15th] | MBR meeting: ~[Month 19th]</i>
```

---

## Copilot Studio Variant

1. Create a monthly trigger (8th of month, 7:00 AM)
2. Invoke the `mbr` skill
3. Skill pulls from SharePoint List + Planner, flags metric gaps, generates scaffold
4. Save to SharePoint via "Create file" agent flow
5. Notify team lead via Teams post + email

---

## Gap-Flagging Protocol

The automation explicitly marks sections where external data is needed:

| Section | Source | Flag Pattern |
|---------|--------|-------------|
| Event registrations | Event platform (Cvent, Innovation Studio, DevPost) | `**[PASTE from event platform]**` |
| Training completions/certifications | Microsoft Learn, Credly | `**[PASTE from Learn/Credly]**` |
| Partner pipeline influence | Dynamics 365, Salesforce | `**[PASTE from CRM]**` |
| Hackathon participant counts | DevPost, Innovation Studio | `**[PASTE from platform]**` |
| Power BI dashboard metrics | Power BI | `**[PASTE from Power BI]**` |

Never leave these blank or fill in placeholder zeros. The gap markers tell the team lead exactly what to pull before the MBR meeting.

---

## Data Source Configuration

| Setting | Value |
|---------|-------|
| SharePoint site URL | [Your team's SharePoint site] |
| Initiative list name | [Your tracker list name] |
| MBR output folder | `/MBR Reports/` |
| Planner plan ID(s) | [Active Planner plans] |
| Teams notification channel | [Team channel or DM to team lead] |
| Month close buffer | 8th of following month (7 days after month-end) |

---

## Gotchas

1. **Metric values require external data.** The biggest limitation of any automated MBR is that program metrics (registrations, certifications, pipeline) don't live in M365. Build the expectation that the team lead will spend 30–60 minutes filling in these gaps even with automation.
2. **"Completed this month" requires clean date tracking.** Items marked complete mid-month in bulk (common at end of sprint) may produce a misleading completions view. Encourage owners to mark items complete when they're actually done.
3. **SharePoint List grounding in Copilot Studio may not be available in all tenants.** Use the Power Automate path (write list data to a SharePoint document, then ground on the document) for reliable behavior.
4. **Don't generate on the 1st.** Wait until the 8th — data from the final days of the prior month may still be settling (especially for anything tracked in external systems).
5. **Narrative sections require human judgment.** The executive summary, competitive context, and strategic framing cannot be automated. The automation creates time for the team lead to focus on those sections by handling the data assembly.
