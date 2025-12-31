---
name: fix
description: "STAGE 3: Fix code on the original PR branch"
---

# PR Feedback Fix Command

> Address PR feedback by making code fixes on the original branch.

**Stage**: STAGE 3 (Fix)

---

## Prerequisites

- Extraction completed (STAGE 0+1)
- Learning integration completed or skipped (STAGE 2)
- Extraction file exists with action items

## Critical Branch Workflow

**ALWAYS update the SAME branch** as the original PR. Never create a new PR for fixes.

```bash
# ✅ RIGHT - Checkout the PR's original branch
git checkout feat/my-feature

# ❌ WRONG - Don't create new branch
git checkout -b fix/pr-123-feedback  # NO!
```

**Why**: The PR is attached to the original branch. Pushing to that branch updates the PR automatically.

---

## Execution

### Step 1: Get Original PR Branch

```bash
# Fetch the branch name from PR
gh pr view {number} --json headRefName --jq '.headRefName'

# Example for PR #1703:
# Output: feat/pre-reg-list
```

### Step 2: Checkout Original Branch

```bash
git checkout {branch_name}

# Pull latest to ensure we have all commits
git pull origin {branch_name}
```

### Step 3: Review Issues to Fix

Read the extraction file for action items:
- **Critical**: Must fix before merge (blocking)
- **Important**: Should fix (strong recommendation)
- **Nice-to-Have**: Optional improvements

Present to user:
```
Issues to Fix from PR #{number}:

Critical (must fix):
1. {file}:{line} - {issue_description}
2. {file}:{line} - {issue_description}

Important (should fix):
3. {file}:{line} - {issue_description}

Nice-to-Have (optional):
4. {file}:{line} - {issue_description}

Ready to fix? (y/n)
```

### Step 4: Make Fixes

Address each issue from the extraction:

```bash
# Fix issue 1
# Edit file: {file}:{line}

# Fix issue 2
# Edit file: {file}:{line}

# ... continue for all issues
```

### Step 5: Commit Fixes

```bash
git add {files}

git commit -m "fix: address PR #{number} feedback

- {fix_1_description}
- {fix_2_description}
- {fix_3_description}

Addresses feedback from: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md"
```

### Step 6: Push to Same Branch (Updates PR)

```bash
# This updates PR #{number} automatically
git push origin {branch_name}
```

**Result**: PR now shows the new commit. Reviewers can see fixes in context.

### Step 7: Record Fixes

Create fixes record linking extraction to commits:

**File**: `violet-brain/pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-fixes.md`

```markdown
# PR #{number} Fixes

**Extraction**: `pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md`
**PR**: {pr_url}
**Branch**: {branch_name}
**Commits**: {commit_hashes}

## Fixes Applied

### Fix 1: {title} (Critical)
**Issue**: {issue_description}
**File**: {file}:{line}
**Commit**: {hash}
**Status**: ✅ Fixed

### Fix 2: {title} (Critical)
**Issue**: {issue_description}
**File**: {file}:{line}
**Commit**: {hash}
**Status**: ✅ Fixed

## Learning Integration

**Brain Updates Committed**: {commit_message}
**Files Updated**:
- {file_1}
- {file_2}

## Re-Review Requested

**Status**: Awaiting re-review from reviewers
```

### Step 8: Request Re-Review

```bash
# Request reviewers to re-review the PR
gh pr review {number} --request-review

# Or comment to notify reviewers
gh pr comment {number} --body "✅ All feedback addressed. Ready for re-review.

Fixes:
- {fix_1}
- {fix_2}
- {fix_3}"
```

---

## Multi-PR Fix Flow

When fixing multiple PRs from a session:

```
# Fix PR #1703
git checkout feat/pre-reg-list
# ... make fixes ...
git commit -m "fix: address PR #1703 feedback"
git push origin feat/pre-reg-list

# Fix PR #1704
git checkout feat/pre-reg-detail
# ... make fixes ...
git commit -m "fix: address PR #1704 feedback"
git push origin feat/pre-reg-detail

# Fix PR #1705
git checkout feat/pre-reg-filtering
# ... make fixes ...
git commit -m "fix: address PR #1705 feedback"
git push origin feat/pre-reg-filtering
```

**Important**: Each PR is on its own branch. Fix and push sequentially.

---

## Commit Message Format

```
fix: address PR #{number} feedback

- {fix_description_1}
- {fix_description_2}
- {fix_description_3}

Addresses feedback from: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
```

---

## Common Fix Categories

### Type Errors
```typescript
// Before (error)
const data: IResponse = fetchData()

// After (fixed)
const data: ICorrectResponse = fetchData()
```

### Nullish Coalescing
```typescript
// Before (bug)
const page = data.page || 1  // Prevents page 0

// After (fixed)
const page = data.page ?? 1  // Allows page 0
```

### Import Order
```typescript
// Before (lint error)
import { useState } from 'react'
import api from './api'
import type { User } from './types'

// After (fixed - types last, alphabetized)
import { useState } from 'react'
import api from './api'
import type { User } from './types'
```

### Logic Errors
```typescript
// Before (bug)
if (items.length > 0) {
  return items[items.length]  // Off by one
}

// After (fixed)
if (items.length > 0) {
  return items[items.length - 1]
}
```

---

## Verification Checklist

Before pushing fixes:

- [ ] All critical issues addressed
- [ ] All important issues addressed (or justified why skipped)
- [ ] Tests pass locally
- [ ] Lint passes locally
- [ ] No new issues introduced
- [ ] Commit message references extraction file
- [ ] On correct branch (original PR branch)

---

## Success Criteria

- [ ] Checked out original PR branch
- [ ] All critical issues fixed
- [ ] All important issues fixed (or documented why not)
- [ ] Changes committed with proper attribution
- [ ] Pushed to same branch (PR updated)
- [ ] Fixes record created in pr-learnings/
- [ ] Re-review requested

---

## Troubleshooting

### "Branch doesn't exist locally"

```bash
# Fetch the branch from remote
git fetch origin {branch_name}
git checkout {branch_name}
```

### "Merge conflicts after pull"

```bash
# Resolve conflicts
git status  # See conflicted files
# Edit files to resolve
git add {resolved_files}
git commit -m "merge: resolve conflicts"
```

### "CI still fails after fixes"

1. Read CI output carefully
2. Check if there are additional issues beyond what extraction captured
3. Fix new issues and push again
4. Update extraction if new patterns found

---

*Command Version: 1.0.0*
