---
description: Feature development workflow - Discovery through Review
allowed-tools:
  - Glob
  - Grep
  - Read
  - Edit
  - Write
  - Bash
  - WebSearch
  - WebFetch
  - Task
  - AskUserQuestion
---

# Feature Development Workflow

You are running the V feature development workflow. This guides a feature from idea through implementation.

## Workflow Phases

```
Discovery → Definition → Design → Development → Review
    ↓           ↓          ↓           ↓           ↓
  Gate 1     Gate 2     Gate 3    (iterative)   Gate 4
```

## Phase 1: Discovery (Current)

**Purpose**: Understand the work before designing solutions.

### Step 1.1: Context Check

First, determine where we are:

1. **Check for existing spec folder**:
   - Look for `product/specs/YYYY-MM-DD-*` folders
   - If found, ask: "Found existing spec at [path]. Resume this or start new?"

2. **Check if we're in a brain repo or code repo**:
   - Brain repo indicators: `/product/specs/`, `/patterns/`, `*-brain` in path
   - Code repo indicators: `/src/`, `/pages/`, `package.json`

3. **If in code repo, ask for brain repo location**:
   ```
   You're in [repo name] (code repo).
   Discovery should be saved to a brain repo.

   Which brain repo?
   - prism-brain
   - beam-brain
   - violet-brain
   - Other: ___
   ```

### Step 1.2: Intake Mode Selection

Ask the user:
```
How would you like to complete the intake?

1. **Guide me** - I'll ask you questions and capture your answers
2. **I'll fill it out** - I'll create the template, you edit directly

You can switch modes at any time.
```

### Step 1.3: Intake Questions (if guided mode)

Ask these systematically:

**WHAT (the work)**
- What are you trying to build, change, or fix?
- What type of work is this? (Problem/Solution, Opportunity/Enablement, Experience/Journey, Capability/Feature, Integration/Connection, Compliance/Requirement)

**WHO (stakeholders)**
- Who is impacted? (Channel, Merchant, Shopper, Violet Team, Beam Channel)
- For each: What's the impact or objective?

**WHY (justification)**
- Why does this matter? What's the business justification?
- What happens if we don't do this?

**WHERE (location/scope)**
- Which products? (Prism, Beam, Relay, VioletConnect, Ecom Integrations)
- Which platforms? (Shopify, BigCommerce, etc.)
- Which systems? (VioletDashboard, ChannelDashboard, etc.)

**WHEN (timing/urgency)**
- Is there a deadline or timeline pressure?
- Is this blocking other work?

**HOW (constraints/approach)**
- Are there constraints we must follow?
- Do you have existing ideas about how this should work?
- What's explicitly OUT of scope?

**EXPLORATION GUIDANCE**
- Specific codebases or patterns to examine?
- Anything to explicitly avoid?
- Specific questions for exploration?

### Step 1.4: Create Spec Directory

After intake, create the spec structure:

```
{brain-repo}/product/specs/YYYY-MM-DD-{feature-slug}/
├── 01-discovery/
│   └── intake-v1.md
└── README.md
```

### Step 1.5: Generate intake-v1.md

Create the intake document with all captured information, including:
- YAML frontmatter (id, feature, phase, document, version, status, created, updated, domains, stakeholders, author)
- All sections from intake questions
- YOUR FEEDBACK sections for user to provide input

### Step 1.6: Existing Work Check

Search for related work:
1. Brain repo - related specs
2. Notion - related pages (if MCP available)
3. Linear - related issues (if MCP available)

Present bibliography with Use/Ignore/Exclude options.

### Step 1.7: Exploration

Launch explorers to gather context:
1. **Codebase Explorer** - Similar features, patterns, constraints
2. **Internal Docs Explorer** - Brain repos, Notion
3. **External Research Explorer** - Only if needed (user confirms first)

### Step 1.8: Generate brief-v1.md

Synthesize intake + exploration into Discovery Brief:
- Work frame
- Stakeholder impact
- Context
- Business justification
- Scope (in/out)
- Dependencies
- Assumptions
- Open questions (with disposition)
- Exploration coverage
- Bibliography

Include YOUR FEEDBACK sections.

### Step 1.9: Review Loop

```
Generate document → User reviews → Claude responds to feedback →
User marks approved/needs changes → Iterate → Create *-final.md
```

### Step 1.10: Gate Check

Verify all criteria:
1. ✓ Intake - APPROVED
2. ✓ Work Frame - SELECTED
3. ✓ Stakeholders - IDENTIFIED with impacts
4. ✓ Business Justification - APPROVED
5. ✓ Scope - APPROVED (in/out defined)
6. ✓ Dependencies - MAPPED
7. ✓ Assumptions - REVIEWED
8. ✓ Open Questions - ADDRESSED or deferred
9. ✓ Bibliography - REVIEWED
10. ✓ Exploration Coverage - DOCUMENTED
11. ✓ Open Questions Disposition - LOGGED

Display gate check and ask:
```
Discovery is complete. Ready to proceed to Definition?
[Yes, proceed] [No, continue iterating]
```

---

## Phase 2: Definition

When Discovery is complete, proceed to Definition phase.

Use: Skill v:define

---

## Phase 3-5: Design, Development, Review

(Not yet implemented - will be added in future versions)
