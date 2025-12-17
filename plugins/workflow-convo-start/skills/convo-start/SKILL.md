---
name: convo-start
description: Conversation start protocol for establishing understanding and alignment. Use this automatically at the start of every new conversation to understand user intent, gather context, and propose an execution plan before diving into work.
---

# Conversation Start

> Establishes understanding and alignment before diving into execution.

## When This Skill Activates

- **Automatically** at the start of every new conversation
- When user explicitly says "convo-start"
- When topic shifts significantly mid-conversation

## Phase 1: Understanding

**Actions**:
1. **Read the request carefully**
2. **Identify**:
   - What type of work is this? (feature, spec review, planning, research, fix)
   - Which agents/patterns are relevant?
   - What context do I need?
3. **Gather initial context** (follow research-priority pattern)

**Research Priority Pattern**:
1. Repo docs (CLAUDE.md, README, existing specs)
2. Linear/issue tracker (if referenced)
3. Ask user for clarification
4. External sources (last resort)

## Phase 2: Alignment

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

## Quick Templates

**For simple tasks:**
```markdown
Got it - you want [X]. I'll [approach]. Sound good?
```

**For complex tasks:**
```markdown
Let me make sure I understand:

**Goal**: [What you want to achieve]
**Scope**: [What's included/excluded]
**Approach**: [How I'll tackle it]
**Output**: [What you'll get]

Anything to adjust before I start?
```

## When to Skip Full Alignment

**Skip when**:
- Simple question requiring quick answer
- User explicitly says "just do X"
- Continuing prior conversation with established context

**Never skip when**:
- Creating or modifying documents/code
- Work will take significant time
- Multiple approaches are possible
- User's intent is unclear

## Anti-patterns

| Wrong | Right |
|-------|-------|
| Immediately starting work | State understanding first, get confirmation |
| Acting on assumptions | Explicitly state assumptions, ask for confirmation |
| Detailed plan for simple task | Match plan detail to task complexity |
| Vague "I'll work on this" | Concrete steps user can evaluate |

## Success Metrics

- [ ] User never confused about what you're doing
- [ ] Plan is specific enough to evaluate
- [ ] User can correct misunderstandings early
- [ ] Execution proceeds without "wait, I thought you were doing X"
