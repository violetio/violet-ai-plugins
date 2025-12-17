---
description: Compare local plugins against marketplace and identify issues
---

## Context

- Current working directory: !`pwd`
- Local settings: !`cat .claude/settings.json 2>/dev/null || echo "No .claude/settings.json found"`
- Marketplace plugins: !`cat ../violet-ai-plugins/.claude-plugin/marketplace.json 2>/dev/null | grep '"name":' | head -60 || echo "Marketplace not found"`

## Your Task

Analyze the plugin configuration and identify issues:

1. **Read `.claude/settings.json`** in the current project
2. **Compare against marketplace.json** in violet-ai-plugins
3. **Identify**:
   - ❌ **Missing**: Plugins in settings.json that don't exist in marketplace
   - ⚠️ **Outdated**: Plugins using old naming convention (not `v-*`)
   - 📦 **Available**: Plugins in marketplace not enabled locally
   - ✅ **OK**: Plugins correctly configured

## Output Format

```markdown
# Plugin Status

## Issues Found

### ❌ Missing (not in marketplace)
| Plugin in settings.json | Likely replacement |
|------------------------|-------------------|
| `workflow-convo-exit@violet` | `v-convo@violet` |

### ⚠️ Outdated naming
| Old Name | New Name |
|----------|----------|
| `agent-backend-engineer@violet` | `v-backend@violet` |

## Available (not enabled)
These plugins exist but aren't enabled in your project:
- `v-feedback` - Gather, organize, and act on user feedback
- `v-contracts` - Contract review and legal analysis

## Summary
- Enabled: X plugins
- Issues: X (run `/v-plugins-update` to fix)
- Available: X additional plugins

## Recommended Action
Run `/v-plugins-update` to fix the X issues found.
```
