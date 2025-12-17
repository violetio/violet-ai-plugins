---
description: Capture current conversation context for continuity
---

## Context

- Current working directory: !`pwd`
- Current todo list state: (will be shown if TodoWrite has items)

## Your Task

Execute the mid-conversation capture protocol:

1. **Summarize current progress**:
   - What has been accomplished so far?
   - What is currently in progress?
   - What remains to be done?

2. **Capture decision context**:
   - What decisions were made and why?
   - What alternatives were considered?
   - Any blockers or open questions?

3. **Document handoff state**:
   - If conversation were to end now, what would someone need to know to continue?
   - What files were modified?
   - What's the next logical step?

4. **Output a capture summary** the user can save or reference later

This is useful for:
- Long conversations that might hit context limits
- Handing off to another session
- Creating checkpoints before risky operations
