---
name: check
description: "Run file detection and prompt for removal of unrelated files"
---

# File Validation Check Command

> Detect and remove unrelated files before PR creation.

**Policy**: STRICT - Unrelated files are removed, not suggested for cleanup.

---

## Execution

### Step 1: List Changed Files

```bash
# Show all unstaged and staged files
git status

# List files that differ from main
git diff --name-only origin/main..HEAD
```

### Step 2: Detect Unrelated Files

For each file, check against detection patterns:

**Path-based patterns** (flag if path contains):
- `local`, `test-data`, `tmp`, `debug`, `dump`, `cache`
- `redis-cluster`, `docker-compose.override`

**Extension-based patterns** (flag if ends with):
- `.log`, `.pid`, `.rdb`, `.dump`, `.swp`, `.pyc`

**Name-based patterns** (flag if matches):
- `.DS_Store`, `Thumbs.db`, `desktop.ini`
- `.env.local`, `.env.*.local`

**Scope-based detection**:
- Is file mentioned in PR description or Linear issue?
- Is file type expected for this PR?

### Step 3: Present Findings

If unrelated files detected:

```
⚠️  Unrelated Files Detected

The following files are not related to this PR:

| File | Reason |
|------|--------|
| redis-cluster/dump.rdb | Local dev artifact |
| .env.local | Local config file |
| tmp/debug.log | Debug file |

These files will be REMOVED from the PR.

[✓] Remove them (Recommended)
[✗] Keep them (User confirms they're intentional)
```

### Step 4: Handle Response

**User chooses Remove** (✓):

```bash
# Unstage unrelated files
git restore --staged redis-cluster/dump.rdb .env.local tmp/debug.log

# If files were already committed
git reset HEAD~1
git add <relevant-files-only>
git commit -m "chore: remove unrelated files"
```

Output:
```
✅ Removed 3 unrelated files from staging
   - redis-cluster/dump.rdb
   - .env.local
   - tmp/debug.log

Ready to continue with PR creation.
```

**User chooses Keep** (✗):

```
Okay, keeping the files.

Please add this to your PR description:

---
## Unusual Files Included

This PR includes files that may appear unrelated:
- redis-cluster/dump.rdb: [Your justification]
- .env.local: [Your justification]
- tmp/debug.log: [Your justification]
---

This documents your decision for reviewers.
```

### Step 5: Update .gitignore (Optional)

If detected files should always be ignored:

```bash
# Add to .gitignore
echo "redis-cluster/" >> .gitignore
echo ".env.local" >> .gitignore

# Stage .gitignore update
git add .gitignore
```

Prompt:
```
Should I add these patterns to .gitignore?

[✓] Yes - Prevent these files from being committed again
[✗] No - They're intentionally tracked
```

---

## Detection Categories

### Local Development Artifacts

| Pattern | Match Type | Example |
|---------|------------|---------|
| `redis-cluster/` | Directory | `redis-cluster/dump.rdb` |
| `dump.rdb` | File | Root or nested |
| `.env.local` | File | Local environment |
| `docker-compose.override.yml` | File | Local Docker config |
| `local.settings.json` | File | Local settings |

**Action**: Always flag. These never belong in PRs.

### Debug & Temporary Files

| Pattern | Match Type | Example |
|---------|------------|---------|
| `tmp/` | Directory | `tmp/debug.log` |
| `debug/` | Directory | `debug/output.txt` |
| `*.log` | Extension | `app.log`, `error.log` |
| `*.pid` | Extension | `server.pid` |
| `console-output.txt` | File | Debug capture |

**Action**: Always flag. Remove before PR.

### OS & IDE Artifacts

| Pattern | Match Type | Example |
|---------|------------|---------|
| `.DS_Store` | File | macOS artifact |
| `Thumbs.db` | File | Windows artifact |
| `.vscode/` | Directory | VS Code settings |
| `.idea/` | Directory | JetBrains settings |
| `*.swp` | Extension | Vim swap files |

**Action**: Flag and suggest adding to .gitignore.

### Build Artifacts

| Pattern | Match Type | Example |
|---------|------------|---------|
| `node_modules/` | Directory | npm dependencies |
| `dist/` | Directory | Build output |
| `build/` | Directory | Build output |
| `__pycache__/` | Directory | Python cache |
| `*.pyc` | Extension | Python compiled |

**Action**: Flag. Should be in .gitignore already.

---

## Scope-Based Detection

When pattern matching isn't enough, check scope:

1. **Read PR description** - What does this PR claim to change?
2. **Read Linear issue** - What's the scope of the ticket?
3. **Match files to scope**:
   - Backend PR should have backend files
   - Frontend PR should have frontend files
   - If file doesn't match scope, flag it

**Example**:
```
PR #123: "Add new API endpoint for users"
Linear: INT-456

Files:
- src/api/users.ts ✅ (matches scope)
- src/api/users.test.ts ✅ (matches scope)
- src/components/Header.tsx ❌ (unrelated - frontend in API PR)
```

---

## Multi-Agent Git Safety

**CRITICAL**: Never use `git add -A` or `git add .`

```bash
# ❌ WRONG - stages everything
git add -A
git add .

# ✅ RIGHT - stage specific files
git add src/api/users.ts src/api/users.test.ts
```

When multiple Claude sessions work in the same repo, `git add -A` could stage another agent's work.

---

## Integration with PR Workflow

This command integrates into the PR creation flow:

```
Developer: /commit-commands:commit-push-pr
    ↓
1. Run /v-file-validation:check
    ↓
2. If unrelated files found:
   - Prompt for removal
   - Wait for user decision
   - Clean up if approved
    ↓
3. Continue with commit + PR
```

---

## Success Criteria

- [ ] All changed files listed
- [ ] Detection patterns applied
- [ ] Unrelated files identified
- [ ] User prompted with clear options
- [ ] Files removed if approved
- [ ] .gitignore updated if needed
- [ ] PR contains only relevant files

---

## Troubleshooting

### "File I need is being flagged"

If a legitimate file matches a pattern:

1. **Explain why it's needed** in PR description
2. **Use Keep option** to override detection
3. **Consider renaming** if pattern match is ambiguous

### "Staged files from another session"

If you see files you didn't create:

```bash
# Check what's staged
git status

# Unstage other work
git restore --staged <other-files>

# Only stage your files
git add <your-files>
```

### "File already committed"

If unrelated files are already in a commit:

```bash
# Reset last commit (keeps changes unstaged)
git reset HEAD~1

# Restage only relevant files
git add <relevant-files>

# Recommit
git commit -m "Your message"
```

---

*Command Version: 1.0.0*
