# Contributing to Violet AI Plugins

## Plugin Categories

Before contributing, understand which category your content belongs to:

| Category | Key Test | Requirements |
|----------|----------|--------------|
| **Agents** | "You ARE a [role]" | Defines a professional persona with specific expertise |
| **Stack** | Only applies to technology X? | Bound to a specific language/framework |
| **Domain** | About a Violet product area? | Covers Violet-specific business logic |
| **Platform** | About an external system? | Third-party API patterns and quirks |
| **Skills** | Transferable technique? | Cross-cutting competency that applies anywhere |
| **Workflows** | Sequential steps/phases? | Process with defined order and gates |

## Decision Matrix

Ask these questions in order:

1. Does it define WHO Claude should be? → **Agent**
2. Is it bound to a specific technology? → **Stack**
3. Is it about a Violet product area? → **Domain**
4. Is it about an external system we integrate with? → **Platform**
5. Does it have sequential steps/phases? → **Workflow**
6. Is it transferable technique knowledge? → **Skill**

## How to Contribute

### Adding to an Existing Plugin

1. Fork this repository
2. Add your content to the appropriate `knowledge/` or `agents/` folder
3. Open a PR with:
   - Description of what you're adding
   - Why it belongs in this category
   - Example usage

### Proposing a New Plugin

New plugins require approval. Open an issue first with:
- Proposed category and name
- What content it would contain
- Why existing plugins don't cover it

## File Structure

### Agents
```
plugins/agents/agents/
├── backend-engineer.md
├── frontend-engineer.md
└── ...
```

### Knowledge-based plugins (Stack, Domain, Platform, Skills)
```
plugins/{category}-{name}/.claude-plugin/
├── plugin.json
└── knowledge/
    └── {topic}.md
```

### Workflows
```
plugins/workflows/
├── .claude-plugin/plugin.json
├── commands/
│   └── git.md
└── knowledge/
    ├── 9d-framework.md
    └── ...
```

## Review Process

1. PRs require approval from @violetio/eng-leads
2. Changes to `.claude-plugin/` files require additional review
3. New plugins require architecture discussion

## Questions?

Contact the platform team or post in #claude-code-plugins.
