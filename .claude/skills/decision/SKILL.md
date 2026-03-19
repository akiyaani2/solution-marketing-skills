---
name: decision
description: Creates a properly formatted decision record (TD-NNNN or DR-NNNN) with structured template — context, options considered, decision, RAPID roles, consequences. Auto-finds next available number.
allowed-tools: [Bash, Read, Write, Glob]
disable-model-invocation: true
trigger_phrases:
  - "/decision"
  - '/decision "description"'
  - "record a decision"
  - "create a decision record"
  - "document this decision"
---

# Decision Record

Creates a formatted, numbered decision record. Finds the next available sequence number, generates a slugified filename, and writes a structured markdown file with RAPID governance roles.

## Usage

```
/decision "Chose Vendor X over Vendor Y for lab hosting"
/decision "Approved $50K MDF allocation for partner hackathons"
/decision "Shifted event format from in-person to hybrid"
```

When invoked with a description, the skill creates the file immediately and prompts for any missing details. When invoked without a description, it prompts interactively.

## When to Use This Skill

| Use It For | Skip It For |
|-----------|-------------|
| Choosing between vendors or platforms | Routine task assignments |
| Budget decisions above your auto-approve threshold | Day-to-day prioritization |
| Strategy changes (format, audience, timing) | Obvious or reversible choices |
| Anything future-you will want to reference | Decisions already documented elsewhere |
| Cross-team commitments or agreements | Internal workflow tweaks |

## Implementation

### Step 1: Find the next available number

Scan the decisions directory for existing records and find the highest number:

```bash
# List existing decision records — adjust path and prefix to your repo
ls operations/decisions/ 2>/dev/null \
  | grep -oE '(TD|DR)-[0-9]+' \
  | grep -oE '[0-9]+' \
  | sort -n \
  | tail -1
```

If no files exist, start at `0001`. Otherwise, increment the highest number by 1.

**Prefix convention:**
- `TD-NNNN` — "Team Decision" (default)
- `DR-NNNN` — "Decision Record" (alternative, choose one and be consistent)

### Step 2: Generate the slug

Convert the description to a filename-safe slug:
- Lowercase
- Replace spaces and special characters with hyphens
- Trim to ~50 characters
- Remove trailing hyphens

```
"Chose Vendor X over Vendor Y" → chose-vendor-x-over-vendor-y
"Approved $50K MDF allocation"  → approved-50k-mdf-allocation
```

### Step 3: Create the decision file

Write to `operations/decisions/TD-NNNN-slug.md` (or your configured decisions directory):

```markdown
# TD-NNNN: [Decision Title]

> **Date:** [Today's date, YYYY-MM-DD]
> **Status:** Accepted
> **Decision Maker:** [From RAPID Decide role]

---

## Context

[What prompted this decision? What problem are we solving?
Include relevant background — budget constraints, timeline pressure,
stakeholder requirements, competitive context.]

## Options Considered

### Option A: [Name]
- **Pros:** [Benefits]
- **Cons:** [Drawbacks]
- **Cost:** [If applicable]

### Option B: [Name]
- **Pros:** [Benefits]
- **Cons:** [Drawbacks]
- **Cost:** [If applicable]

### Option C: [Name] *(if applicable)*
- **Pros:** [Benefits]
- **Cons:** [Drawbacks]
- **Cost:** [If applicable]

## Decision

[What we decided and why. Be specific — "We chose Option A because..."
Include the key factor(s) that tipped the decision.]

## RAPID

| Role | Person | Notes |
|------|--------|-------|
| **R**ecommend | [Who proposed/analyzed] | Prepared the options |
| **A**gree | [Who has veto power, or "—"] | Must approve before execution |
| **P**erform | [Who executes] | Responsible for implementation |
| **I**nput | [Who was consulted] | Provided expertise or data |
| **D**ecide | [Who made the final call] | Accountable for the decision |

## Consequences

### What Changes
- [Concrete change 1]
- [Concrete change 2]

### What We Are Giving Up
- [Trade-off 1]
- [Trade-off 2]

### Next Steps
- [ ] [Action item 1] — Owner: [name], Due: [date]
- [ ] [Action item 2] — Owner: [name], Due: [date]

---

*Decision record created [date].*
```

### Step 4: Prompt for missing details

If the user provided only a short description, prompt for:
1. **Context** — What prompted this?
2. **Options** — What alternatives were considered?
3. **RAPID roles** — Who recommends, agrees, performs, inputs, decides?
4. **Consequences** — What changes as a result?

Fill in what you can infer. Mark unknowns as `[TBD]` for the user to complete.

### Step 5: Optionally create a linked GitHub Issue

If the decision requires follow-up actions, offer to create a tracking issue:

```bash
gh issue create --repo OWNER/REPO \
  --title "TD-NNNN: [Decision Title] — Follow-up" \
  --label "type:decision-needed" \
  --body "$(cat <<'EOF'
## Decision Record

See `operations/decisions/TD-NNNN-slug.md`

## Follow-up Actions

- [ ] [Action from consequences section]
- [ ] [Action from consequences section]

---
*Linked to decision record TD-NNNN*
EOF
)"
```

### Step 6: Optionally link to a solution play

If the decision is specific to a solution play, also place a copy or symlink in the play's decisions directory:

```
solution-plays/[fy]/[play]/decisions/TD-NNNN-slug.md
```

## File Naming Convention

```
TD-0001-chose-skillable-over-cloudlabs.md
TD-0002-approved-nvidia-mdf-allocation.md
TD-0003-shifted-to-hybrid-event-format.md
TD-0004-fy27-hackathon-program-restructure.md
```

## Decision Statuses

| Status | Meaning |
|--------|---------|
| **Proposed** | Under discussion, not yet decided |
| **Accepted** | Decision made, implementation underway |
| **Superseded** | Replaced by a later decision (link to successor) |
| **Deprecated** | No longer relevant due to changed circumstances |

When a decision is superseded, add to the original:
```markdown
> **Status:** Superseded by [TD-NNNN](./TD-NNNN-slug.md)
```

## Gotchas

- **Always create the file, even for "obvious" decisions.** The most contentious future debates are about decisions everyone assumed were documented. Write it down.
- **RAPID roles are often incomplete at creation time.** It is better to create the record with `[TBD]` roles and fill them in later than to delay creating the record. Prompt the user but do not block on missing roles.
- **The "Agree" role is not always needed.** For decisions within the team lead's authority, the Agree field can be "—" (no veto required). Only populate it when someone external has veto power (e.g., finance for budget decisions, legal for vendor contracts).
- **Decision numbers must be globally unique within the repo.** If your repo has decisions in multiple directories (e.g., `operations/decisions/` and `solution-plays/fy26/ai-apps/decisions/`), scan ALL directories when finding the next number to avoid collisions.
- **Slug collisions are possible.** If two decisions have similar titles, the slug may collide. Append the number to guarantee uniqueness: `TD-0005-chose-vendor-x.md`.
- **Do not backfill decisions retroactively in bulk.** If you discover 20 undocumented decisions, create records for the 3-5 most consequential ones. The rest can be noted in a single "historical decisions" summary.
- **Link decisions to their triggering issue.** If a GitHub Issue prompted the decision (e.g., `type:decision-needed`), reference it in the Context section and close the issue after the record is created.

## Related Skills

- `/mbr` — Monthly Business Review references key decisions made that month
- `/escalations` — Escalated items often result in decisions that should be recorded
- `/sp-health` — Strategic health scoring may surface decision gaps in solution plays
