---
name: Graphite
description: Graphite CLI detection and stacked PR workflow management
---

# Graphite Skill

> Auto-detect Graphite CLI and enable stacked PR workflows.

**Key Principle**: Detect once, ask once, remember forever.

---

## Overview

Graphite CLI (`gt`) enables stacked pull requests - a series of PRs where each depends on the previous one. This allows breaking large features into smaller, reviewable pieces.

**Why Stacked PRs?**
- Large features (500+ lines) broken into reviewable pieces (<200 lines)
- Each PR reviewed independently
- Merge progressively as approved
- Easier to isolate issues and rollback

---

## When to Use

**Trigger Detection**:
- During PR creation workflows
- When user says "create stacked PR"
- First commit operation in a new repo

**Trigger Workflow**:
- `/v-graphite:detect` - Check for CLI and set preference
- `/v-graphite:stack` - Create/manage stacked PRs

---

## Commands

| Command | Purpose |
|---------|---------|
| `/v-graphite:detect` | Check for gt CLI and save preference |
| `/v-graphite:stack` | Create or manage stacked PRs |

---

## Detection Flow

```
1. Check if gt command exists
   ↓
2. Check stored preference
   ├─ YES → Use stored preference
   └─ NO  ↓
3. Ask user: "Enable Graphite workflow?"
   ↓
4. Save preference to .claude/settings.json
   ↓
5. Never ask again
```

### Preference Storage

**Repo-level** (team preference):
```json
// .claude/settings.json
{
  "skills": {
    "git": {
      "graphite": {
        "enabled": true,
        "detected_version": "1.4.2"
      }
    }
  }
}
```

**Precedence**: Repo-level > User-level > Ask user

---

## Stacked PR Basics

### What Are Stacked PRs?

```
main
 └── PR #1: Backend API (200 lines)
      └── PR #2: Redux State (150 lines)
           └── PR #3: UI Components (180 lines)
```

Each PR depends on the previous. Merge from bottom to top.

### Good Candidates for Stacking

✅ **Use stacking for**:
- Large features (500+ lines) with natural breakpoints
- Backend → Frontend dependencies
- Refactor → Feature patterns
- Multi-layer implementations (DB → API → UI)

❌ **Don't stack**:
- Simple features (<200 lines total)
- Urgent hotfixes
- Independent features (use parallel PRs)

### Core Graphite Commands

```bash
# Create branch in stack
gt branch create <name>

# Create commit
gt commit create -m "message"

# Submit current branch as PR
gt branch submit

# Submit entire stack
gt stack submit

# Sync with main and restack
gt stack sync

# Navigate stack
gt branch up      # Move toward tip
gt branch down    # Move toward main

# View stack
gt log short
```

---

## Toggle Commands

Users can change preference anytime:

```
/git graphite on      # Enable
/git graphite off     # Disable
/git graphite status  # Show current setting
```

---

## Integration with PR Workflow

When creating PRs:

1. Check Graphite preference
2. If enabled AND Graphite installed:
   - Use `gt stack submit`
3. If disabled OR not installed:
   - Use `gh pr create`

---

## Handling Feedback on Stacked PRs

**CRITICAL**: Update the same branch, never create new PR.

**Feedback on bottom of stack**:
```bash
# Checkout branch with feedback
gt branch checkout feat/api

# Make fixes
git commit -m "fix: address feedback"

# Sync changes up the stack
gt stack sync

# Push
git push origin feat/api
```

**Feedback on middle of stack**:
```bash
# Only PRs above are rebased
gt stack sync
```

**Feedback on top of stack**:
```bash
# No restack needed, just push
git push origin feat/ui
```

---

## Quality Standards for Stacks

Each PR in stack must still:
- Be <200 lines (preferred)
- Pass tests
- Pass security review
- Be self-contained (compilable)
- Have clear description

---

## Reference Files

| File | Purpose |
|------|---------|
| `violet-brain/shared/skills/git/sections/06-graphite-detection.md` | Detection logic |
| `violet-brain/shared/skills/git/sections/07-stacked-prs.md` | Stacked PR workflow |
| `violet-brain/patterns/pr-quality.md` | PR quality standards |

---

*Skill Version: 1.0.0*
