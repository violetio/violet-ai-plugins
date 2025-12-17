---
description: Start a new conversation with proper understanding and alignment
---

## Context

- Current working directory: !`pwd`
- Check for CLAUDE.md: !`cat CLAUDE.md 2>/dev/null | head -100 || echo "No CLAUDE.md found"`
- Recent git activity: !`git log --oneline -5 2>/dev/null || echo "Not a git repo"`

## Your Task

You are starting a new conversation. Follow the convo-start protocol:

1. **Read the context above** to understand the project
2. **If CLAUDE.md exists**, identify key workflows, patterns, and conventions
3. **Check for active work** (dashboards, status files, Linear references)
4. **Summarize the current state** to the user:
   - What project is this?
   - What's the current state of work?
   - Any active features or blockers?
5. **Ask what they'd like to work on**

If the user has already stated what they want to work on, proceed to understanding and alignment:
- State your understanding of their request
- Propose an execution plan
- Wait for confirmation before proceeding
