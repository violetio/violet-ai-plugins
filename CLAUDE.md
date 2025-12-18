# Violet AI Plugins

> Marketplace of Claude Code plugins for Violet teams.

---

## What Is This Repo?

This repository contains reusable AI plugins for Claude Code. All plugins use the `v-` prefix for easy identification. Plugin types include:

- **Agents**: Professional personas with specific expertise (e.g., `v-architect`, `v-frontend`)
- **Workflows**: Sequential processes with phases and gates (e.g., `v-9d`, `v-convo`)
- **Skills**: Transferable techniques that apply anywhere (e.g., `v-security`, `v-testing`)
- **Domains**: Violet-specific product area knowledge (e.g., `v-prism`, `v-dashboard`)
- **Platforms**: Third-party API patterns (e.g., `v-shopify`, `v-bigcommerce`)
- **Stacks**: Technology-specific conventions (e.g., `v-typescript-react`, `v-nextjs`)

## Quick Start

### Using Plugins

Plugins are consumed by adding them to a project's `.claude/settings.json`:

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

### Refreshing Plugins

After plugin changes are pushed to this repo, run in Claude Code:
```
/plugin marketplace update violet
```

---

## Repository Structure

```
violet-ai-plugins/
├── .claude-plugin/
│   └── marketplace.json      # Plugin registry (MUST be updated for new plugins)
├── plugins/
│   └── v-*/                  # All plugins use v-* prefix
│       ├── .claude-plugin/
│       │   └── plugin.json   # Plugin metadata
│       ├── skills/           # Knowledge content (SKILL.md files)
│       └── commands/         # Slash commands (.md files)
├── CLAUDE.md                 # This file
├── README.md                 # Public documentation
└── CONTRIBUTING.md           # Contribution guidelines
```

---

## Plugin File Structure

Every plugin follows this structure:

```
plugins/v-{name}/
├── .claude-plugin/
│   └── plugin.json           # Metadata (required)
├── skills/
│   └── {skill-name}/
│       └── SKILL.md          # Knowledge with YAML frontmatter (optional)
└── commands/
    └── {command}.md          # Slash commands (optional)
```

### plugin.json Format

```json
{
  "name": "v-{name}",
  "version": "1.0.0",
  "description": "Brief description",
  "author": {
    "name": "Violet Platform Team",
    "email": "platform@violet.io"
  }
}
```

**Important**: Do NOT use the `knowledge` field - it's not supported by Claude Code.

### SKILL.md Format

```markdown
---
name: Skill Name
description: What this skill provides
---

# Skill Content

Your knowledge content here...
```

### Command Format

```markdown
---
description: What this command does
allowed-tools: ["Tool1", "Tool2"]  # Optional: restrict available tools
---

Command prompt content here...
```

**IMPORTANT: Bash Safety in Commands**

Commands can use `!`command`` to execute bash and inject output, but **avoid complex operations** that trigger safety checks:

- ❌ **Don't use**: pipes (`|`), redirects (`2>`), boolean operators (`||`, `&&`)
- ✅ **Do use**: Simple commands like `pwd`
- ✅ **Best practice**: Let the agent use tools (Read, Bash, Glob) instead of pre-executing

**Example - Unsafe** (triggers safety checks):
```markdown
## Context
- Settings: !`cat .claude/settings.json 2>/dev/null || echo "Not found"`

## Your Task
Do something with the settings...
```

**Example - Safe**:
```markdown
## Your Task
1. Read `.claude/settings.json` if it exists
2. Do something with the settings...
```

The agent will use the `Read` tool, which handles missing files gracefully.

---

## Updating Plugins

### Step-by-Step Guide

#### 1. Make Plugin Changes

Edit files in `plugins/v-{name}/`:
- Update `skills/{name}/SKILL.md` for knowledge changes
- Update `commands/{name}.md` for command changes
- Update `.claude-plugin/plugin.json` for metadata changes

#### 2. Update Marketplace Registry (if adding/renaming plugins)

Edit `.claude-plugin/marketplace.json`:

```json
{
  "plugins": [
    {
      "name": "v-newplugin",
      "source": "./plugins/v-newplugin",
      "description": "What this plugin does",
      "version": "1.0.0",
      "author": { "name": "Violet Platform Team" },
      "keywords": ["relevant", "keywords"]
    }
  ]
}
```

#### 3. Commit and Push

```bash
git add -A
git commit -m "feat: description of plugin changes"
git push
```

#### 4. Refresh in Claude Code

In any Claude Code session using these plugins:
```
/plugin marketplace update violet
```

### Creating a New Plugin

1. **Create directory structure**:
   ```bash
   mkdir -p plugins/v-{name}/.claude-plugin
   mkdir -p plugins/v-{name}/skills/{name}
   ```

2. **Create plugin.json**:
   ```json
   {
     "name": "v-{name}",
     "version": "1.0.0",
     "description": "What this plugin does",
     "author": {
       "name": "Violet Platform Team",
       "email": "platform@violet.io"
     }
   }
   ```

3. **Create SKILL.md**:
   ```markdown
   ---
   name: Plugin Name
   description: What knowledge this provides
   ---

   # Content

   Your knowledge content...
   ```

4. **Add to marketplace.json** (see step 2 above)

5. **Commit, push, and refresh**

### Adding Commands to a Plugin

1. **Create commands directory**:
   ```bash
   mkdir -p plugins/v-{name}/commands
   ```

2. **Create command file** `commands/{command-name}.md`:
   ```markdown
   ---
   description: What this command does
   ---

   Your command prompt here. This will be expanded when user runs /v-{name}:{command-name}
   ```

3. **Commit, push, and refresh**

Commands are invoked as: `/plugin-name:command-name`
Example: `/v-convo:start`, `/v-convo:exit`

---

## Agent Routing

| Task | Use Plugin |
|------|-----------|
| Create new plugin | `v-plugin-dev` |
| Test/validate plugin | `v-plugin-test` |
| Gather user feedback | `v-feedback` |
| Daily/weekly routines | `v-assistant` |

---

## Key Plugins

### Meta Plugins (for this repo)

| Plugin | Purpose |
|--------|---------|
| `v-plugin-dev` | Create plugins following patterns |
| `v-plugin-test` | Validate plugins before deployment |
| `v-plugins` | Manage plugins in consuming projects |
| `v-feedback` | Gather and act on feedback |

### Conversation Management

| Plugin | Commands | Purpose |
|--------|----------|---------|
| `v-convo` | `/v-convo:start` | Understanding + alignment at conversation start |
| `v-convo` | `/v-convo:capture` | Mid-conversation context save |
| `v-convo` | `/v-convo:exit` | Debrief + improvements at end |

### Personal Productivity

| Plugin | Purpose |
|--------|---------|
| `v-assistant` | Daily routines, task management |

---

## Plugin Management

Use the `v-plugins` plugin to manage plugins in your projects.

### Management Commands

| Command | Purpose |
|---------|---------|
| `/v-plugins-list` | List all enabled plugins in the current project |
| `/v-plugins-status` | Compare local plugins against marketplace, identify issues |
| `/v-plugins-update` | Fix outdated plugin names in settings.json |
| `/v-plugins-commands` | Show available v-plugins commands |

### Naming Convention

All plugins use `v-*` naming. Old naming conventions are deprecated:

| Old Pattern | New Pattern |
|-------------|-------------|
| `agent-*@violet` | `v-*@violet` |
| `workflow-*@violet` | `v-*@violet` |
| `domain-*@violet` | `v-*@violet` |
| `platform-*@violet` | `v-*@violet` |
| `skill-*@violet` | `v-*@violet` |
| `stack-*@violet` | `v-*@violet` |

### Quick Fix

If your plugins aren't working, run:
```
/v-plugins-status
```

Then fix issues with:
```
/v-plugins-update
```

---

## Quality Checklist

Before merging plugin changes:

- [ ] Directory follows `plugins/v-{name}/` pattern
- [ ] `plugin.json` has all required fields (name, version, description, author)
- [ ] `plugin.json` does NOT have `knowledge` field
- [ ] Name in plugin.json matches directory name
- [ ] Skills use SKILL.md with YAML frontmatter
- [ ] Commands use .md files with YAML frontmatter
- [ ] Marketplace.json updated with new plugin
- [ ] README.md updated if adding new plugins

---

## Common Issues

### Plugin Not Recognized

1. **Check marketplace.json**: Plugin must be listed in `.claude-plugin/marketplace.json`
2. **Check structure**: Must use `skills/` directory, not `knowledge/`
3. **Refresh marketplace**: Run `/plugin marketplace update violet`

### Command Not Working

1. **Check command file**: Must be in `commands/{name}.md` with YAML frontmatter
2. **Check invocation**: Use format `/plugin-name:command-name`
3. **Refresh marketplace**: Run `/plugin marketplace update violet`

### Changes Not Appearing

1. **Push to remote**: Changes must be on GitHub
2. **Refresh marketplace**: Run `/plugin marketplace update violet`

---

## Related Repos

| Repo | Purpose |
|------|---------|
| `violet-brain` | Central patterns, templates, coordination |
| `beam-brain` | Beam product context |
| `prism-brain` | Prism product context |

Plugins provide horizontal (generic) knowledge; brain repos provide vertical (product-specific) depth.
