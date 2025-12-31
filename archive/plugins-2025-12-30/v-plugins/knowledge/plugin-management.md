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

## Repository Context and Plugin Selection

### Brain Repos vs Code Repos

Violet uses two types of repositories with different plugin needs:

#### Brain Repositories

**Purpose**: Planning, specs, requirements, architecture, strategy, cross-product coordination

**Examples**: `violet-brain`, `prism-brain`, `beam-brain`

**Plugin Count**: 25-40 plugins

**Should Include**:
- Planning agents: `v-architect`, `v-tech-lead`, `v-commerce-pm`, `v-platform-pm`
- Workflows: `v-9d`, `v-discovery`, `v-gates`, `v-convo`, `v-git`
- All relevant domains (for strategic context)
- All relevant platforms (for integration strategy)
- Core skills: `v-security`, `v-testing`, `v-observability`, `v-documentation`
- Implementation agents (optional): `v-backend`, `v-frontend`, `v-qa`
- Tools: `v-plugins`, `v-feedback`

#### Code Repositories

**Purpose**: Implementation from specs, bug fixes, testing

**Examples**: `VioletDashboard`, `BeamService`, `PrismService`

**Plugin Count**: 8-12 plugins (maximum)

**Should Include**:
- ONE implementation agent: `v-backend` OR `v-frontend` (not both)
- ONE stack: `v-java-spring`, `v-typescript-react`, `v-nextjs`, or `v-python`
- ONE domain: The specific domain this service implements
- Core skills: `v-security`, `v-testing`, `v-observability`, `v-documentation`
- Essential workflows: `v-git`, `v-convo`
- Tools: `v-plugins`

**Should NOT Include**:
- Planning agents (`v-architect`, `v-tech-lead`, `v-*-pm`)
- Product workflows (`v-9d`, `v-discovery`, `v-gates`)
- Multiple domain plugins (strategy belongs in brain repo)
- Business agents (`v-customer-success`, `v-support`, `v-partnerships`)

### Why This Matters

Starting planning work in a code repo when you should start in a brain repo means missing:
- Strategic context and product roadmap
- Existing specs and architecture decisions
- Planning agent expertise (architect, tech lead, product managers)
- Cross-product coordination and impact analysis

### Check Your Repository Context

Use `/v-plugins-context` to verify you're working in the right repository for your task:

```
/v-plugins-context
```

This command will:
- Detect if you're in a brain repo or code repo
- Analyze your conversation context
- Warn if you're doing planning work in a code repo
- Suggest switching to the appropriate repo if needed

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
