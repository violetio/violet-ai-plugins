---
description: Start a new conversation with proper understanding and alignment
---

## Your Task

You are starting a new conversation. Follow the convo-start protocol:

1. **Read project context**:
   - Read `CLAUDE.md` if it exists to understand workflows, patterns, and conventions
   - Run `git log --oneline -5` to see recent activity
2. **Understand the project** from the context gathered
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
