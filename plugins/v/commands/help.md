---
description: Get help with V plugin commands and workflows
---

# V Plugin Help

## Quick Start

**Installation:**
Add to `.claude/settings.json`:
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

**First Use:**
Run `/v:v` and describe what you want to build.

**Updates:**
When violet-ai-plugins updates, run: `/plugin marketplace update violet`

---

## Commands Available

| Command | When to Use |
|---------|-------------|
| `/v:v` | Starting any product work - analyzes your request and routes automatically |
| `/v:feature` | Run the full feature workflow (Discovery → Review) |
| `/v:discovery` | Iterate on Discovery phase only |
| `/v:define` | Iterate on Definition phase only |
| `/v:rca` | Conduct root cause analysis with three-test isolation |
| `/v:convo-exit` | End conversation with debrief, feedback, and knowledge capture |
| `/v:convo-capture` | Save mid-conversation context for continuity or handoff |
| `/v:help` | Show this help |

---

## Workflow Overview

The V workflow guides features from idea to implementation:

**Discovery → Definition → Design → Development → Review**

**Implemented Phases:**
- **Discovery**: Understand the problem (intake → exploration → brief)
- **Definition**: Define requirements, experience design, and feasibility

**Coming Soon:**
- Design (technical architecture)
- Development (implementation)
- Review (quality gates)

*Want details on a specific phase? Ask: "Tell me more about [phase name]"*

---

## Skills Available

The V workflow uses specialized skills for different tasks:

| Skill | What It Does |
|-------|--------------|
| `requirements-analyst` | Converts discovery into functional requirements |
| `experience-designer` | Designs user experience across personas and surfaces |
| `architect` | Analyzes technical feasibility and approach |
| `codebase-explorer` | Finds patterns and constraints in existing code |
| `docs-explorer` | Searches brain repos and Notion documentation |
| `research-explorer` | Conducts external research (with permission) |

---

## Getting More Help

**Have questions?**
- Ask: "How do I [task]?" for specific guidance
- Ask: "What does [skill/command] do?" for details

**Plugin Info:**
- Repository: https://github.com/violetio/violet-ai-plugins
- Current version: 0.0.6
