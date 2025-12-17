---
description: End conversation with proper debrief and knowledge capture
---

## Context

- Current working directory: !`pwd`
- Check for violet-brain convo-exits: !`ls -la ../violet-brain/convo-exits/ 2>/dev/null | tail -10 || echo "violet-brain not found at ../violet-brain"`

## Your Task

Execute the conversation exit protocol:

1. **Summarize what was accomplished** in this conversation
2. **Identify any learnings or improvements** that should be captured
3. **Create a conversation exit file** in `../violet-brain/convo-exits/` if violet-brain is available:
   - Use the template from the convo skill
   - File naming: `YYYY-MM-DD-HHMM-topic.md`
4. **Ask if user wants to update their personal activity log**:
   > Would you like me to update your personal activity log with what you accomplished?

   If yes, append to `../violet-brain/personal/activity-log.md`

5. **Confirm completion** and any follow-up items for next conversation
