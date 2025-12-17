# Conversation Capture

> Saves context mid-conversation for continuity, handoff, and audit trail.

---

## When to Capture

### Automatic Triggers

1. **Significant decision made**
   - Architecture choice
   - Approach selected from options
   - Scope change approved

2. **Large work completed**
   - Document created
   - Major code changes
   - Phase of work finished

3. **Topic shift detected**
   - Before switching to new topic
   - When conversation scope expands

4. **Natural breakpoint**
   - Before user goes to meeting
   - Logical pause in work

### On Request

- User asks "save where we are"
- User needs to leave but wants to continue later
- Handoff to another person/session

---

## What to Capture

### Essential

| Element | Why It Matters |
|---------|----------------|
| **Current state** | Where we are right now |
| **Decisions made** | What was decided and why |
| **Files changed** | What was created/modified |
| **Next steps** | What comes next |

### Optional

| Element | When to Include |
|---------|-----------------|
| **Alternatives rejected** | When decision was non-obvious |
| **Blockers encountered** | If work is paused |
| **Dependencies** | If waiting on something |
| **Questions raised** | If unresolved issues exist |

---

## Capture Format

### Inline Capture (Quick)

For brief saves during conversation:

```markdown
---
**Checkpoint**: [Brief description]

**Status**: [Current state of work]
**Just completed**: [What was done]
**Next**: [What's queued]
---
```

### Context File (Detailed)

For formal captures, create `convo-captures/YYYY-MM-DD-HHMM-topic.md`:

```markdown
# Context Capture: [Topic]

**Date**: YYYY-MM-DD HH:MM
**Status**: In Progress / Paused / Blocked

---

## Current State

[Where we are in the work]

---

## Decisions Made

| Decision | Rationale | Alternatives Considered |
|----------|-----------|------------------------|
| [Choice] | [Why] | [Other options] |

---

## Files Changed

**Created**:
- `path/file.md` - [Description]

**Modified**:
- `path/file.md` - [What changed]

---

## Next Steps

1. [ ] [Immediate next step]
2. [ ] [Following step]

---

## To Resume

[Instructions for picking this back up]

---

## Open Questions

- [ ] [Unresolved question 1]
- [ ] [Unresolved question 2]
```

### Activity Log Entry (Personal)

For personal tracking:

```markdown
## YYYY-MM-DD HH:MM - [Topic] (In Progress)

**Working on**: [Brief description]
**Current state**: [Where we are]
**Next session**: [What to tackle]
```

---

## Where to Save

| Type | Location | When |
|------|----------|------|
| **Inline** | In conversation | Quick checkpoints |
| **Context file** | `convo-captures/` or working directory | Detailed saves |
| **Activity log** | `personal/activity-log.md` | Personal tracking |
| **Working doc** | Relevant spec/doc itself | Document-specific context |

---

## Capture Process

### Step 1: Announce Capture

```markdown
Let me capture where we are before [reason].
```

### Step 2: Summarize State

```markdown
**Current state**: [Where we are]

**Completed so far**:
- [Item 1]
- [Item 2]

**Key decisions**:
- [Decision 1]: [Rationale]
```

### Step 3: Document Next Steps

```markdown
**To continue**:
1. [First thing to do]
2. [Second thing to do]

**Blockers/Questions**:
- [Any blockers]
```

### Step 4: Confirm with User

```markdown
Does this capture everything important? Anything to add?
```

---

## Use Cases

### Handoff to Another Session

```markdown
## Handoff: [Topic]

**Context**: [Why this is being handed off]

**Work completed**: [Summary]

**Work remaining**: [What's left]

**Key context**:
- [Important detail 1]
- [Important detail 2]

**Files to review**:
- [File 1]

**To continue**: [Start here]
```

### Pause for Meeting

```markdown
**Quick save** before your meeting:

We were working on [X]. We got through [Y]. When you're back, we'll [Z].

Files touched: [list]
```

### Scope Expansion

```markdown
**Checkpoint** - scope is expanding:

**Original goal**: [What we started with]
**Now also includes**: [New scope]
**Decision**: [Continue together / Split into separate effort]
```

---

## Anti-patterns

### Over-capturing
**Wrong**: Saving every micro-step
**Right**: Capture meaningful checkpoints

### Under-capturing
**Wrong**: No record of decisions made
**Right**: Capture decisions with rationale

### Vague Captures
**Wrong**: "We discussed some options"
**Right**: "Decided on X because Y; rejected Z because W"

### No Resume Instructions
**Wrong**: Context dump without how to continue
**Right**: Clear "To continue, do X" instructions

---

## Integration

### With Convo-Start
- Captures inform future convo-starts
- Previous context helps alignment phase

### With Convo-Exit
- Convo-capture is subset of convo-exit
- Use capture during work, exit at end

### With Activity Log
- Captures can feed into activity log
- Helps track work across sessions

---

## Success Metrics

Good captures enable:
- [ ] Resuming work without context loss
- [ ] Handing off to someone else smoothly
- [ ] Understanding past decisions later
- [ ] Finding where specific choices were made
