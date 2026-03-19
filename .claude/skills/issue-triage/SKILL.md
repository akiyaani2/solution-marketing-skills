---
name: issue-triage
description: Bulk issue triage — stale P0 detection, P0 downgrade cascade pattern, missing label detection, orphan detection. The highest-value operational hygiene skill for GitHub-based team management.
allowed-tools: [Bash, Read]
trigger_phrases:
  - "/triage"
  - "/issue-triage"
  - "/triage --p0-only"
  - "/triage --stale"
  - "/triage --orphans"
  - "/triage --labels"
  - "triage the board"
  - "what needs attention"
---

# Issue Triage

The most impactful manual process in any GitHub-based team hub, now systematized. Scans all open issues, identifies hygiene problems, and executes the P0 downgrade cascade pattern.

## Usage

```
/triage                # Full board triage — all checks
/triage --p0-only      # P0 cascade analysis only
/triage --stale        # Stale issue sweep (14+ days no activity)
/triage --orphans      # Issues not wired to any parent epic
/triage --labels       # Label hygiene check
```

## What This Skill Does

### 1. Full Board Triage (`/triage`)

Scan ALL open issues and categorize problems into a prioritized report.

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --limit 300 \
  --json number,title,labels,updatedAt,assignees,body
```

**Report sections (in priority order):**

1. **Stale P0s** — P0 issues not updated in 7+ days (CRITICAL — these should never exist)
2. **Stale P1s** — P1 issues not updated in 14+ days
3. **P0 Cascade Candidates** — P0 issues that should be downgraded per the cascade pattern
4. **Missing Labels** — Issues without `owner:` or `func:` labels
5. **Orphaned Issues** — Issues not wired to a parent epic
6. **Duplicate Owner Labels** — Issues with conflicting ownership (unless intentionally co-owned)
7. **Priority Mismatches** — Sequential dependencies marked P0 when they should be P1

---

### 2. The P0 Downgrade Cascade (`/triage --p0-only`)

The core decision framework for priority hygiene. For every program or epic that has P0-labeled items, apply this cascade:

#### The Cascade Decision Framework

```
For each program/epic with P0 items:

  Step 1: IDENTIFY THE GATING TASK
    The one task everything else waits for.
    → KEEP at P0

  Step 2: IDENTIFY SEQUENTIAL DEPENDENCIES
    Tasks that cannot start until the gating task completes.
    → DOWNGRADE to P1

  Step 3: IDENTIFY POST-COMPLETION TASKS
    Tasks that happen after the main work (judging, wrap-up,
    metrics collection, retrospectives, reporting).
    → DOWNGRADE to P1 or P2

  Step 4: IDENTIFY DISTANT FUTURE TASKS
    Tasks 4+ months out with no current dependencies.
    → DOWNGRADE to P2
```

#### Cascade Decision Rules

| Condition | Action | Rationale |
|-----------|--------|-----------|
| Work is actively blocking other work | **Keep P0** | Gating item — removal causes cascade delay |
| Has a due date within 6 weeks | **Keep P0** | Imminent deadline requires top priority |
| Is a gating item for a program launch | **Keep P0** | Critical path item |
| Sequential dependency — cannot start until predecessor completes | **Downgrade to P1** | Not actionable now, will become P0 when unblocked |
| Post-event task (judging, wrap-up, metrics) | **Downgrade to P1** | Important but not urgent until event completes |
| 4+ months out with no current dependencies | **Downgrade to P2** | Long-horizon item, revisit next quarter |

#### Cascade Example

For a hackathon program with 5 sub-tasks:
- **Partner Coordination** → P0 (gating — partners must be locked before anything else)
- **Registration & Promo** → P1 (sequential — cannot register without partner confirmation)
- **Content & Challenges** → P1 (sequential — cannot write challenges without partner input)
- **Judging & Awards** → P1 (post-completion — happens after the event)
- **Metrics & Retrospective** → P2 (post-completion — happens well after event closes)

#### Executing a Downgrade

For each issue being downgraded:

```bash
# Remove current priority, add new one
gh issue edit [NUMBER] --repo OWNER/REPO \
  --remove-label "p0" \
  --add-label "p1"

# Post comment explaining the change
gh issue comment [NUMBER] --repo OWNER/REPO --body "$(cat <<'EOF'
Priority adjusted: P0 → P1

**Reason**: This is a sequential dependency on #[gating issue]. It cannot start until [predecessor] completes. Keeping the gating item (#[N]) at P0.

---
*[Date] | Priority Change*
EOF
)"
```

**Always post a comment explaining WHY before changing priority labels.** Never silently downgrade.

---

### 3. Stale Issue Sweep (`/triage --stale`)

Find issues with no activity in 14+ days.

```bash
# Calculate the stale threshold date (14 days ago)
# macOS:
STALE_DATE=$(date -v-14d +%Y-%m-%d)
# Linux:
STALE_DATE=$(date -d '14 days ago' +%Y-%m-%d)

gh issue list --repo OWNER/REPO \
  --state open \
  --limit 300 \
  --json number,title,labels,updatedAt \
  | jq "[.[] | select(.updatedAt[:10] <= \"$STALE_DATE\")]"
```

**For each stale issue:**

1. Check if related issues have upstream activity (work might be happening on linked items)
2. Check the owner — some team members may be active on work but not updating GitHub
3. Categorize: genuinely stuck vs. just not updated vs. waiting on external
4. Post a substantive context-aware comment (not a generic "please update" ping)
5. Flag for the team lead if genuinely stuck

**Stale Thresholds:**

| Priority | Stale After | Critical After |
|----------|-------------|----------------|
| P0 | 7 days | 3 days |
| P1 | 14 days | 7 days |
| P2 | 30 days | 14 days |

---

### 4. Orphan Detection (`/triage --orphans`)

Find issues not wired to any parent epic.

```bash
gh issue list --repo OWNER/REPO \
  --state open \
  --limit 300 \
  --json number,title,body,labels
```

For each issue, check:
- Does the body contain a `Parent:` or `Epic:` reference?
- Does any `type:initiative` epic reference this issue in its task list (`- [ ] #NN`)?
- Is it labeled `type:initiative` itself (epics are top-level, not orphans)?

**Output:** List orphans grouped by owner label, suggest parent epics based on `func:` and `sp:` label overlap.

---

### 5. Label Hygiene (`/triage --labels`)

Find and fix label issues:

| Problem | Detection | Suggestion Logic |
|---------|-----------|-----------------|
| Missing `owner:` label | No `owner:*` label present | Infer from assignee or `func:` label using ownership matrix |
| Missing `func:` label | No `func:*` label present | Infer from title keywords (e.g., "hack" → `func:hackathon`) |
| Missing priority | No `p0`/`p1`/`p2` label | Suggest based on due date proximity and parent epic priority |
| Conflicting labels | Multiple conflicting labels | Flag for manual resolution (some multi-owner is intentional) |

```bash
# Find issues missing owner labels
gh issue list --repo OWNER/REPO \
  --state open \
  --limit 300 \
  --json number,title,labels \
  | jq '[.[] | select([.labels[].name | select(startswith("owner:"))] | length == 0)]'
```

## Full Triage Report Format

```markdown
# Board Triage Report — [Date]

## Summary
| Category | Count | Action Needed |
|----------|-------|---------------|
| Stale P0s | X | CRITICAL — update or downgrade |
| Cascade candidates | X | Review for downgrade |
| Stale P1s | X | Update or re-prioritize |
| Missing labels | X | Apply labels |
| Orphaned issues | X | Wire to parent epic |
| Label conflicts | X | Resolve ownership |

---

## 🚨 Stale P0s (Resolve Immediately)

| # | Title | Owner | Days Stale | Recommendation |
|---|-------|-------|------------|----------------|
| #NN | [Title] | [owner] | 12 | Downgrade to P1 — sequential dep |
| #NN | [Title] | [owner] | 8 | Keep P0 — update status |

---

## 🔽 P0 Cascade Candidates

### [Program/Epic Name] (#NN)
| # | Title | Current | Recommended | Reason |
|---|-------|---------|-------------|--------|
| #NN | Partner coordination | P0 | **Keep P0** | Gating task |
| #NN | Registration setup | P0 | **→ P1** | Sequential dep on #NN |
| #NN | Post-event judging | P0 | **→ P1** | Post-completion task |

---

## 📋 Missing Labels
[Issues grouped by problem type]

## 🔗 Orphaned Issues
[Issues grouped by owner with suggested parents]
```

## Gotchas

- **"No update" does not mean "no progress."** Some team members work actively but do not update GitHub. Check related issues and context before flagging. A stale issue with active sub-issues is not truly stale.
- **Always comment before changing labels.** Silent priority changes erode team trust. Every downgrade gets a comment explaining the reasoning.
- **Some issues intentionally have multiple owner labels.** Co-owned items (e.g., `owner:alice` + `owner:bob`) are valid. Only flag as a conflict if the owners are from different functions with no overlap.
- **Support/capacity tickets are not task tickets.** If your repo tracks bandwidth or capacity requests alongside tasks, do not triage them using the same rules. They follow different lifecycle patterns.
- **Pagination is required for large boards.** Repos with 200+ open issues will truncate at the default limit. Always use `--limit 300` or paginate explicitly.
- **The cascade pattern only applies within a single program or epic.** Do not cascade across unrelated epics. Each program has its own gating chain.
- **P0 count is a key leadership metric.** Track the before/after count when reporting triage results. "Reduced P0s from 30 to 12" is a meaningful operational win.
- **Date arithmetic differs between macOS and Linux.** macOS uses `date -v-14d`, Linux uses `date -d '14 days ago'`. Check your environment.

## Related Skills

- `/epic-health` — Health scoring for individual epics (feeds triage decisions)
- `/standup` — Daily view surfaces stale items early
- `/sp-health` — Strategic view shows which plays need triage attention
- `/escalations` — Items flagged during triage that need leadership attention
