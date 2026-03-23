# New Item Auto-Triage

**Cadence:** Event-driven — fires when a new item is created in the initiative tracker
**Platform:** Power Automate
**Output:** SharePoint list item update + optional Teams notification

---

## What This Does

Analyzes every new item the moment it is created in the SharePoint List initiative tracker and applies classification suggestions for owner, priority, and motion/category. It also flags hygiene problems immediately — missing owner, missing target date, missing priority — so items don't enter the tracker in a broken state.

This eliminates the lag between item creation and triage — instead of waiting for a weekly hygiene check, every new item gets immediate classification and any gaps are flagged right away. The automation does NOT apply changes automatically; it suggests them for owner review and posts a notification if a critical gap is detected.

## Sample Output

When someone creates a new item titled "Draft partner enablement guide for NVIDIA 1:many hackathon," the following happens within 60 seconds:

**If the item has gaps:**

Teams notification (to item creator):
> ⚠️ **New item needs attention** — "Draft partner enablement guide for NVIDIA 1:many hackathon"
> - ❌ No owner assigned
> - ❌ No target date set
> - 💡 Suggested Motion: Partner Enablement
> - 💡 Suggested Priority: P1
>
> [Open item to review →](link)

**If the item is complete:**

No notification — item enters the tracker cleanly.

---

## How to Build This

### Step 1: Power Automate — SharePoint Trigger

- **Trigger:** When an item is created (SharePoint)
- **Site:** [Your team's SharePoint site]
- **List:** [Your initiative tracker list name]

### Step 2: Evaluate New Item

Check the following fields:

| Field | Check | Flag if |
|-------|-------|---------|
| Owner | Is empty? | ❌ Missing owner |
| Target Date | Is null? | ❌ Missing target date |
| Priority | Is empty? | ❌ Missing priority |
| Motion/Category | Is empty? | Apply classification suggestion |
| Status | Is empty? | Default to "Not Started" |

### Step 3: Classification Logic (Suggest, Don't Apply)

Use keywords in the item title and notes to suggest values:

**Priority suggestion:**
```
IF title contains "P0" OR "urgent" OR "critical" OR "Build 2026" → Suggest P0
IF title contains "NVIDIA" OR "partner" OR "MDF" → Suggest P1
DEFAULT → Suggest P2
```

**Motion/Category suggestion:**
```
IF title contains "hackathon" OR "hack" → Suggest "Hackathon Portfolio"
IF title contains "content" OR "blog" OR "brief" OR "deck" → Suggest "Content"
IF title contains "event" OR "Build" OR "Ignite" → Suggest "Events"
IF title contains "partner" OR "NVIDIA" OR "GitHub" → Suggest "Partner Enablement"
IF title contains "skill" OR "learn" OR "certification" → Suggest "Skilling"
DEFAULT → Leave for manual classification
```

### Step 4: Update Item with Defaults (Safe Fields Only)

Apply non-destructive defaults automatically:
- If Status is empty → set to "Not Started"
- If Created date is empty → set to today (should auto-populate, but confirm)

Do NOT auto-assign owners or set priority — these are suggestions only.

### Step 5: Notify If Critical Gaps Detected

If Owner is empty OR Target Date is null:

**Action:** Post adaptive card in Teams to item creator

```html
⚠️ <b>New item needs attention</b><br>
"[Item title]"<br><br>
[If no owner:] ❌ No owner assigned<br>
[If no date:] ❌ No target date set<br>
[If priority suggestion:] 💡 Suggested Priority: [value]<br>
[If motion suggestion:] 💡 Suggested Motion: [value]<br><br>
<a href="[SharePoint item URL]">Open item to review →</a>
```

**If no gaps:** No notification. Item enters tracker cleanly.

### Step 6: Optional — Weekly Gap Sweep

Pair this event-driven triage with the Monday `weekly-board-health` check, which catches any items that slipped through without being triage'd (created via bulk import, migration, etc.).

---

## Data Source Configuration

| Setting | Value |
|---------|-------|
| SharePoint site URL | [Your team's SharePoint site] |
| Initiative list name | [Your tracker list name] |
| Owner field name | [Column name — e.g., "Owner"] |
| Priority field name | [Column name — e.g., "Priority"] |
| Target Date field name | [Column name — e.g., "Target Date"] |
| Motion field name | [Column name — e.g., "Motion" or "Category"] |
| Teams notification target | Item creator (dynamic from trigger output) |

---

## Gotchas

1. **SharePoint field names are case-sensitive in Power Automate.** Pull exact column names from your list settings before building.
2. **Bulk imports will trigger the flow for every item.** If migrating a batch of items, disable the flow temporarily or add a condition to skip items created before a certain date.
3. **Classification keywords need tuning.** The suggested motion/priority logic is a starting point — adjust keyword lists to match your team's actual program vocabulary.
4. **Don't auto-assign owners.** It's tempting to automatically assign items based on keywords (e.g., "NVIDIA" → @Erica), but auto-assignment causes confusion when the item doesn't belong to that person. Always keep owner assignment as a human action.
5. **This is complementary to weekly triage, not a replacement.** The event-driven flow catches new items. The weekly `tracker-triage` check catches items that have degraded over time.
