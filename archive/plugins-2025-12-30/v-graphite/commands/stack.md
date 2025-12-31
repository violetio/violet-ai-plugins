---
name: stack
description: "Create and manage stacked PRs using Graphite"
---

# Graphite Stack Command

> Create and manage stacked pull requests.

**Prerequisite**: Graphite must be enabled (`/v-graphite:detect`)

---

## When to Use

- Large feature (500+ lines) that breaks into phases
- Backend → Frontend dependencies
- Refactor → Feature patterns
- Multi-layer implementations (DB → API → UI)

**Don't use for**:
- Simple features (<200 lines)
- Urgent hotfixes
- Independent features (use parallel PRs)

---

## Execution

### Create a Stack

#### Step 1: Start from Main

```bash
git checkout main
git pull origin main
```

#### Step 2: Create First Branch (Foundation)

```bash
gt branch create feat/my-feature-api

# Make changes
# ... edit files ...

# Commit
gt commit create -m "feat(api): add API endpoints"

# Submit as PR
gt branch submit
```

**Output**:
```
✅ Created PR #101: Add API endpoints
📎 https://github.com/org/repo/pull/101
```

#### Step 3: Create Second Branch (Depends on First)

```bash
# Create on top of first branch
gt branch create feat/my-feature-redux --parent feat/my-feature-api

# Make changes
# ... edit files ...

# Commit
gt commit create -m "feat(redux): add state management"

# Submit as PR
gt branch submit
```

**Output**:
```
✅ Created PR #102: Add state management
📎 https://github.com/org/repo/pull/102
⛓️  Depends on: PR #101
```

#### Step 4: Create Third Branch (Depends on Second)

```bash
gt branch create feat/my-feature-ui --parent feat/my-feature-redux

# Make changes
# ... edit files ...

# Commit
gt commit create -m "feat(ui): add UI components"

# Submit as PR
gt branch submit
```

### Submit Entire Stack at Once

```bash
# From any branch in the stack
gt stack submit
```

**Output**:
```
✅ Created PR #101: Add API endpoints
✅ Created PR #102: Add state management (depends on #101)
✅ Created PR #103: Add UI components (depends on #102)

Stack visualization:
main
 └── feat/my-feature-api (PR #101)
      └── feat/my-feature-redux (PR #102)
           └── feat/my-feature-ui (PR #103) ← current
```

---

## Manage a Stack

### View Stack

```bash
gt log short
```

**Output**:
```
◉ feat/my-feature-ui (current)
│ feat(ui): add UI components
◉ feat/my-feature-redux
│ feat(redux): add state management
◉ feat/my-feature-api
│ feat(api): add API endpoints
◯ main
```

### Navigate Stack

```bash
# Move up (toward main)
gt branch down

# Move down (toward tip)
gt branch up

# Jump to specific branch
gt branch checkout feat/my-feature-redux
```

### Sync Stack

After changes to main or when lower PR merges:

```bash
gt stack sync
```

This automatically:
- Rebases all branches on updated base
- Resolves conflicts (prompts if needed)
- Updates all PRs

---

## Handle Feedback

### Feedback on Bottom of Stack

**Scenario**: Feedback on PR #101 (foundation)

```bash
# Checkout branch
gt branch checkout feat/my-feature-api

# Make fixes
# ... edit files ...

# Commit
gt commit create -m "fix: address PR feedback"

# Sync stack (rebases PRs above)
gt stack sync

# Push
git push origin feat/my-feature-api
```

**Result**: All PRs above automatically rebased.

### Feedback on Middle of Stack

**Scenario**: Feedback on PR #102

```bash
gt branch checkout feat/my-feature-redux

# Make fixes
gt commit create -m "fix: address PR feedback"

# Sync stack (only rebases PRs above)
gt stack sync

git push origin feat/my-feature-redux
```

**Result**: PR #103 rebased, PR #101 unaffected.

### Feedback on Top of Stack

**Scenario**: Feedback on PR #103

```bash
gt branch checkout feat/my-feature-ui

# Make fixes
gt commit create -m "fix: address PR feedback"

# No sync needed - just push
git push origin feat/my-feature-ui
```

---

## Merge Stack

### Merge Order: Bottom to Top

```
1. Merge PR #101 (API) to main
   ↓
2. gt stack sync (rebases remaining)
   ↓
3. Merge PR #102 (Redux) to main
   ↓
4. gt stack sync
   ↓
5. Merge PR #103 (UI) to main
```

### After Each Merge

```bash
git checkout main
git pull origin main

# Restack remaining branches
gt stack sync
```

Graphite handles rebasing automatically.

---

## Handle Conflicts

### During Sync

```bash
gt stack sync

# If conflict:
# ⚠️  Conflict in feat/my-feature-redux
# Please resolve conflicts and run: gt stack sync --continue

# Resolve conflicts
git status
# ... edit conflicting files ...

# Mark resolved
git add <resolved-files>

# Continue
gt stack sync --continue
```

---

## Common Patterns

### Pattern 1: Refactor-Then-Feature

```
main
 └── refactor/extract-utilities
      └── feat/new-feature
```

### Pattern 2: Backend-Then-Frontend

```
main
 └── feat/backend-api
      └── feat/frontend-ui
```

### Pattern 3: Multi-Layer

```
main
 └── feat/database-schema
      └── feat/api-endpoints
           └── feat/business-logic
                └── feat/ui-components
```

---

## Quality Standards

Each PR in stack must:

- [ ] <200 lines (preferred)
- [ ] Tests pass
- [ ] Security review passes
- [ ] Self-contained (compilable)
- [ ] Clear description

**Note**: Each PR undergoes full review cycle.

---

## Best Practices

### DO:
- ✅ Break at logical boundaries
- ✅ Keep each PR reviewable (<200 lines)
- ✅ Make each PR independently testable
- ✅ Write clear descriptions with stack context
- ✅ Sync stack after every merge

### DON'T:
- ❌ Create stacks for simple features
- ❌ Make PRs depend on unrelated PRs
- ❌ Let stacks grow too tall (>5 PRs)
- ❌ Forget to sync after merges
- ❌ Rebase manually (use gt stack sync)

---

## Troubleshooting

### "Cannot submit: No remote branch"

```bash
gt branch submit --publish
```

### "Conflicts when syncing"

```bash
# Check conflicts
git status

# Resolve and continue
git add <files>
gt stack sync --continue
```

### "PR shows wrong diff"

Base branch may be incorrect:

```bash
gh pr edit {number} --base feat/parent-branch
```

### "Lost track of stack"

```bash
# View stack
gt log short

# Rebuild metadata
gt repo sync
```

---

## Success Criteria

- [ ] Stack created with proper dependencies
- [ ] Each PR is reviewable size
- [ ] Stack synced after changes
- [ ] Feedback handled on correct branch
- [ ] Merge order followed (bottom to top)

---

*Command Version: 1.0.0*
