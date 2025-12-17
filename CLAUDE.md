# Violet AI Plugins

> Marketplace of Claude Code plugins for Violet teams.

---

## What Is This Repo?

This repository contains reusable AI plugins for Claude Code. Plugins provide:
- **Agents**: Professional personas with specific expertise
- **Workflows**: Sequential processes with phases and gates
- **Skills**: Transferable techniques that apply anywhere
- **Domains**: Violet-specific product area knowledge
- **Platforms**: Third-party API patterns
- **Stacks**: Technology-specific conventions

## Quick Start

### Using Plugins

Plugins are consumed by adding them to a project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "agent-backend-engineer@violet": true,
    "workflow-convo-exit@violet": true
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

### Creating Plugins

Use the plugin developer agent:
```
I need to create a new plugin for [purpose]
```

Or use the `/new-plugin` command.

---

## Repository Structure

```
violet-ai-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Plugin registry
├── plugins/
│   ├── agent-*/              # Agent plugins (26)
│   ├── workflow-*/           # Workflow plugins (7)
│   ├── stack-*/              # Stack plugins (4)
│   ├── domain-*/             # Domain plugins (6)
│   ├── platform-*/           # Platform plugins (5)
│   └── skill-*/              # Skill plugins (4)
├── CLAUDE.md                 # This file
├── README.md                 # Public documentation
└── CONTRIBUTING.md           # Contribution guidelines
```

---

## Working in This Repo

### When to Work Here

- Creating new plugins
- Improving existing plugins
- Testing plugin quality
- Updating marketplace registry

### Plugin Categories

| Category | Key Test | Count |
|----------|----------|-------|
| **Agents** | "You ARE a [role]" | 26 |
| **Workflows** | Sequential steps/phases | 7 |
| **Stacks** | Bound to specific technology | 4 |
| **Domains** | About a Violet product area | 6 |
| **Platforms** | About an external system | 5 |
| **Skills** | Transferable technique | 4 |

### Category Decision Tree

1. Does it define WHO Claude should be? → **Agent**
2. Is it bound to a specific technology? → **Stack**
3. Is it about a Violet product area? → **Domain**
4. Is it about an external system? → **Platform**
5. Does it have sequential steps/phases? → **Workflow**
6. Is it transferable technique knowledge? → **Skill**

---

## Agent Routing

| Task | Use Agent |
|------|-----------|
| Create new plugin | `agent-plugin-developer` |
| Test/validate plugin | `agent-plugin-tester` |
| Gather user feedback | `agent-feedback-collector` |
| Daily/weekly routines | `agent-assistant` |

---

## Key Plugins

### Meta Plugins (for this repo)

| Plugin | Purpose |
|--------|---------|
| `agent-plugin-developer` | Create plugins following patterns |
| `agent-plugin-tester` | Validate plugins before deployment |
| `agent-feedback-collector` | Gather and act on feedback |

### Personal Productivity

| Plugin | Purpose |
|--------|---------|
| `agent-assistant` | Daily routines, task management |
| `workflow-convo-start` | Understanding + alignment |
| `workflow-convo-capture` | Mid-conversation context save |
| `workflow-convo-exit` | Debrief + improvements |

---

## Commands

| Command | Purpose |
|---------|---------|
| `/new-plugin` | Create a new plugin with guided flow |
| `/test-plugin` | Validate a plugin before deployment |
| `/day-start` | Morning routine: Linear tasks, priorities, save to coordination/ |
| `/day-end` | Evening routine: accomplishments, progress, save to coordination/ |
| `/week-start` | Weekly planning: Linear overview, week plan, save to coordination/weeks/ |
| `/week-end` | Weekly review: summary, learnings, save to coordination/weeks/ |

### Command Outputs

Commands save their outputs for continuity:

| Output | Location |
|--------|----------|
| Day starts | `coordination/day-starts/YYYY-MM-DD.md` |
| Day ends | `coordination/day-ends/YYYY-MM-DD.md` |
| Week plans | `coordination/weeks/YYYY-WXX.md` |
| Activity log | `personal/activity-log.md` |

### Linear Integration

Day/week commands use Linear MCP tools to:
- Get issues assigned to you
- Check project status
- Review initiative/cycle deadlines
- Track progress over time

---

## Plugin File Structure

Every plugin follows this structure:

```
plugins/{category}-{name}/
├── .claude-plugin/
│   └── plugin.json           # Metadata
└── knowledge/
    └── {topic}.md            # Knowledge content
```

### plugin.json Format

```json
{
  "name": "{category}-{name}",
  "version": "1.0.0",
  "description": "Brief description",
  "author": {
    "name": "Violet Platform Team",
    "email": "platform@violet.io"
  },
  "knowledge": ["./knowledge/"]
}
```

---

## Quality Checklist

Before merging plugin changes:

- [ ] Directory follows `plugins/{category}-{name}/` pattern
- [ ] `plugin.json` has all required fields
- [ ] Name in plugin.json matches directory name
- [ ] Knowledge files follow category patterns
- [ ] Marketplace.json updated with new plugin
- [ ] README.md updated with counts and table

---

## Publishing Workflow

1. Create plugin in `plugins/` directory
2. Add entry to `.claude-plugin/marketplace.json`
3. Update `README.md` counts and tables
4. Commit and push to main
5. Plugins are immediately available via marketplace

---

## Related Repos

| Repo | Purpose |
|------|---------|
| `violet-brain` | Central patterns, templates, coordination |
| `beam-brain` | Beam product context |
| `prism-brain` | Prism product context |

Plugins provide horizontal (generic) knowledge; brain repos provide vertical (product-specific) depth.
