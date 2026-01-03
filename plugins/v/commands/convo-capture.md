---
description: Capture current conversation context for continuity
allowed-tools:
  - Read
  - Write
  - Glob
  - AskUserQuestion
---

# Conversation Capture Protocol

Execute this protocol to save context mid-conversation for continuity and handoff.

---

## When to Capture

### Automatic Triggers
- **Significant decision made** - Architecture choice, approach selected, scope change
- **Large work completed** - Document created, major code changes, phase finished
- **Topic shift detected** - Before switching to a new topic
- **Natural breakpoint** - Before user goes to meeting, logical pause

### On Request
- User asks "save where we are"
- User needs to leave but wants to continue later
- Handoff to another person/session
- Before risky operations (in case rollback needed)

---

## Step 1: Summarize Progress

Document the current state:

```markdown
**Current Status**: [In Progress / Paused / Blocked]

**What has been accomplished**:
- [Completed item 1]
- [Completed item 2]

**What is currently in progress**:
- [Active work]

**What remains to be done**:
- [ ] [Remaining task 1]
- [ ] [Remaining task 2]
```

---

## Step 2: Capture Decision Context

Document decisions and their rationale:

```markdown
**Decisions Made**:

| Decision | Rationale | Alternatives Considered |
|----------|-----------|------------------------|
| [Choice] | [Why we chose this] | [Other options we rejected] |
```

Also note:
- Any blockers or open questions
- Assumptions being made
- Dependencies identified

---

## Step 3: Document Handoff State

Create clear resume instructions:

```markdown
**Files Changed**:
- **Created**: [list with paths]
- **Modified**: [list with paths]

**To Resume This Work**:
1. [First thing to do]
2. [Second thing to do]
3. [Next logical step]

**Context Needed**:
- [Any background knowledge required]
- [Related issues or specs]
```

---

## Step 4: Output Capture

Choose the appropriate format based on need:

### Option A: Inline Capture (Quick)

Output directly in conversation:

```markdown
---
**Checkpoint**: [Brief description]

**Status**: [Current state]
**Just completed**: [What was done]
**Next**: [What's queued]
**To continue**: [Resume instructions]
---
```

### Option B: Context File (Detailed)

Create a file for complex state. Ask user:

> Should I save this capture to a file for future reference?

If yes, create `convo-captures/YYYY-MM-DD-HHMM-topic.md`:

```markdown
# Context Capture: [Topic]

**Date**: YYYY-MM-DD HH:MM
**Status**: In Progress / Paused / Blocked
**Repo**: [which repo]

## Current State
[Where we are in the work]

## Decisions Made
| Decision | Rationale | Alternatives Considered |
|----------|-----------|------------------------|
| [Choice] | [Why] | [Other options] |

## Files Changed
**Created**:
- [list]

**Modified**:
- [list]

## Open Questions
- [Question 1]
- [Question 2]

## Next Steps
1. [ ] [Immediate next step]
2. [ ] [Following step]

## To Resume
[Clear instructions for picking this back up]
```

---

## Use Cases

| Scenario | Recommended Format |
|----------|-------------------|
| Quick break, same session | Inline capture |
| End of day, continuing tomorrow | Context file |
| Handing off to colleague | Context file (detailed) |
| Before risky operation | Either (depends on complexity) |
| Context window getting full | Context file |

---

## Success Criteria

- [ ] Work can be resumed without context loss
- [ ] Another person could pick this up smoothly
- [ ] Past decisions are understandable with rationale
- [ ] Clear "what's next" instructions exist