---
name: git-conventions
description: Git workflow and commit conventions
---

# Git Conventions

## Commit Message Format

### Conventional Commits
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code change, no feature/fix |
| `perf` | Performance improvement |
| `test` | Adding/fixing tests |
| `chore` | Maintenance tasks |

### Examples
```
feat(orders): add batch order processing

Implements bulk order creation endpoint for high-volume channels.
Supports up to 100 orders per request with validation.

Closes #123
```

```
fix(beam): handle null variant mapping

Prevents NPE when Shopify returns product without variants.
Adds null check before accessing variant list.
```

## Branch Naming

### Format
```
<type>/<ticket>-<short-description>
```

### Examples
```
feat/BEAM-123-batch-product-sync
fix/PRISM-456-checkout-validation
chore/INF-789-update-dependencies
```

### Types
- `feat/` - New features
- `fix/` - Bug fixes
- `chore/` - Maintenance
- `docs/` - Documentation
- `refactor/` - Code refactoring
- `test/` - Test additions

## Pull Request Guidelines

### Title Format
```
[TICKET-123] Brief description of change
```

### PR Template
```markdown
## Summary
[1-3 sentences describing what this PR does]

## Changes
- Change 1
- Change 2

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if UI)
[Add screenshots for UI changes]

## Related Issues
Closes #123
```

### PR Checklist
- [ ] Code follows project conventions
- [ ] Tests pass locally
- [ ] Documentation updated
- [ ] No secrets in code
- [ ] Reviewed own changes before requesting review

## Merge Strategy

### Squash and Merge (Preferred)
- Use for feature branches
- Creates clean linear history
- Squash message should summarize all commits

### Rebase and Merge
- Use when individual commits are meaningful
- Each commit should be atomic and pass CI

### Never Direct Push to Main
- All changes through PRs
- Require at least one approval
- CI must pass

## Conflict Resolution

### Steps
1. Fetch latest main: `git fetch origin main`
2. Rebase your branch: `git rebase origin/main`
3. Resolve conflicts in each file
4. Continue rebase: `git rebase --continue`
5. Force push: `git push --force-with-lease`

### Tips
- Keep branches short-lived
- Sync with main frequently
- Communicate with team about large refactors
