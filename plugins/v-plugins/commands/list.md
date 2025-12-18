---
description: List all enabled plugins in the current project
---

## Your Task

List all plugins enabled in this project:

1. **Read `.claude/settings.json`** in the current project
2. **Read marketplace.json** from `../violet-ai-plugins/.claude-plugin/marketplace.json` if available
3. **For each enabled plugin**, show:
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
