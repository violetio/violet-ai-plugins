---
name: convo-exit
description: Conversation exit protocol for debrief and continuous improvement
---

# Conversation Exit

> Wraps up conversations with debrief, feedback, improvements, and knowledge capture.

---

## Triggers

- User says "we're done", "that's it", "looks good"
- Conversation naturally concluding
- User explicitly requests exit/debrief
- Topic drift suggests starting fresh

---

## Topic Drift Detection

### Signs of Drift

- Original task incomplete but discussing something new
- Multiple unrelated topics in one thread
- Context window filling with unrelated content
- Hard to summarize what the conversation is about

### Handling Drift

```markdown
I notice we've shifted from [original topic] to [new topic].

Would you like to:
A) Start a new conversation focused on [new topic]?
B) Continue here and I'll help with both?

Starting fresh helps maintain context and lets us debrief separately.
```

**Don't offer** if:
- Still on the same logical topic
- In the middle of productive work
- Would disrupt flow

---

## Exit Protocol

### Step 1: Review

```markdown
Let me review our conversation:

**Original goal**: [State the goal]

**What we accomplished**:
- [Outcome 1]
- [Outcome 2]

**Files created/modified**:
- `path/file.md` - [Brief description]
```

### Step 2: Tasks Remaining

```markdown
**Tasks Remaining**:

For this effort:
- [ ] [Remaining task 1]
- [ ] [Remaining task 2]

For this repo:
- [ ] Update [file]: [why]
- [ ] Add to [agent]: [what]
- [ ] Create new pattern: [for what]
```

### Step 3: Ask for Feedback

```markdown
**Your feedback**:

1. What worked well in this conversation?
2. What would you have liked to see improved?
3. Were there moments where I was unclear or off-track?
```

### Step 4: Propose Changes

After receiving feedback:

```markdown
**Based on our conversation, I propose these changes**:

1. [File path]: [Specific change]
2. [File path]: [Specific change]

Should I make these changes?
```

### Step 5: Execute Approved Changes

Make only the approved changes.

### Step 6: Create Convo-Exit Document

**File**: `convo-exits/YYYY-MM-DD-HHMM-topic.md`

Use the template below.

### Step 7: Activity Log Update (If Applicable)

**Required if** brain repo has `personal/activity-log.md` AND significant work was done.

**Not optional for significant work.** If the file exists and this was substantive effort, always update it:

```markdown
Would you like me to update your personal activity log with what you accomplished?
```

Then proceed with the update using the Activity Log Format below.

---

## Convo-Exit Template

Create `convo-exits/YYYY-MM-DD-HHMM-topic.md`:

```markdown
# Conversation Exit: [Topic]

**Date**: YYYY-MM-DD HH:MM
**Duration**: [approximate, if determinable]
**User**: [if identifiable]
**Repo**: [which brain repo(s) were involved]

---

## Original Goal

[What we set out to do at the start of the conversation]

---

## Outcomes

**Files Created**:
- `path/to/file.md` - Description

**Files Modified**:
- `path/to/file.md` - What changed and why

**Decisions Made**:
- Key decision 1
- Key decision 2

---

## User Feedback

**What worked well**:
[User's positive feedback]

**What could improve**:
[User's suggestions]

**Additional notes**:
[Any other feedback]

---

## Improvements Made

**To this repo**:
- [ ] [Change 1] - [Status: Completed/Deferred/Rejected]

**To CLAUDE.md**:
- [ ] [Change 1] - [Status]

**To agents**:
- [ ] [Change 1] - [Status]

**To patterns**:
- [ ] [Change 1] - [Status]

---

## Suggestions for Future

**Patterns to consider**:
- [Pattern idea - what it would solve]

**Agent improvements**:
- [Agent improvement]

**Documentation gaps**:
- [Gap - what's missing]

---

## Conversation Summary

[3-5 sentence summary: what we discussed, how it evolved, what we accomplished, notable insights or pivots]

---

## Next Steps

**Immediate**:
- [ ] [Action item 1]

**Future**:
- [ ] [Longer-term follow-up]

---

## Related

**Linear Issues**:
- [Issue ID] - [Brief description]

**Other Conversations**:
- [Link to related convo-exit]

**Specs/Docs Created**:
- [Link to specs/docs]
```

---

## Activity Log Format

For `personal/activity-log.md`:

```markdown
## YYYY-MM-DD HH:MM - [Topic]

**What I accomplished**:
- [Accomplishment 1]
- [Accomplishment 2]

**Repos affected**: [list]

**Related**:
- `convo-exits/YYYY-MM-DD-HHMM-topic.md`

**Key insights**:
- [Insight]

**Ideas for future**:
- [Deferred work or follow-up]
```

---

## When to Use

### Always Use When
- Creating or modifying documents/code
- Significant work was done
- User wants feedback captured

### Skip When
- Quick question/answer
- User explicitly says "no debrief"
- Trivial interaction

### Abbreviated Exit
For shorter conversations:

```markdown
**Quick wrap-up**:
- Created [file]
- Next: [follow-up item]
- Feedback? [quick prompt]
```

---

## Setting Up convo-exits/

To enable in a brain repo:

1. Create directory: `mkdir -p convo-exits`
2. Add `.gitkeep` or initial template
3. Optionally gitignore (if local-only):
   ```
   convo-exits/*.md
   !convo-exits/_template.md
   ```

---

## Success Metrics

A conversation exited well:
- [ ] User never confused about outcomes
- [ ] Clear record of what was accomplished
- [ ] Learnings captured for future
- [ ] Improvements made to the system
- [ ] User feels heard and in control

---

## Integration

### With Convo-Start
- Exit captures inform future starts
- Patterns identified improve alignment

### With Convo-Capture
- Exit is comprehensive capture at end
- Mid-conversation captures feed into exit

### With Feedback Collector
- Exit includes feedback gathering
- Feedback drives improvements

### With Assistant
- Exit can update activity log
- Informs day-end summaries
