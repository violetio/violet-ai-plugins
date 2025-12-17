---
description: List all enabled plugins in the current project
---

## Context

- Current working directory: !`pwd`
- Local settings: !`cat .claude/settings.json 2>/dev/null || echo "No .claude/settings.json found"`
- Marketplace: !`cat ../violet-ai-plugins/.claude-plugin/marketplace.json 2>/dev/null | head -50 || echo "Marketplace not found at ../violet-ai-plugins"`

## Your Task

List all plugins enabled in this project:

1. **Read `.claude/settings.json`** in the current project
2. **For each enabled plugin**, show:
   - Plugin name
   - Description (from marketplace.json if available)
   - Status: ✅ Active, ⚠️ Deprecated, ❌ Not found in marketplace
3. **Group by category**:
   - Agents (v-architect, v-backend, etc.)
   - Workflows (v-9d, v-convo, v-git, etc.)
   - Domains (v-prism, v-beam, v-dashboard, etc.)
   - Platforms (v-shopify, v-bigcommerce, etc.)
   - Skills (v-security, v-testing, etc.)
   - Tools (v-plugins, etc.)

## Output Format

```markdown
# Enabled Plugins

## Agents (X enabled)
| Plugin | Description | Status |
|--------|-------------|--------|
| v-architect | System architecture and technical design | ✅ Active |

## Workflows (X enabled)
...

## Summary
- Total: X plugins enabled
- Active: X
- Issues: X (deprecated or missing)
```
