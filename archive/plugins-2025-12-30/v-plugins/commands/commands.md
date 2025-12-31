---
description: Show available v-plugins commands
---

## Your Task

Display the available v-plugins commands:

```markdown
# v-plugins Commands

Plugin management commands for violet-ai-plugins.

## Available Commands

| Command | Description |
|---------|-------------|
| `/v-plugins-list` | List all enabled plugins in the current project |
| `/v-plugins-status` | Compare local plugins against marketplace, identify issues |
| `/v-plugins-update` | Fix outdated plugin names in settings.json |
| `/v-plugins-commands` | Show this help message |

## Quick Start

1. **Check your status**: `/v-plugins-status`
2. **Fix any issues**: `/v-plugins-update`
3. **See what's enabled**: `/v-plugins-list`

## Plugin Naming Convention

All plugins use `v-*` naming:
- `v-backend@violet` (not `agent-backend-engineer@violet`)
- `v-convo@violet` (not `workflow-convo-exit@violet`)
- `v-shopify@violet` (not `platform-shopify@violet`)

## Adding Plugins

Edit `.claude/settings.json` in your project:

```json
{
  "enabledPlugins": {
    "v-backend@violet": true,
    "v-convo@violet": true
  },
  "extraKnownMarketplaces": {
    "violet": {
      "source": {
        "source": "github",
        "repo": "violetio/violet-ai-plugins"
      }
    }
  }
}
```

## Feedback

To suggest improvements to plugins, create an issue at:
https://github.com/violetio/violet-ai-plugins/issues
```
