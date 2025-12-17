# Product Discovery

## Overview

Product discovery is the process of understanding user problems and validating solutions before building.

## The Discovery Process

```
Understand → Explore → Validate → Define
```

### 1. Understand the Problem

**Activities**:
- User interviews
- Support ticket analysis
- Usage data review
- Competitive analysis

**Questions to Answer**:
- What problem are users trying to solve?
- How are they solving it today?
- What's painful about current solutions?
- How important is this problem?

**Output**:
- Problem statement
- User personas
- Current state mapping

### 2. Explore Solutions

**Activities**:
- Brainstorming
- Sketching
- Rapid prototyping
- Expert consultation

**Questions to Answer**:
- What solutions could address this?
- What are the trade-offs of each?
- What's the simplest viable solution?
- What risks exist?

**Output**:
- Solution options
- Trade-off analysis
- Initial wireframes

### 3. Validate Assumptions

**Activities**:
- Prototype testing
- User feedback sessions
- A/B tests (if applicable)
- Feasibility assessment

**Questions to Answer**:
- Does this solve the user's problem?
- Will users actually use this?
- Can we build this?
- Does it align with business goals?

**Output**:
- Validation results
- User feedback
- Technical feasibility assessment

### 4. Define the Solution

**Activities**:
- Requirements writing
- Success metrics definition
- Scope agreement
- Stakeholder alignment

**Output**:
- Clear requirements
- Success criteria
- Agreed scope
- Stakeholder sign-off

## Discovery Artifacts

### Problem Statement Template
```markdown
## Problem Statement

**User**: [Who experiences this problem]
**Problem**: [What they're trying to do]
**Current State**: [How they do it today]
**Pain Points**: [What makes it painful]
**Impact**: [What happens if unsolved]
```

### Hypothesis Template
```markdown
## Hypothesis

We believe that [solution]
for [user segment]
will result in [outcome]

We will know this is true when:
- [Measurable signal 1]
- [Measurable signal 2]
```

### User Interview Guide
```markdown
## Interview Guide: [Topic]

### Opening (5 min)
- Thank them for their time
- Explain purpose (learning, not selling)
- Ask permission to take notes

### Context (10 min)
- Tell me about your role
- Walk me through a typical [relevant activity]
- What tools do you use for [topic]?

### Problem Exploration (20 min)
- What's the hardest part about [topic]?
- Tell me about the last time you [experienced problem]
- How did you handle it?
- What did you try first?

### Solution Exploration (10 min)
- If you could wave a magic wand, what would change?
- Have you tried [alternative]? What happened?
- What would make you switch from current solution?

### Closing (5 min)
- Is there anything I should have asked?
- Can I follow up if I have more questions?
```

## Discovery Anti-Patterns

### Skip Discovery
"We already know what to build"
- Risk: Building wrong thing
- Fix: Even brief discovery reduces risk

### Feature Factory
"Users asked for X, let's build X"
- Risk: Building symptom fix, not solving root cause
- Fix: Dig into the "why" behind requests

### Solution First
"I have a great idea for..."
- Risk: Solving non-problem
- Fix: Start with problem, not solution

### Analysis Paralysis
"We need more research"
- Risk: Never shipping
- Fix: Time-box discovery, ship to learn

## Success Criteria for Discovery

Discovery is complete when you can confidently answer:

1. **Problem**: What specific problem are we solving?
2. **User**: For whom?
3. **Value**: Why does it matter?
4. **Solution**: What are we building?
5. **Success**: How will we know it worked?
