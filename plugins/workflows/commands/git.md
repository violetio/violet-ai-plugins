# Git Skill Toggle

Toggle the git workflow skill on or off for this user.

**Usage**: `/git [on|off|status]`

---

## What This Command Does

When the user invokes `/git`, parse the argument and manage their git skill settings.

### Arguments

- `on` - Enable the git workflow skill
- `off` - Disable the git workflow skill
- `status` - Show current git skill status (default if no argument)

---

## Implementation

### Check Current Status

First, check if the user has git settings configured:

```bash
cat ~/.claude/settings.json 2>/dev/null
```

**If file doesn't exist or git settings missing**: The skill is **disabled** (default state).

### Handle Each Argument

#### `/git status` (or `/git` with no argument)

1. Read `~/.claude/settings.json`
2. Check if `skills.git.enabled` exists and is `true`
3. Report status to user:

**If enabled:**
```
Git workflow skill: ENABLED

Current conventions:
- Commit style: conventional
- Branch prefixes: required
- PR templates: enabled

Run `/git off` to disable.
```

**If disabled (or no settings):**
```
Git workflow skill: DISABLED (default)

When enabled, I'll follow conventions for:
- Conventional commits (feat, fix, docs, etc.)
- Branch naming (type/ticket-description)
- PR templates and review checklists
- Conflict resolution guidance

Run `/git on` to enable.
```

#### `/git on`

1. Read existing `~/.claude/settings.json` (or create empty object if doesn't exist)
2. Set `skills.git.enabled = true` with default conventions
3. Write back to file
4. Confirm to user

**Settings to write:**
```json
{
  "skills": {
    "git": {
      "enabled": true,
      "conventions": {
        "commit_style": "conventional",
        "branch_prefix": true,
        "pr_template": true
      }
    }
  }
}
```

**Response:**
```
Git workflow skill: ENABLED

I'll now follow conventions for commits, branches, and PRs.
See `shared/skills/git/SKILL.md` for full documentation.

Run `/git off` to disable.
```

#### `/git off`

1. Read existing `~/.claude/settings.json`
2. Set `skills.git.enabled = false`
3. Write back to file
4. Confirm to user

**Response:**
```
Git workflow skill: DISABLED

Using standard git commands without conventions.

Run `/git on` to re-enable.
```

---

## Edge Cases

### No Settings File Exists

When `/git on` is run and `~/.claude/settings.json` doesn't exist:

1. Create the file with git settings
2. Ensure directory exists (`mkdir -p ~/.claude`)
3. Write the settings JSON

### Settings File Exists But No Git Section

When the file exists but has no `skills.git` section:

1. Read existing settings
2. Merge in git settings (preserve other settings)
3. Write back

### Invalid JSON in Settings File

If the settings file contains invalid JSON:

1. Warn the user
2. Offer to backup and recreate
3. Don't silently overwrite

---

## Example Session

```
User: /git
Claude: Git workflow skill: DISABLED (default)

When enabled, I'll follow conventions for:
- Conventional commits (feat, fix, docs, etc.)
- Branch naming (type/ticket-description)
- PR templates and review checklists

Run `/git on` to enable.

User: /git on
Claude: Git workflow skill: ENABLED

I'll now follow conventions for commits, branches, and PRs.
Run `/git off` to disable.

User: /git status
Claude: Git workflow skill: ENABLED

Current conventions:
- Commit style: conventional
- Branch prefixes: required
- PR templates: enabled

Run `/git off` to disable.
```

---

## Related

- [Git Skill Documentation](../../shared/skills/git/SKILL.md)
- [Commits Section](../../shared/skills/git/sections/02-commits.md)
- [Branches Section](../../shared/skills/git/sections/03-branches.md)
