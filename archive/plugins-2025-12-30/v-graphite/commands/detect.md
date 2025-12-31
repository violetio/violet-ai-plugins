---
name: detect
description: "Check for Graphite CLI and save preference"
---

# Graphite Detect Command

> Detect Graphite CLI and configure preference for stacked PR workflow.

**Principle**: Detect once, ask once, remember forever.

---

## Execution

### Step 1: Check for Graphite CLI

```bash
# Check if gt command exists
which gt

# If found, get version
gt --version
```

**Expected output** (if installed):
```
/usr/local/bin/gt
gt version 1.4.2
```

### Step 2: Check for Existing Preference

```bash
# Check repo-level settings
cat .claude/settings.json | grep graphite

# Check user-level settings
cat ~/.claude/settings.json | grep graphite
```

**Precedence**: Repo-level > User-level > Ask user

### Step 3: Handle Based on Detection

#### Scenario A: Graphite Not Installed

```
Graphite CLI not found.

Using standard GitHub PR workflow.

To install Graphite:
npm install -g @withgraphite/graphite-cli
```

No prompt needed - proceed with standard `gh` commands.

#### Scenario B: Preference Already Exists

If `.claude/settings.json` has Graphite preference:

```
✓ Graphite preference found: {enabled|disabled}
```

No prompt needed - use stored preference.

#### Scenario C: First-Time Detection

If Graphite installed but no preference stored:

```
🔍 Detected Graphite CLI (gt version 1.4.2)

Graphite enables stacked pull requests for breaking large features
into smaller, dependent PRs.

Enable Graphite workflow for this repo?

[✓] Yes - Use Graphite for stacked PRs (Recommended)
[✗] No - Use standard GitHub workflow
[?] What's Graphite? - Show documentation
```

### Step 4: Save Preference

**If user chooses Yes**:

```bash
mkdir -p .claude

# Write settings
cat > .claude/settings.json << 'EOF'
{
  "skills": {
    "git": {
      "graphite": {
        "enabled": true,
        "detected_version": "1.4.2",
        "detected_date": "2025-12-23"
      }
    }
  }
}
EOF
```

Output:
```
✅ Graphite workflow enabled

Preference saved to: .claude/settings.json
You can change this anytime with: /git graphite off
```

**If user chooses No**:

```bash
mkdir -p .claude

cat > .claude/settings.json << 'EOF'
{
  "skills": {
    "git": {
      "graphite": {
        "enabled": false,
        "detected_version": "1.4.2",
        "detected_date": "2025-12-23"
      }
    }
  }
}
EOF
```

Output:
```
❌ Graphite workflow disabled

Using standard GitHub PR workflow.
You can enable later with: /git graphite on
```

**If user chooses What's Graphite?**:

Provide brief explanation and re-prompt.

---

## Toggle Commands

User can change preference anytime:

### Enable Graphite
```
/git graphite on
```

**Action**:
```bash
jq '.skills.git.graphite.enabled = true' .claude/settings.json > tmp.json
mv tmp.json .claude/settings.json
```

**Output**:
```
✅ Graphite workflow enabled
```

### Disable Graphite
```
/git graphite off
```

**Action**:
```bash
jq '.skills.git.graphite.enabled = false' .claude/settings.json > tmp.json
mv tmp.json .claude/settings.json
```

**Output**:
```
❌ Graphite workflow disabled
```

### Check Status
```
/git graphite status
```

**Output**:
```
Graphite Status:
- CLI Installed: ✓ (version 1.4.2)
- Workflow Enabled: ✓
- Preference Level: repo-level (.claude/settings.json)
```

---

## Configuration Schema

```json
{
  "skills": {
    "git": {
      "graphite": {
        "enabled": true,
        "auto_stack": false,
        "detected_version": "1.4.2",
        "detected_date": "2025-12-23",
        "last_used": "2025-12-23"
      }
    }
  }
}
```

**Fields**:
- `enabled`: Whether Graphite workflow is active
- `auto_stack`: Automatically create stacked PRs (future feature)
- `detected_version`: Version of gt CLI detected
- `detected_date`: When first detected
- `last_used`: Last time workflow was used

---

## Integration Points

### With commit-commands Plugin

When `/commit-commands:commit-push-pr` runs:

```typescript
// Check preference before PR creation
const graphiteEnabled = await checkGraphitePreference()

if (graphiteEnabled && isGraphiteInstalled()) {
  // Use Graphite workflow
  await exec('gt stack submit')
} else {
  // Use standard GitHub workflow
  await exec('gh pr create ...')
}
```

### With Tech Lead

Before tech-lead review, detect Graphite:

```
Is this PR part of a stack?
├─ YES → Show stack context
└─ NO  → Standard review
```

---

## Troubleshooting

### "gt: command not found"

**Solution**:
```bash
# Install with npm
npm install -g @withgraphite/graphite-cli

# Or with Homebrew
brew install graphite
```

### Preference Not Persisting

**Solution**:
```bash
# Create directory
mkdir -p .claude

# Initialize settings
echo '{"skills":{"git":{"graphite":{"enabled":true}}}}' > .claude/settings.json
```

### Team Uses Graphite, But Disabled

**Solution**:
```bash
# Enable for team
/git graphite on

# Commit the change
git add .claude/settings.json
git commit -m "chore: enable Graphite workflow for team"
```

---

## Success Criteria

- [ ] Graphite CLI detection attempted
- [ ] Existing preference checked
- [ ] User prompted if needed (first time only)
- [ ] Preference saved to .claude/settings.json
- [ ] User informed of toggle commands

---

*Command Version: 1.0.0*
