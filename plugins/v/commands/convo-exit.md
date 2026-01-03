---
description: End conversation with proper debrief and knowledge capture
allowed-tools:
  - Read
  - Write
  - Glob
  - Bash
  - AskUserQuestion
---

# Conversation Exit Protocol

Execute this 6-step protocol to properly close out a conversation with knowledge capture and feedback.

---

## Step 1: Review Accomplishments

Summarize what was accomplished in this conversation:

```markdown
**Original goal**: [State what the user wanted to achieve]

**What we accomplished**:
- [Outcome 1]
- [Outcome 2]

**Files created/modified**:
- `path/file.md` - [Brief description]
```

---

## Step 2: Identify Remaining Tasks

List any incomplete work:

```markdown
**Tasks Remaining**:

For this effort:
- [ ] [Remaining task 1]
- [ ] [Remaining task 2]

**Blockers or dependencies**:
- [Any blockers]
```

---

## Step 3: Ask for Feedback

Request user feedback with these questions:

```markdown
**Your feedback**:

1. What worked well in this conversation?
2. What would you have liked to see improved?
3. Were there moments where I was unclear or off-track?
```

Wait for the user to respond before proceeding.

---

## Step 4: Propose Improvements

Based on feedback received, propose actionable changes:

```markdown
**Based on our conversation, I propose these changes**:

1. [File path]: [Specific change]
2. [File path]: [Specific change]

Should I make these changes?
```

If user approves, make the changes.

---

## Step 5: Create Convo-Exit Document

Create a conversation exit file in the current repo's `convo-exits/` directory.

**File**: `convo-exits/YYYY-MM-DD-HHMM-topic.md`

Use this template:

```markdown
# Conversation Exit: [Topic]

**Date**: YYYY-MM-DD HH:MM
**Repo**: [which repo(s) were involved]

## Original Goal
[What we set out to do]

## Outcomes
**Files Created**:
- [list with descriptions]

**Files Modified**:
- [list with descriptions]

**Decisions Made**:
- [key decisions and rationale]

## User Feedback
**What worked well**: [feedback]
**What could improve**: [feedback]

## Improvements Made
- [Change 1] - [Status: Completed/Deferred/Rejected]

## Next Steps
**Immediate**:
- [actionable items]

**Future**:
- [longer-term follow-ups]
```

---

## Step 6: Activity Log Update (Optional)

Ask the user:

> Would you like me to update your personal activity log with what you accomplished?

If yes, and `personal/activity-log.md` exists in the repo, append:

```markdown
## YYYY-MM-DD HH:MM - [Topic]

**What I accomplished**:
- [Accomplishment 1]
- [Accomplishment 2]

**Repos affected**: [list]
**Related**: `convo-exits/YYYY-MM-DD-HHMM-topic.md`
```

---

## When to Use

- User says "we're done", "that's it", "looks good"
- Conversation naturally concluding
- User explicitly requests exit/debrief
- Significant work was completed

## When to Skip

- Quick questions with simple answers
- No files were created or modified
- User explicitly wants to skip

---

## Success Criteria

- [ ] Clear record of what was accomplished
- [ ] User feedback captured
- [ ] Learnings/improvements identified
- [ ] User feels heard and in control