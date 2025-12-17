# Plugin Management

## Overview

The `v-plugins` plugin provides commands to manage violet-ai-plugins in your projects. It helps you:

- List enabled plugins
- Check for outdated or missing plugins
- Update plugin names to the current convention

## Plugin Naming Convention

All plugins use the `v-*` naming convention:

| Category | Example |
|----------|---------|
| Agents | `v-architect`, `v-backend`, `v-frontend` |
| Workflows | `v-9d`, `v-convo`, `v-git`, `v-discovery` |
| Domains | `v-prism`, `v-beam`, `v-dashboard` |
| Platforms | `v-shopify`, `v-bigcommerce`, `v-woocommerce` |
| Skills | `v-security`, `v-testing`, `v-observability` |
| Tools | `v-plugins`, `v-plugin-dev`, `v-plugin-test` |

## Configuration

Plugins are configured in `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "v-backend@violet": true,
    "v-convo@violet": true,
    "v-plugins@violet": true
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

## Common Issues

### Old Plugin Names

If you see plugins like `agent-backend-engineer@violet` or `workflow-convo-exit@violet`, these use the old naming convention and won't work. Run `/v-plugins-update` to fix them.

### Missing Marketplace

If plugins aren't loading, ensure `extraKnownMarketplaces` is configured with the violet-ai-plugins repo.

### Deprecated Plugins

Some plugins have been consolidated:
- `workflow-convo-start`, `workflow-convo-capture`, `workflow-convo-exit` → `v-convo`
- `v-conversation-legacy` → `v-convo`

## Available Commands

| Command | Purpose |
|---------|---------|
| `/v-plugins-list` | List all enabled plugins |
| `/v-plugins-status` | Check for issues |
| `/v-plugins-update` | Fix outdated names |
| `/v-plugins-commands` | Show help |
