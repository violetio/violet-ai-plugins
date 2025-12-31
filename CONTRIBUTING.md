# Contributing to Violet AI Plugins

This guide covers how to contribute to the unified `v` plugin for Claude Code.

---

## Plugin Structure

The `v` plugin lives in `plugins/v/` with this structure:

```
plugins/v/
├── .claude-plugin/
│   └── plugin.json       # Plugin metadata and version
├── skills/               # Agent skills (SKILL.md files)
│   ├── requirements-analyst/
│   │   └── SKILL.md
│   ├── experience-designer/
│   │   └── SKILL.md
│   └── ...
└── commands/             # Slash commands (.md files)
    ├── define.md
    ├── help.md
    └── ...
```

---

## Adding Skills

Skills provide specialized knowledge and capabilities to Claude.

### 1. Create the skill directory

```bash
mkdir -p plugins/v/skills/{skill-name}
```

### 2. Create SKILL.md with YAML frontmatter

```markdown
---
name: skill-name
description: Brief description of what this skill provides
---

# Skill Name

Detailed skill content here...
```

**Required frontmatter:**
- `name`: Kebab-case identifier (must match directory name)
- `description`: One-line summary

### 3. Follow the release process (below)

---

## Adding Commands

Commands are slash commands users can invoke (e.g., `/v:define`).

### 1. Create the command file

```bash
touch plugins/v/commands/{command-name}.md
```

### 2. Add content with frontmatter

```markdown
---
description: What this command does
allowed-tools: ["Tool1", "Tool2"]  # Optional: restrict available tools
---

Command prompt content here...
```

**Required frontmatter:**
- `description`: Shown in command listings

**Optional frontmatter:**
- `allowed-tools`: Array of tool names to restrict access

### 3. Follow the release process (below)

---

## Versioning

All plugins follow semantic versioning starting at `0.0.1`:

| Level | When to Increment | Example |
|-------|-------------------|---------|
| **Patch (0.0.X)** | Bug fixes, minor improvements, small additions | 0.0.1 → 0.0.2 |
| **Minor (0.X.0)** | Big updates, new features, significant changes | 0.0.5 → 0.1.0 |
| **Major (X.0.0)** | Massive reworks, breaking changes, architectural shifts | 0.3.0 → 1.0.0 |

**Guidelines:**
- Most changes increment **patch** (third number)
- Only increment **minor** for substantial new functionality
- Reserve **major** for complete redesigns or breaking changes

---

## Release Process

When making changes to the plugin:

### 1. Update CHANGELOG.md

Add a new entry at the top of the changelog (or update the Unreleased section):

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- New feature description (commit_hash)

### Changed
- Change description (commit_hash)

### Fixed
- Bug fix description (commit_hash)

### Removed
- Removed feature description (commit_hash)
```

### 2. Bump version in plugin.json

Edit `plugins/v/.claude-plugin/plugin.json`:

```json
{
  "name": "v",
  "version": "0.0.4",  // <-- Increment this
  ...
}
```

### 3. Sync version in marketplace.json

Edit `.claude-plugin/marketplace.json` to match:

```json
{
  "plugins": [
    {
      "name": "v",
      "version": "0.0.4",  // <-- Must match plugin.json
      ...
    }
  ]
}
```

**Important:** These versions must always match!

### 4. Commit with conventional format

```bash
git add .
git commit -m "feat(v): description of changes"
# or
git commit -m "fix(v): description of bug fix"
```

### 5. Push and merge

After merge, consumers refresh with:
```
/plugin marketplace update violet
```

---

## Changelog Guidelines

We follow [Keep a Changelog](https://keepachangelog.com/) format.

### Categories

- **Added** - New features or capabilities
- **Changed** - Changes to existing functionality
- **Fixed** - Bug fixes
- **Removed** - Removed features
- **Deprecated** - Features marked for future removal
- **Security** - Security-related changes

### Writing Good Entries

**Do:**
- Start with a verb (Add, Fix, Update, Remove)
- Include commit hash in parentheses: `(abc1234)`
- Be specific about what changed
- Group related changes together

**Don't:**
- Use vague descriptions ("Various fixes")
- Include internal implementation details users don't care about
- Forget the commit hash reference

### Example Entry

```markdown
## [0.0.4] - 2025-12-31

### Added
- `/v:help` command for user documentation (bfddd31)
- Pattern-extraction skill for documentation workflows (8539a3e)

### Fixed
- Product descriptions in domain selection (96932d5)
```

---

## Review Process

1. PRs require approval from @violetio/eng-leads
2. Changes to `.claude-plugin/` files require additional review
3. New skills or commands should include:
   - Clear description in frontmatter
   - Example usage (for commands)

---

## Questions?

Contact the platform team or post in #claude-code-plugins.
