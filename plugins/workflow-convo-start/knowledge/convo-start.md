# Conversation Start

> Establishes understanding and alignment before diving into execution.

---

## Phase 1: Understanding (Start of conversation)

**Trigger**: New conversation begins or topic shifts significantly

**Actions**:
1. **Read the request carefully**
2. **Identify**:
   - What type of work is this? (feature, spec review, planning, research, fix)
   - Which agents/patterns are relevant?
   - What context do I need?
3. **Gather initial context** (follow research-priority pattern)

**Output**: Internal understanding of the work

**Research Priority Pattern**:
1. Repo docs (CLAUDE.md, README, existing specs)
2. Linear/issue tracker (if referenced)
3. Ask user for clarification
4. External sources (last resort)

---

## Phase 2: Alignment (Before execution)

**Trigger**: After understanding the request

**Actions**:

### 1. State Your Understanding
```markdown
I understand you want to [specific goal].

This involves:
- [Scope item 1]
- [Scope item 2]
```

### 2. Propose Execution Plan
```markdown
Here's my approach:
1. [Step 1]
2. [Step 2]
3. [Step 3]

I'll use:
- [Relevant agent/pattern]
- [Another relevant pattern]
```

### 3. Wait for Confirmation
Do NOT proceed until user confirms or corrects the plan.

**Output**: User confirms or corrects your plan

---

## Example

```markdown
I understand you want to create a requirements spec for merchant onboarding.

This involves:
- Understanding the current onboarding flow
- Identifying gaps and improvements
- Creating a formal requirements document

Here's my approach:
1. Read existing merchant onboarding context
2. Follow Definition phase skill
3. Apply research-priority pattern
4. Create the requirements spec
5. Validate with quality-gate-reviewer

Does this align with what you need?
```

---

## When to Skip

**Skip full alignment when**:
- Simple question requiring quick answer
- User explicitly says "just do X"
- Continuing prior conversation with established context
- User is in a hurry (adjust to quick confirmation)

**Never skip when**:
- Creating or modifying documents/code
- Work will take significant time
- Multiple approaches are possible
- User's intent is unclear

---

## Anti-patterns

### Jumping to Execution
**Wrong**: Immediately starting work without stating understanding
**Right**: State what you understood, get confirmation

### Assuming Context
**Wrong**: Acting on assumptions about what user wants
**Right**: Explicitly state assumptions and ask for confirmation

### Over-planning
**Wrong**: Detailed multi-page plan for simple task
**Right**: Match plan detail to task complexity

### Under-planning
**Wrong**: Vague "I'll work on this" without specifics
**Right**: Concrete steps user can evaluate

---

## Quick Alignment Template

For simpler tasks:

```markdown
Got it - you want [X]. I'll [approach]. Sound good?
```

For complex tasks:

```markdown
Let me make sure I understand:

**Goal**: [What you want to achieve]
**Scope**: [What's included/excluded]
**Approach**: [How I'll tackle it]
**Output**: [What you'll get]

Anything to adjust before I start?
```

---

## Integration

### Before Convo-Start
- Review any available context (CLAUDE.md, recent activity)
- Check if this is a continuation of prior work

### After Convo-Start
- Proceed to execution with approved plan
- Use TodoWrite to track progress
- Don't interrupt unnecessarily during execution

### Related Workflows
- `workflow-convo-capture` - For saving progress mid-conversation
- `workflow-convo-exit` - For wrapping up and capturing learnings

---

## Success Metrics

Good conversation starts result in:
- [ ] User never confused about what you're doing
- [ ] Plan is specific enough to evaluate
- [ ] User can correct misunderstandings early
- [ ] Execution proceeds without "wait, I thought you were doing X"
