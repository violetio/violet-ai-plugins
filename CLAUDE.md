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

## Versioning Convention

**All plugins start at version 0.0.1** and follow this increment pattern:

| Level | When to Increment | Example |
|-------|-------------------|---------|
| **Patch (0.0.X)** | Bug fixes, minor improvements, small additions | 0.0.1 → 0.0.2 |
| **Minor (0.X.0)** | Big updates, new features, significant changes | 0.0.5 → 0.1.0 |
| **Major (X.0.0)** | Massive reworks, breaking changes, architectural shifts | 0.3.0 → 1.0.0 |

**Guidelines:**
- Most changes increment the **patch** level (third number)
- Only increment **minor** for substantial new functionality
- Reserve **major** for complete redesigns or breaking changes
- This keeps versions meaningful and avoids confusion from rapid major version jumps

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

## Adding New Skills

1. Create skill directory:
   ```bash
   mkdir -p plugins/v/skills/{skill-name}
   ```

2. Create SKILL.md:
   ```markdown
   ---
   name: skill-name
   description: What this skill does
   ---

   # Skill Name

   Content...
   ```

3. Increment version in `plugins/v/.claude-plugin/plugin.json`

4. Update marketplace.json if needed

5. Commit, push, and refresh

---

## Adding New Commands

1. Create command file:
   ```bash
   touch plugins/v/commands/{command-name}.md
   ```

2. Add content with frontmatter:
   ```markdown
   ---
   description: What this command does
   ---

   Command prompt...
   ```

3. Increment version in `plugins/v/.claude-plugin/plugin.json`

4. Commit, push, and refresh

---

## Quality Checklist

Before merging changes:

- [ ] Version incremented appropriately (usually patch level)
- [ ] Skills have YAML frontmatter with name and description
- [ ] Commands have YAML frontmatter with description
- [ ] marketplace.json updated if adding new plugins
- [ ] No `knowledge` field in plugin.json (not supported)

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
