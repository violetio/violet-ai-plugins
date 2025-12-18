---
description: Check if you're working in the right repository context (brain vs code)
---

## Your Task

Analyze the current repository and conversation context to verify you're working in the appropriate repository type for the task at hand.

### Analysis Steps

1. **Identify Repository Type**
   - Read `.claude/settings.json` to see enabled plugins
   - Look for directory indicators:
     - Brain repo: `specs/`, `roadmap/`, `patterns/`, `docs/` directories
     - Code repo: `src/`, `lib/`, `main/`, `components/`, Dockerfile, etc.
   - Count enabled plugins (brain repos: 20+, code repos: 8-15)
   - Check for planning agent presence: v-architect, v-tech-lead, v-*-pm

2. **Analyze Conversation Context**
   - Review recent messages to identify task type
   - Look for planning indicators (design, architecture, specs, decisions)
   - Look for implementation indicators (code changes, bug fixes, tests)

3. **Determine Appropriate Repository**
   - **Brain Repo is appropriate if**: NEW work, specs needed, architecture decisions, trade-offs, product input required
   - **Code Repo is appropriate if**: Implementing from spec, fixing known bug, adding tests, refactoring code

4. **Provide Appropriate Response**

If in **WRONG context** (planning work in code repo):
```markdown
⚠️ **Warning: Planning Work in Code Repository**

You're currently in a **code repository** but the conversation suggests you're doing:
- {specific planning activities detected}

These activities belong in a **brain repository** where you have:
- Planning agents (v-architect, v-tech-lead, product managers)
- Strategic context and roadmap
- Requirements and specs
- Architecture decision records
- Cross-product coordination

**Recommended action:**
1. Switch to the appropriate brain repository:
   - `violet-brain/` for cross-functional work
   - `{product}-brain/` for product-specific work
2. Create or refine specs/requirements there
3. Return here to implement once you have a complete spec

**Related brain repos:**
- violet-brain: Central coordination and cross-product context
- prism-brain: Prism product area context
- beam-brain: Beam product area context
```

If in **CORRECT context** (code repo, implementation):
```markdown
✅ **Correct Context: Code Repository**

You're in the right place for implementation work! This code repository is appropriate for:
- Implementing from existing specs or ADRs
- Bug fixes with known root cause
- Adding tests to existing code
- Code refactoring without architectural changes

If you need to make architectural decisions or create new specs, consider switching to the brain repository first.
```

If in **CORRECT context** (brain repo, planning):
```markdown
✅ **Correct Context: Brain Repository**

You're in the right place for planning and strategy! This brain repository is appropriate for:
- Planning new features and products
- Creating specifications and requirements
- Making architectural and trade-off decisions
- Coordinating with multiple teams
- Analyzing cross-product impacts

When you have a complete spec ready to implement, you can switch to the code repository for implementation.
```
