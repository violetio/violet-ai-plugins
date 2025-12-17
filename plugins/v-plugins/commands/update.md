---
description: Update settings.json to fix outdated plugin names
---

## Context

- Current working directory: !`pwd`
- Local settings: !`cat .claude/settings.json 2>/dev/null || echo "No .claude/settings.json found"`

## Your Task

Update the plugin configuration to use correct names:

1. **Read `.claude/settings.json`** in the current project
2. **Apply name mappings** for any outdated plugin names:

| Old Name | New Name |
|----------|----------|
| `workflow-convo-*@violet` | `v-convo@violet` |
| `workflow-9d-framework@violet` | `v-9d@violet` |
| `workflow-git-conventions@violet` | `v-git@violet` |
| `workflow-product-discovery@violet` | `v-discovery@violet` |
| `workflow-review-gates@violet` | `v-gates@violet` |
| `agent-architect@violet` | `v-architect@violet` |
| `agent-backend-engineer@violet` | `v-backend@violet` |
| `agent-frontend-engineer@violet` | `v-frontend@violet` |
| `agent-tech-lead@violet` | `v-tech-lead@violet` |
| `agent-qa-engineer@violet` | `v-qa@violet` |
| `agent-commerce-pm@violet` | `v-commerce-pm@violet` |
| `agent-platform-pm@violet` | `v-platform-pm@violet` |
| `agent-data-engineer@violet` | `v-data-engineer@violet` |
| `agent-infrastructure-engineer@violet` | `v-infrastructure@violet` |
| `agent-experience-designer@violet` | `v-designer@violet` |
| `agent-assistant@violet` | `v-assistant@violet` |
| `agent-plugin-developer@violet` | `v-plugin-dev@violet` |
| `stack-java-spring@violet` | `v-java-spring@violet` |
| `stack-typescript-react@violet` | `v-typescript-react@violet` |
| `stack-nextjs@violet` | `v-nextjs@violet` |
| `stack-python@violet` | `v-python@violet` |
| `domain-prism@violet` | `v-prism@violet` |
| `domain-beam@violet` | `v-beam@violet` |
| `domain-violet-dashboard@violet` | `v-dashboard@violet` |
| `domain-relay@violet` | `v-relay@violet` |
| `domain-ecom-integrations@violet` | `v-ecom-integrations@violet` |
| `domain-open-commerce@violet` | `v-open-commerce@violet` |
| `platform-shopify@violet` | `v-shopify@violet` |
| `platform-bigcommerce@violet` | `v-bigcommerce@violet` |
| `platform-woocommerce@violet` | `v-woocommerce@violet` |
| `platform-rithum@violet` | `v-rithum@violet` |
| `platform-squarespace@violet` | `v-squarespace@violet` |
| `skill-security@violet` | `v-security@violet` |
| `skill-testing@violet` | `v-testing@violet` |
| `skill-observability@violet` | `v-observability@violet` |
| `skill-documentation@violet` | `v-documentation@violet` |

3. **Consolidate duplicates**: If both old and new names exist, keep only the new name
4. **Generate updated settings.json**
5. **Show diff** of changes
6. **Ask user to confirm** before writing

## Output Format

```markdown
# Plugin Update

## Changes to Apply

| Action | Plugin |
|--------|--------|
| Rename | `agent-backend-engineer@violet` → `v-backend@violet` |
| Remove | `workflow-convo-start@violet` (consolidated into v-convo) |

## Updated settings.json

```json
{
  "enabledPlugins": {
    "v-backend@violet": true,
    ...
  }
}
```

**Apply these changes?** (y/n)
```
