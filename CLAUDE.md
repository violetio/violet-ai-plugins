# Violet AI Plugins

> Single unified Claude Code plugin for Violet product development.

---

## What Is This Repo?

This repository contains the `v` plugin for Claude Code - Violet's unified product development workflow. The plugin provides agents, skills, and commands for the complete product lifecycle.

## Quick Start

### Using the Plugin

Add to your project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "v@violet": true
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

### Refreshing After Updates

After plugin changes are pushed to this repo:
```
/plugin marketplace update violet
```

---

## Contributing & Releases

For detailed guidance on:
- **Adding skills or commands** → See [CONTRIBUTING.md](./CONTRIBUTING.md#adding-skills)
- **Versioning convention** → See [CONTRIBUTING.md](./CONTRIBUTING.md#versioning)
- **Releasing new versions** → See [CONTRIBUTING.md](./CONTRIBUTING.md#release-process)
- **Change history** → See [CHANGELOG.md](./CHANGELOG.md)

**Quick reference:**
- Increment patch (0.0.X) for most changes
- Update both `plugin.json` and `marketplace.json` versions (must match!)
- Update CHANGELOG.md with your changes

---

## Repository Structure

```
violet-ai-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Plugin registry
├── plugins/
│   └── v/                    # The unified v plugin
│       ├── .claude-plugin/
│       │   └── plugin.json   # Plugin metadata
│       ├── skills/           # Agent skills (SKILL.md files)
│       │   ├── requirements-analyst/
│       │   ├── experience-designer/
│       │   └── ...
│       └── commands/         # Slash commands (.md files)
│           ├── define.md
│           └── ...
├── archive/                  # Archived old plugins
├── CLAUDE.md                 # This file
└── README.md                 # Public documentation
```

---

## Plugin Structure

### plugin.json Format

```json
{
  "name": "v",
  "version": "0.0.1",
  "description": "Violet product development workflow",
  "author": {
    "name": "Violet Platform Team",
    "email": "platform@violet.io"
  }
}
```

### SKILL.md Format

```markdown
---
name: skill-name
description: What this skill provides
---

# Skill Name

Skill content here...
```

### Command Format

```markdown
---
description: What this command does
allowed-tools: ["Tool1", "Tool2"]  # Optional
---

Command prompt content here...
```

Commands are invoked as: `/v:command-name`

---

## Product Development Phases

The v plugin supports Violet's product development workflow:

| Phase | Purpose | Key Agents |
|-------|---------|------------|
| **Discovery** | Understand the problem | (coming soon) |
| **Definition** | Define requirements & experience | requirements-analyst, experience-designer |
| **Design** | Technical architecture | (coming soon) |
| **Development** | Implementation | (coming soon) |
| **Review** | Quality gates | (coming soon) |

### Definition Phase Commands

| Command | Purpose |
|---------|---------|
| `/v:define` | Run the Definition phase |

---

## Quality Checklist

Before merging changes:

- [ ] Version incremented in both `plugin.json` and `marketplace.json`
- [ ] CHANGELOG.md updated with changes
- [ ] Skills have YAML frontmatter (name, description)
- [ ] Commands have YAML frontmatter (description)

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines.

---

## Archived Plugins

Previous separate plugins (v-architect, v-frontend, etc.) have been archived in `archive/plugins-{date}/`. The unified `v` plugin consolidates all functionality.

---

## Related Repos

| Repo | Purpose |
|------|---------|
| `violet-brain` | Central specifications and coordination |
| `beam-brain` | Beam product context |
| `prism-brain` | Prism product context |
