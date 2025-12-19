# Model Selection Strategy

## Overview

Claude Code supports multiple models with different capabilities and costs. Always select the appropriate model for the task at hand.

## Model Characteristics

| Model | Speed | Cost | Best For |
|-------|-------|------|----------|
| **Sonnet 4.5** | Medium | Medium | Complex reasoning, architectural decisions, debugging |
| **Haiku** | Fast | Low | Simple tasks, pattern matching, established workflows |
| **Opus 4.5** | Slow | High | Critical decisions, novel problems, high-stakes work |

## Selection Guidelines

### Use Sonnet 4.5 When:

**Code Changes:**
- Architectural violations that need fixing
- Pattern compliance (first-time learning)
- Complex refactoring
- Debugging difficult failures
- Understanding nuanced review feedback

**Testing:**
- Designing test strategy
- Complex integration test scenarios
- Debugging unexpected test failures
- First-time test implementation for new patterns

**Documentation:**
- Evaluating domain documentation accuracy
- Recommending documentation updates
- Writing architectural decision records

**General:**
- Work requires understanding context across multiple files
- Decision-making with trade-offs
- Learning and applying new patterns

### Use Haiku When:

**Code Changes:**
- Simple fixes (console.log removal, typo fixes)
- Following established patterns exactly
- Straightforward implementations with clear examples
- Batch operations (multiple similar changes)

**Testing:**
- Implementing tests following existing patterns
- Running test suites
- Writing unit tests with clear examples to follow
- Simple assertion updates

**Documentation:**
- Simple documentation updates
- Adding examples to existing docs

**General:**
- Task has clear, unambiguous requirements
- Pattern to follow is well-established
- No architectural decisions needed

### Use Opus 4.5 When:

**Critical Work:**
- Security-sensitive decisions
- Complex multi-system architecture
- Novel problem solving (no established patterns)
- High-stakes refactoring with significant risk
- Critical production incidents

**Generally:**
- Opus should be rare - reserve for truly complex, high-stakes work
- Most work should be Sonnet (complex) or Haiku (simple)

## Decision Tree

```
Is this a critical security/architecture decision?
└─ YES → Opus 4.5
└─ NO ↓

Is this following an established pattern exactly?
└─ YES → Haiku
└─ NO ↓

Does this require understanding context/trade-offs?
└─ YES → Sonnet 4.5
└─ NO → Haiku
```

## Agent-Specific Recommendations

### Tech Lead
- **Sonnet**: PR comment analysis, coordination decisions, evaluation
- **Haiku**: Status updates, simple routing decisions

### Frontend Engineer
- **Sonnet**: Architectural changes, pattern violations, complex refactoring
- **Haiku**: console.log removal, following patterns, simple component updates

### QA Engineer
- **Sonnet**: Test strategy, complex scenarios, debugging
- **Haiku**: Implementing tests from patterns, running suites, simple assertions

### Architect
- **Sonnet**: Always (architectural decisions require deep reasoning)
- **Opus**: Critical architecture decisions affecting multiple systems

### PM Agent
- **Sonnet**: Discovery, requirements, complex product decisions
- **Haiku**: Simple status updates, straightforward spec updates

## Cost/Performance Trade-offs

**Optimize for:**
- **Speed**: Use Haiku for simple tasks (10x faster)
- **Cost**: Use Haiku for batch operations (much cheaper)
- **Quality**: Use Sonnet for complex work (better reasoning)
- **Critical**: Use Opus sparingly (expensive, slower, but highest quality)

## Examples

### Example 1: PR Review Comments

```
User: "Got 10 PR comments back on merchant pre-registration"

Tech Lead (Sonnet):
- Analyzes: Complex architectural violations
- Routes to: Frontend Engineer (Sonnet) for fixes
- Reason: Pattern violations require understanding WHY patterns exist

Frontend Engineer (Sonnet):
- Rewrites thunks to use createAppAsyncThunk
- Reason: First time learning this pattern, need to understand it

Tech Lead (Sonnet):
- Reviews fixes, routes to QA Engineer
- Reason: Need to determine test strategy

QA Engineer (Sonnet for strategy):
- Designs: What tests needed, coverage targets
- Then switches to Haiku for implementation
- Reason: Strategy is complex, but implementing tests follows patterns

QA Engineer (Haiku for implementation):
- Writes 30 tests following established patterns
- Reason: Tests follow clear examples, just pattern matching
```

### Example 2: Simple Console.log Removal

```
User: "PR has 2 comments about console.log statements"

Tech Lead (Haiku):
- Analyzes: Simple cleanup task
- Routes to: Frontend Engineer (Haiku)
- Reason: Straightforward, no complexity

Frontend Engineer (Haiku):
- Removes console.log statements
- Reason: Simple deletion, no architectural decisions

Tech Lead (Haiku):
- Quick review, no tests needed for simple cleanup
- Reason: Trivial change doesn't need test updates
```

### Example 3: Critical Security Issue

```
User: "Security audit found PII exposure risk"

Tech Lead (Opus):
- Analyzes: Critical security issue
- Routes to: Architect (Opus) for design
- Reason: High-stakes security decision

Architect (Opus):
- Designs secure solution
- Reason: Security-sensitive, multiple trade-offs

Frontend Engineer (Sonnet):
- Implements security fixes
- Reason: Complex implementation, security-critical

QA Engineer (Sonnet):
- Writes comprehensive security tests
- Reason: Security tests need deep understanding
```

## Switching Models Mid-Task

**When to switch:**
- Started with Haiku but hit complexity → Switch to Sonnet
- Started with Sonnet for strategy → Switch to Haiku for implementation
- Critical issue discovered → Escalate to Opus

**How to switch:**
- Use the `/model` command or specify in agent invocation
- Document why you're switching in your reasoning

## Anti-Patterns

**❌ Don't:**
- Use Opus for simple tasks (waste of resources)
- Use Haiku for architectural decisions (insufficient reasoning)
- Use Sonnet for batch simple operations (unnecessary cost)
- Never check which model to use (always deliberate)

**✅ Do:**
- Start with Sonnet when uncertain (safe default)
- Switch to Haiku once pattern is clear
- Use Opus sparingly for truly critical work
- Document model selection reasoning

## Measuring Success

Track:
- Time to completion (Haiku should be faster)
- Cost per task (Haiku should be cheaper)
- Quality of output (Sonnet should handle complexity better)
- Pattern compliance (Haiku should match patterns exactly)

Adjust strategy based on results.

## References

- [Tech Lead Agent](../plugins/v-tech-lead/skills/tech-lead/SKILL.md) - Coordination workflow
- [Frontend Engineer Agent](../plugins/v-frontend/skills/frontend-engineer/SKILL.md) - Implementation workflow
- [QA Engineer Agent](../plugins/v-qa/skills/qa-engineer/SKILL.md) - Testing workflow
