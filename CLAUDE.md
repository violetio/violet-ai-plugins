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

## Brain Repos vs Code Repos: Where to Start Work

### The Problem

Violet has two types of repositories with different purposes:

1. **Brain Repos**: Strategic planning, specifications, architecture decisions, and cross-product coordination
2. **Code Repos**: Implementation of specifications, bug fixes, and testing

Starting work in a code repo when you should start in a brain repo means you'll miss critical context:
- Strategic context and product roadmap
- Requirements and specifications
- Architecture decisions and trade-offs
- Planning agents (v-architect, v-tech-lead, v-commerce-pm, v-platform-pm)
- Cross-product coordination and design reviews

**Result**: Isolated implementation, violated architecture, duplicated work.

### Decision Tree: Where Should I Start?

```
START: You have a task or idea

┌─────────────────────────────────────────┐
│ Is this NEW work that doesn't exist yet?│
└─────────────┬───────────────────────────┘
              │
      ┌───────┴────────┐
      │ YES            │ NO
      │                │
      ▼                ▼
┌──────────────┐  ┌──────────────────────────┐
│ START IN     │  │ Is there an existing spec│
│ BRAIN REPO   │  │ or ADR for this work?    │
└──────────────┘  └──────┬───────────────────┘
                         │
                 ┌───────┴────────┐
                 │ NO             │ YES
                 │                │
                 ▼                ▼
        ┌─────────────────┐  ┌──────────────────────┐
        │ START IN         │  │ Are you implementing │
        │ BRAIN REPO       │  │ from the spec?       │
        │ (create spec)    │  └──────┬───────────────┘
        └──────────────────┘         │
                                ┌────┴─────┐
                                │ YES      │ NO
                                │          │
                                ▼          ▼
                        ┌────────────┐  ┌─────────────┐
                        │ CODE REPO  │  │ BRAIN REPO  │
                        │ (implement)│  │ (refine)    │
                        └────────────┘  └─────────────┘
```

**Quick Questions to Ask:**

1. Does this require understanding "why" we're building it? → **BRAIN REPO**
2. Does this involve trade-off decisions? → **BRAIN REPO**
3. Does this need product manager input? → **BRAIN REPO**
4. Does this need architect approval? → **BRAIN REPO**
5. Do you have a complete spec to implement? → **CODE REPO** (only if YES)
6. Are you fixing a bug in existing code? → **CODE REPO**
7. Are you adding tests to existing code? → **CODE REPO**
8. Are you refactoring without changing behavior? → **CODE REPO**

### Plugin Distribution: Brain Repos vs Code Repos

**Brain Repos Should Have** (25-40 plugins):
- Planning agents: v-architect, v-tech-lead, v-commerce-pm, v-platform-pm
- Workflows: v-9d, v-discovery, v-gates
- ALL relevant domain plugins (for strategic context)
- ALL relevant platform plugins (for integration strategy)
- Core skills: v-security, v-testing, v-observability, v-documentation
- Implementation agents (optional): v-backend, v-frontend, v-qa
- Tools: v-plugins, v-convo, v-git

**Code Repos Should Have** (8-12 plugins maximum):
- ONE implementation agent: v-backend OR v-frontend (not both)
- ONE stack plugin: v-java-spring, v-typescript-react, v-nextjs, or v-python
- ONE domain plugin: The specific domain this repository implements
- Core skills: v-security, v-testing, v-observability, v-documentation
- Essential workflows: v-git, v-convo
- Tools: v-plugins

**Code Repos Should NOT Have**:
- Planning agents (v-architect, v-tech-lead, v-*-pm)
- Product development workflows (v-9d, v-discovery, v-gates)
- Multiple domain plugins (strategy belongs in brain repo)
- Business agents (v-customer-success, v-support, v-partnerships, etc.)

### Repository Examples

| Repo Type | Example | Expected Plugin Count | Key Agents |
|-----------|---------|----------------------|-----------|
| Brain Repo | violet-brain | 37-40 | v-architect, v-tech-lead, all product PMs |
| Brain Repo | prism-brain | 25-30 | v-architect, v-tech-lead, v-commerce-pm |
| Brain Repo | beam-brain | 25-30 | v-architect, v-tech-lead, v-commerce-pm |
| Code Repo | VioletDashboard | 8-10 | v-frontend only |
| Code Repo | BeamService | 8-10 | v-backend only |

### Using the Context Checker

Run `/v-plugins-context` to verify you're working in the right repository type for your task:

```
/v-plugins-context
```

This command will:
- Detect if you're in a brain repo or code repo
- Analyze your conversation context
- Warn if doing planning work in a code repo
- Suggest switching repos if needed

---

## Model Selection Strategy

Claude Code supports multiple models with different capabilities and costs. **Always select the appropriate model for the task at hand.**

See comprehensive guide: [patterns/model-selection.md](patterns/model-selection.md)

### Quick Reference

| Model | Speed | Cost | Best For |
|-------|-------|------|----------|
| **Sonnet 4.5** | Medium | Medium | Complex reasoning, architectural decisions, debugging |
| **Haiku** | Fast | Low | Simple tasks, pattern matching, established workflows |
| **Opus 4.5** | Slow | High | Critical decisions, novel problems, high-stakes work |

### Decision Tree

```
Is this a critical security/architecture decision?
└─ YES → Opus 4.5
└─ NO ↓

Is this following an established pattern exactly?
└─ YES → Haiku
└─ NO ↓

Does this require understanding context/trade-offs?
└─ YES → Sonnet 4.5
└─ NO → Haiku
```

### Agent-Specific Recommendations

| Agent | Sonnet Use Cases | Haiku Use Cases |
|-------|------------------|-----------------|
| **Tech Lead** | PR analysis, coordination, evaluation | Status updates, routing |
| **Frontend** | Architectural changes, pattern violations | console.log removal, following patterns |
| **Backend** | Complex logic, first-time implementations | CRUD operations, established patterns |
| **QA** | Test strategy, complex scenarios | Implementing tests from patterns |
| **Architect** | Always use Sonnet (Opus for critical decisions) | N/A |
| **PM** | Discovery, requirements, complex decisions | Status updates, routine tasks |

### When to Use Each Model

**Use Sonnet 4.5 When:**
- Architectural violations need fixing
- Pattern compliance (first-time learning)
- Complex refactoring or debugging
- Understanding nuanced feedback
- Designing test strategy
- Making decisions with trade-offs

**Use Haiku When:**
- Simple fixes (typo, console.log removal)
- Following established patterns exactly
- Straightforward implementations with clear examples
- Running test suites
- Simple documentation updates

**Use Opus 4.5 When:**
- Security-sensitive decisions
- Complex multi-system architecture
- Novel problem solving (no established patterns)
- High-stakes refactoring
- Critical production incidents

**Default**: When uncertain, use Sonnet (safe default for most work)

---

## Agent Invocation Syntax

All agents in violet-ai-plugins now support **explicit invocation syntax** for coordination.

### Invocation Template

When routing work between agents, use this format:

```
Invoke: Skill v-{agent-name}

Task: [Specific task description]

Model: [sonnet | haiku | opus]

Context:
- [Relevant specs, files, patterns to read]
- [Domain docs to reference]
- [Constraints or requirements]

Deliverable:
- [Expected output]
- Report completion back to [agent]
```

### Examples

**Tech Lead routes to Frontend Engineer:**
```
Invoke: Skill v-frontend-engineer

Task: Fix architectural violations in merchant pre-registration PR

Model: sonnet

Context:
- Read: systems/violet-dashboard/architecture-patterns.md (Redux patterns)
- PR: github.com/violetio/VioletDashboard/pull/1703
- Fix: Rewrite thunks to use createAppAsyncThunk pattern
- Files: redux/thunks/preRegistration.ts, redux/reducers/preRegistration.ts

Deliverable:
- Pattern-compliant implementation
- Report completion back to Tech Lead
```

**Tech Lead routes to QA Engineer:**
```
Invoke: Skill v-qa-engineer

Task: Write comprehensive tests for merchant pre-registration feature

Model: sonnet (strategy), haiku (implementation)

Context:
- What was implemented: redux/thunks/preRegistration.ts, pages/api/merchants/pre-register.ts
- Critical functionality: Redux state management, API proxy pattern
- Domain testing: Multi-tenancy isolation testing required
- Coverage target: >80% for Redux, API routes, hooks

Deliverable:
- Test plan (what to test, coverage targets)
- Unit tests for Redux (thunks + reducers)
- Unit tests for API routes
- All tests passing
- Report completion back to Tech Lead
```

**PM routes to Architect:**
```
Invoke: Skill v-architect

Task: Architecture design for merchant pre-registration feature

Model: sonnet

Context:
- Requirements document: specs/merchant-pre-registration/requirements.md
- Experience design: specs/merchant-pre-registration/experience.md
- Technical constraints: Must use Redux, API proxy pattern mandatory
- Performance: <200ms p99 for dashboard API calls

Deliverable:
- Architecture specification
- API contracts
- Database schemas (if needed)
- ADR for significant decisions
- Hand off to Tech Lead for implementation planning
```

### Completion Reports

After completing work, agents should report back in a structured format:

```
Completion Report: [Agent Name]

Task: [What was worked on]

Changes:
- [Key changes made]
- [Files modified]

Status:
- Requirements met: [Yes/No]
- Tests passing: [Yes/No]
- Ready for next phase: [Yes/No]

Blockers: [Any issues or None]
```

### Benefits of Explicit Invocation

1. **Actionable**: Agents know exactly what to do (not just prose descriptions)
2. **Model selection**: Specifies which model to use based on complexity
3. **Context loading**: Lists exact files and patterns to read
4. **Deliverables**: Clear expectations for output
5. **Coordination**: Explicit handoff between agents

---

## Related Repos

| Repo | Purpose |
|------|---------|
| `violet-brain` | Central patterns, templates, coordination |
| `beam-brain` | Beam product context |
| `prism-brain` | Prism product context |

Plugins provide horizontal (generic) knowledge; brain repos provide vertical (product-specific) depth.
