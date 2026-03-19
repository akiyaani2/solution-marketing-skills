---
name: git-sync
description: Automated git pull/commit/push with standardized commit messages. Safety features include branch validation, force-push prevention, merge conflict detection, and reporting of both local file changes and GitHub API changes.
allowed-tools: [Bash]
trigger_phrases:
  - "/git-sync"
  - "/git-sync -m"
  - "git sync"
  - "sync everything"
  - "commit and push"
  - "push everything"
---

# Git Sync

Automates the complete git sync workflow: pull latest, stage changes, commit with a standardized message, and push to remote. Includes safety checks, merge conflict handling, and a unified report of both local file changes and GitHub API changes made during the session.

## Trigger Phrases

- `/git-sync` — Full sync (pull, commit, push)
- `/git-sync -m "custom message"` — Sync with a custom commit message
- `/git-sync --branch [name]` — Sync a specific branch
- `/git-sync --no-push` — Commit locally without pushing
- `/git-sync --dry-run` — Preview what would happen without executing

## What This Skill Does

### Two Types of Changes

This skill handles and reports **two types of changes** made during a work session:

| Type | What It Is | How It Syncs |
|------|-----------|-------------|
| **Local File Changes** | Edits to markdown, code, configs in the repo | Git add + commit + push |
| **GitHub API Changes** | Issues created, labels added, comments posted via `gh` CLI | Already live on GitHub (reported for visibility) |

GitHub API changes (via `gh` CLI) are immediately live on GitHub the moment they are executed. They do not need git commit/push. This skill reports them alongside local changes so you have a complete picture of everything that changed in the session.

### Workflow Steps

#### Step 1: Pre-Flight Checks

```bash
# Verify branch
BRANCH=$(git branch --show-current)
if [ -z "$BRANCH" ]; then
  echo "ERROR: Not on a valid branch (detached HEAD?)"
  exit 1
fi

# Verify remote
git remote -v

# Check current status
git status --porcelain
```

**Safety checks performed:**
- Confirm you are on a valid branch (not detached HEAD)
- Confirm the remote exists
- Check for uncommitted changes
- Warn if on a protected branch with unusual operations

#### Step 2: Pull Latest Changes

```bash
git pull origin $(git branch --show-current)
```

If merge conflicts are detected, STOP and present options:
1. Resolve manually (recommended)
2. Abort merge and retry later
3. Keep local version

#### Step 3: Stage and Commit

```bash
# Stage all changes
git add .

# Commit with standardized message (or custom if -m provided)
git commit -m "<type>(<scope>): <description>"
```

#### Step 4: Push to Remote

```bash
git push origin $(git branch --show-current)

# If push rejected (remote has new commits):
# Auto-pull and retry once
```

#### Step 5: Report Summary

Report both local file changes and any GitHub API changes made during the session.

### Commit Message Format

```
<type>(<scope>): <description>

<body — optional, for multi-file changes>

Co-Authored-By: Claude <noreply@anthropic.com>
```

**Types:**

| Type | When to Use | Example |
|------|------------|---------|
| `feat` | New feature, content, or capability | `feat(events): Add Build 2026 workback schedule` |
| `fix` | Bug fix, correction, or error repair | `fix(calendar): Correct Q3 event dates` |
| `docs` | Documentation changes only | `docs(handbook): Update team roster` |
| `chore` | Maintenance, config, housekeeping | `chore(labels): Standardize label prefixes` |
| `refactor` | Restructuring without changing behavior | `refactor(skills): Reorganize skill file structure` |
| `style` | Formatting, whitespace, naming | `style(templates): Fix markdown table alignment` |

**Scopes (customize to your repo):**

| Scope | Content Area |
|-------|-------------|
| `events` | Event planning and operations |
| `content` | Content pipeline and production |
| `handbook` | Team processes and reference docs |
| `operations` | Ops, budget, meetings |
| `templates` | Issue templates and forms |
| `workflows` | GitHub Actions and automation |
| `skills` | Claude Code skills |
| `config` | Configuration files |
| `programs` | Program-specific content |

**Auto-generation logic:** Analyze the changed files to determine type and scope:
- If all files are in `docs/` or `handbook/` -> `docs`
- If new files are created -> `feat`
- If files are restructured but content is unchanged -> `refactor`
- If templates or configs are updated -> `chore`
- Use the most common directory among changed files as the scope

### Safety Features

**Protected branches — never force push:**
- `main`
- `master`
- `production`
- `release/*`

**Automatic safeguards:**
- Validates branch exists before pushing
- Detects merge conflicts and halts
- Prevents force push (`--force`, `-f`) on protected branches
- Shows preview of changes before commit
- Reports file count and diff stats

**Merge conflict handling:**

```
MERGE CONFLICT DETECTED

Conflicting files:
- path/to/file1.md
- path/to/file2.md

Options:
1. Resolve manually (recommended) — edit the files, then re-run /git-sync
2. Abort merge and retry later — git merge --abort
3. Keep local version — git checkout --ours [files]
4. Keep remote version — git checkout --theirs [files]

What would you like to do?
```

### GitHub API Change Reporting

At the end of sync, report any GitHub API operations made during the session:

```bash
# Find issues created today by the authenticated user
TODAY=$(date '+%Y-%m-%d')
gh issue list --state all --json number,title,createdAt --limit 50 | jq "[.[] | select(.createdAt | startswith(\"$TODAY\"))]"
```

**Report format:**

```
GitHub API Changes (already live):
- Issues created: [N] (#1, #2, #3)
- Issues updated: [N] (labels, assignees, status changes)
- Labels created: [N] (label1, label2)
- Comments added: [N]
```

### Example Outputs

**Basic sync with changes:**

```
SYNCING WITH REMOTE

Step 1: Pre-flight checks...
  On branch: main
  Remote: origin (github.com)

Step 2: Pulling latest changes...
  Already up to date

Step 3: Local changes found — 8 files modified
  operations/meetings/team-sync-2026-03-19.md (new)
  handbook/glossary.md (modified)
  programs/hackathons/nvidia/README.md (modified)
  ... and 5 more

Step 4: Committing...
  feat(operations): Add team sync notes and update glossary

Step 5: Pushing to origin/main...
  Success

Step 6: GitHub API changes (already live)
  Issues created: 3 (#145, #146, #147)
  Issues updated: 12 (labels added)

SYNC COMPLETE (22 seconds)
  Local: 8 files changed (+234, -18)
  GitHub: 3 issues created, 12 updated
```

**No local changes, but API changes exist:**

```
SYNCING WITH REMOTE

Step 1-2: Pre-flight and pull...
  On branch: main — up to date

Step 3: No local file changes to commit.

Step 6: GitHub API changes (already live)
  Issues created: 5 (#150-#154)
  Labels created: 2 (func:workshops, event:summit-2026)
  Comments added: 8

SYNC COMPLETE
  Local repo: Already in sync
  GitHub: All API changes are live
```

**Dry run:**

```
DRY RUN MODE (no changes will be made)

Would pull from: origin/main
Would commit: 3 files
  - operations/decisions/TD-0042-vendor-selection.md (new)
  - handbook/how-we-work.md (modified)
  - events/build-2026/speakers.md (modified)

Commit message preview:
  feat(operations): Add vendor selection decision record

Ready to sync? Run /git-sync to execute.
```

**Custom commit message:**

```
User: /git-sync -m "Add Q4 planning materials"

SYNCING WITH REMOTE
  Using custom commit message...

  Committed: Add Q4 planning materials
  Pushed to origin/main

SYNC COMPLETE (18 seconds)
  Local: 5 files changed (+312, -0)
```

## Execution Script

```bash
#!/bin/bash
set -e

BRANCH=$(git branch --show-current)
REMOTE="origin"

echo "SYNCING WITH REMOTE"
echo ""

# Step 1: Pre-flight
echo "Step 1: Pre-flight checks..."
if [ -z "$BRANCH" ]; then
  echo "ERROR: Not on a valid branch"
  exit 1
fi
echo "  On branch: $BRANCH"

if ! git remote | grep -q "$REMOTE"; then
  echo "ERROR: Remote '$REMOTE' not found"
  exit 1
fi
echo "  Remote: $REMOTE"
echo ""

# Step 2: Pull
echo "Step 2: Pulling latest changes..."
if ! git pull $REMOTE $BRANCH; then
  echo "ERROR: Pull failed — check for merge conflicts"
  git status
  exit 1
fi
echo ""

# Step 3: Check for local changes
echo "Step 3: Checking for local changes..."
CHANGES=$(git status --porcelain)
if [ -z "$CHANGES" ]; then
  echo "  No local file changes to commit"
else
  FILE_COUNT=$(echo "$CHANGES" | wc -l | tr -d ' ')
  echo "  Found $FILE_COUNT modified files"
  echo ""

  # Step 4: Stage and commit
  echo "Step 4: Committing changes..."
  git add .
  # Claude generates the commit message based on changed files

  # Step 5: Push
  echo "Step 5: Pushing to remote..."
  if ! git push $REMOTE $BRANCH; then
    echo "  Push failed, pulling and retrying..."
    git pull $REMOTE $BRANCH
    git push $REMOTE $BRANCH
  fi
fi
echo ""

# Step 6: Report
echo "Step 6: Session summary..."
echo ""
echo "SYNC COMPLETE"
```

## Gotchas

- **`git add .` stages everything including untracked files.** Before committing, review the staged files list. Watch for accidentally staging sensitive files (.env, credentials, API keys), large binaries, or OS-generated files (.DS_Store, Thumbs.db). Use a `.gitignore` to prevent this.
- **GitHub API changes are already live.** If you created issues, added labels, or posted comments via `gh` CLI during the session, those changes are immediately visible on GitHub. The git sync only handles local file changes. The API change report is purely informational.
- **Merge conflicts require human judgment.** The auto-pull-and-retry handles simple cases (non-overlapping changes). True conflicts where the same lines were edited need manual resolution. Do not auto-resolve by picking "ours" or "theirs" without the user's explicit instruction.
- **Force push prevention is critical.** Force pushing to main/master can destroy other people's work. The safety check is non-negotiable. If the user truly needs to force push, they must do it manually outside this skill.
- **Commit message quality matters.** Auto-generated commit messages should accurately describe the changes. A message like "update files" is unhelpful. Analyze the actual files changed and write a message that a team member reading the git log in 6 months would understand.
- **Large commits are a code smell.** If git sync is about to commit 50+ files, pause and ask the user if this should be broken into smaller, logical commits. Large commits are harder to review and revert.
- **The --no-push flag is useful for work in progress.** If you are mid-task and want to checkpoint your work without pushing to remote, use `--no-push`. This is especially useful on shared branches where half-finished work would confuse teammates.
- **Background processes and running servers can create unexpected file changes.** Build artifacts, log files, and temp files may appear in the working directory. Review the file list before committing, especially if you see files you did not intentionally edit.
- **Git hooks may run on commit.** If the repository has pre-commit hooks (linting, formatting, tests), they will run during the commit step. If a hook fails, the commit does not happen. Fix the issue and create a new commit — do NOT amend the previous commit.
