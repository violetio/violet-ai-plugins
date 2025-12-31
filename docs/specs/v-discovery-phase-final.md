# V Plugin: Discovery Phase Design

> **Status**: APPROVED
> **Version**: Final
> **Approved**: 2025-12-26
> **History**: v1 (review) → v2 (review) → v3 (review) → final

---

## What is Discovery?

**Purpose**: Understand the work before designing solutions.

**Inputs**:
- User's initial request/idea
- Intake document (generated from conversation or direct editing)
- Existing work (brain repo specs, Notion, Linear)
- Codebase context (if implementation will happen)

**Outputs**:
- Approved intake document
- Discovery Brief document (structured, with user feedback incorporated)
- Approved understanding of the strategy and scope of the work

**Gate**: User explicitly approves Discovery Brief before proceeding to Definition.

---

## Entry Points

### Option A: Via /v (Auto-Detect)
```
User: /v I want to add merchant pre-registration to the dashboard

Claude: I detected this as feature work. Proceed with /v:feature?
        [Yes] [No, let me clarify]

User: Yes

Claude: Starting Discovery phase...
        Would you like me to guide you through intake questions,
        or would you prefer to fill out the intake document directly?
```

### Option B: Via /v:feature (Explicit)
```
/v:feature Add merchant pre-registration
```
→ Starts with Discovery phase, offers intake mode choice

### Option C: Via /v:discovery (Granular)
```
/v:discovery
```
→ Runs only Discovery phase (for iterating on an existing discovery)

---

## Directory Structure

### When Directories Are Created

Directories are created AFTER intake conversation is complete and brain repo is confirmed (not before).

### Structure

```
{brain-repo}/product/specs/YYYY-MM-DD-{feature-slug}/
├── README.md                   # Human-readable overview (for GitHub browsing)
├── index.md                    # Machine-readable metadata + links to all phases
├── 01-discovery/
│   ├── intake-v1.md           # First draft of intake
│   ├── intake-final.md        # Approved intake (no feedback sections)
│   ├── brief-v1.md            # First draft of discovery brief
│   ├── brief-v2.md            # Second draft (if needed)
│   ├── brief-final.md         # Approved brief (no feedback sections)
│   └── materials/             # Screenshots, diagrams, reference docs
├── 02-definition/
│   ├── requirements-v1.md
│   ├── requirements-final.md
│   ├── experience-v1.md
│   ├── experience-final.md
│   └── materials/
├── 03-design/
│   └── ...
└── ...
```

### Naming Convention

**Format**: `YYYY-MM-DD-{feature-slug}`

Examples:
- `2024-01-15-merchant-pre-registration`
- `2024-01-16-channel-webhook-improvements`
- `2024-01-16-02-beam-sync-optimization` (if second spec that day)

### Frontmatter Standard

All documents include YAML frontmatter:

```yaml
---
id: 2024-01-15-merchant-pre-registration
feature: merchant-pre-registration
phase: discovery
document: intake | brief
version: 1
status: draft | review | approved
created: 2024-01-15
updated: 2024-01-16
domains: [prism, violet-dashboard]
work_frame: capability
stakeholders: [merchant, channel]
author: brandon
sources_checked:
  brain_repos: [prism-brain]
  notion: checked
  linear: checked
---
```

### Brain Repo Selection

After domain selection in intake, confirm with user:
```
Based on the domains you selected (Prism, VioletDashboard), this spec should live in:

Recommended: **prism-brain**

Create at: prism-brain/product/specs/2024-01-15-merchant-pre-registration/

Is this correct? Or should it go elsewhere?
```

### When Domain is Unclear

If work doesn't map to an existing brain repo:

**Location**: `violet-brain/inbox/YYYY-MM-DD-{feature-slug}/`

**Frontmatter annotation**:
```yaml
---
inbox: true
needs_processing: true
potential_new_product: true  # if applicable
notes: |
  This discovery doesn't fit existing product repos.
  May require:
  - New brain repo creation
  - Assignment to existing product
  - Further scoping to determine ownership
---
```

---

## Complete Discovery Flow

### Master Timeline

```
PHASE 1: INTAKE
├─ 1.1 Entry point (/v, /v:feature, or /v:discovery)
├─ 1.2 User chooses mode: "Guide me" or "I'll fill it out"
├─ 1.3 Complete intake (via conversation or document)
│       - WHAT: the work, work type
│       - WHO: stakeholders + impact per stakeholder
│       - WHY: business justification
│       - WHERE: domains, brain repo
│       - WHEN: timing/urgency
│       - HOW: constraints, scope
│       - EXPLORATION GUIDANCE: specific codebases, patterns, questions
├─ 1.4 Brain repo confirmation
├─ 1.5 CREATE directory + intake-v1.md
│
│   ════════════════════════════════════════════════
│   📁 FILE CREATED: intake-v1.md
│   ════════════════════════════════════════════════
│
├─ 1.6 USER REVIEWS intake-v1.md
├─ 1.7 Claude responds to feedback
├─ 1.8 Iterate until approved
└─ 1.9 CREATE intake-final.md (clean, no feedback sections)

════════════════════════════════════════════════════════════

PHASE 2: EXISTING WORK CHECK
├─ 2.1 Search brain repo for related specs
├─ 2.2 Search Notion for related pages/docs
├─ 2.3 Search Linear for related issues/projects
├─ 2.4 Present bibliography to user
├─ 2.5 User marks Use/Ignore/Exclude for each reference
└─ 2.6 UPDATE intake-final.md with bibliography

════════════════════════════════════════════════════════════

PHASE 3: EXPLORATION
├─ 3.1 Launch Codebase Explorer (patterns, similar features)
├─ 3.2 Launch Internal Docs Explorer (brain repos, Notion)
├─ 3.3 [Conditional] Launch External Research Explorer
└─ 3.4 Synthesize findings

════════════════════════════════════════════════════════════

PHASE 4: DISCOVERY BRIEF
├─ 4.1 Generate brief-v1.md from intake + exploration
│
│   ════════════════════════════════════════════════
│   📁 FILE CREATED: brief-v1.md
│   ════════════════════════════════════════════════
│
├─ 4.2 USER REVIEWS brief-v1.md
├─ 4.3 Claude responds to feedback
├─ 4.4 CREATE brief-v2.md (or next version)
├─ 4.5 Iterate until all sections approved
└─ 4.6 CREATE brief-final.md (clean, no feedback sections)

════════════════════════════════════════════════════════════

PHASE 5: GATE CHECK
├─ 5.1 Verify all criteria met
└─ 5.2 Proceed to Definition (or continue iterating)
```

---

## Intake Process

### Mode Selection

At the start of discovery, offer the user a choice:

```
How would you like to complete the intake?

1. **Guide me** - I'll ask you questions and capture your answers
2. **I'll fill it out** - I'll create the template, you edit directly

You can switch modes at any time.
```

### Option A: Conversational Intake (Claude Guides)

Claude asks questions systematically:

```
INTAKE CONVERSATION STRUCTURE

1. WHAT (the work)
   "What are you trying to build, change, or fix?"
   "What type of work is this?"
   - Problem/Solution (fixing something broken)
   - Opportunity/Enablement (new capability)
   - Experience/Journey (UX improvement)
   - Capability/Feature (new functionality)
   - Integration/Connection (platform/API work)
   - Compliance/Requirement (regulatory/security)

2. WHO (stakeholders)
   "Who is impacted by this work?"
   - Channel
   - Merchant
   - Shopper
   - Violet Team
   - Beam Channel

   [For each selected]:
   "What's the impact or objective for [stakeholder]?"

3. WHY (justification)
   "Why does this matter? What's the business justification?"
   "What happens if we don't do this?"

4. WHERE (location/scope)
   "Which products does this touch?" (Prism, Beam, Relay, etc.)
   "Which platforms?" (Shopify, BigCommerce, etc.)
   "Which systems?" (VioletDashboard, ChannelDashboard, etc.)

5. WHEN (timing/urgency)
   "Is there a deadline or timeline pressure?"
   "Is this blocking other work?"

6. HOW (constraints/approach)
   "Are there constraints or requirements we must follow?"
   "Do you have existing ideas about how this should work?"
   "What's explicitly OUT of scope?"
   "Are there specific codebases or patterns I should look at?"
   "Anything I should avoid exploring?"
   "Any specific questions you want answered during exploration?"

7. PROJECT MANAGEMENT
   "Is there a Linear issue or project for this?"
   "Any Notion pages I should reference?"
```

After conversation, Claude generates `intake-v1.md` for review.

### Option B: Direct Document Intake (User Drives)

Claude creates `intake-v1.md` with empty sections and YOUR FEEDBACK blocks. User fills in directly, saves, and notifies Claude to review.

### Intake Document Template

```markdown
# Discovery Intake: [Feature Name]

---
id: YYYY-MM-DD-{feature-slug}
feature: {feature-slug}
phase: discovery
document: intake
version: 1
status: draft
created: YYYY-MM-DD
---

## WHAT: The Work

**Description:**
[What are you trying to build, change, or fix?]

**Work Type:**
- [ ] Problem/Solution
- [ ] Opportunity/Enablement
- [ ] Experience/Journey
- [ ] Capability/Feature
- [ ] Integration/Connection
- [ ] Compliance/Requirement

---

## WHO: Stakeholders

**Who is impacted?** (Check all that apply)

- [ ] Channel
  **Impact/Objectives:**
  [How are channels affected?]

- [ ] Merchant
  **Impact/Objectives:**
  [How are merchants affected?]

- [ ] Shopper
  **Impact/Objectives:**
  [How are shoppers affected?]

- [ ] Violet Team
  **Impact/Objectives:**
  [How is the Violet team affected?]

- [ ] Beam Channel
  **Impact/Objectives:**
  [How are beam channels affected?]

---

## WHY: Business Justification

**Why does this matter?**
[Business justification]

**What happens if we don't do this?**
[Consequences of inaction]

---

## WHERE: Domains & Location

**Products:**
- [ ] Prism
- [ ] Beam
- [ ] Relay
- [ ] VioletConnect
- [ ] Ecom Integrations

**Platforms:**
- [ ] Shopify
- [ ] BigCommerce
- [ ] WooCommerce
- [ ] Squarespace
- [ ] Other: ___

**Systems:**
- [ ] VioletDashboard
- [ ] ChannelDashboard
- [ ] VioletConnect
- [ ] InternalDashboard
- [ ] API Services: ___

**Brain Repo:** [To be confirmed]

---

## WHEN: Timing

**Is there a deadline?**
[Timeline or "No specific deadline"]

**Is this blocking other work?**
[Dependencies or "No blockers"]

---

## HOW: Constraints & Scope

**Constraints or requirements:**
[Must-follow constraints]

**Existing ideas about approach:**
[Any preliminary thinking]

**Explicitly OUT of scope:**
[What this is NOT]

---

## Exploration Guidance (Optional)

**Specific codebases or repos to examine:**
[e.g., "Look at VioletDashboard/pages/merchants for similar patterns"]

**Specific patterns or implementations to reference:**
[e.g., "The channel onboarding flow in ChannelDashboard is similar"]

**Files or areas to explicitly AVOID:**
[e.g., "Don't look at the legacy merchant module - it's being deprecated"]

**Specific questions for explorers to answer:**
[e.g., "How does the current pre-registration flow handle OAuth?"]

---

## Project Management

**Linear:**
- [ ] Issue: ___
- [ ] Project: ___
- [ ] No Linear yet (create after discovery)
- [ ] Skip Linear

**Notion:**
- [ ] Related pages: ___
- [ ] No Notion pages

---

## Approval

- [ ] WHAT approved
- [ ] WHO approved
- [ ] WHY approved
- [ ] WHERE approved
- [ ] WHEN approved
- [ ] HOW approved
- [ ] Exploration Guidance reviewed
- [ ] Project Management approved

**Ready to proceed to Existing Work Check?** [ ] Yes [ ] No - needs revision
```

---

## Existing Work Check

### Search Order

1. **Brain repo** - Related specs in the confirmed brain repo
2. **Notion** - Related pages and docs
3. **Linear** - Related issues and projects

### Tool Availability

```
Searching for related work...

**Brain Repos**: ✓ Found 2 related specs
**Notion**: ⚠️ Could not connect (MCP server not configured)
**Linear**: ✓ Found 1 related issue

---

**Note**: Some sources were unavailable:
- Notion: Not configured. [Configure Notion MCP] or [Skip]

Do you want to:
1. Continue with available sources
2. Pause to configure missing tools
3. Manually add references I should know about
```

### Fallback Behavior

| Tool | If Unavailable |
|------|----------------|
| Brain repo | ERROR - Required, cannot proceed |
| Notion | WARN - Continue without, note in bibliography |
| Linear | WARN - Continue without, note in bibliography |

### Bibliography Presentation

```markdown
## Existing Work Found

I found related material:

| # | Source | Type | Location | Status |
|---|--------|------|----------|--------|
| 1 | Merchant Pre-Reg Requirements | Spec | prism-brain/product/specs/... | [ ] Use / [ ] Ignore / [ ] Exclude |
| 2 | Pre-Registration Planning | Notion | notion.so/... | [ ] Use / [ ] Ignore / [ ] Exclude |
| 3 | PRISM-1234 | Linear | linear.app/... | [ ] Use / [ ] Ignore / [ ] Exclude |

**Your notes on references:**
[Add context about why to use/ignore/exclude specific references]
```

### Handling "Start Fresh"

If user wants to start fresh with existing work found:

```
How should I treat this existing work?

1. **Build on it** - Use as foundation, extend/refine
2. **Reference only** - Include in bibliography, start fresh conceptually
3. **Explicitly exclude** - Do not include (will be documented)
4. **Let me review first** - Show me the content before I decide
```

---

## Exploration

### Three Explorer Types

| Explorer | Focus | Always Run? | Tools |
|----------|-------|-------------|-------|
| **Codebase Explorer** | Similar features, patterns, constraints, dependencies | Yes | Glob, Grep, Read |
| **Internal Docs Explorer** | Brain repos, Notion | Yes | Read, WebFetch |
| **External Research Explorer** | Competitors, standards, external APIs | **Conditional** | WebSearch, WebFetch |

### Explorer Input

Each explorer receives:
- Approved intake document (including Exploration Guidance)
- Domain selection
- Bibliography decisions (use/ignore/exclude)

### Explorer Output Format

```markdown
## Exploration Findings

### Questions Answered

| Question | Answer | Source |
|----------|--------|--------|
| What similar features exist? | Merchant settings page | VioletDashboard/pages/merchants:45 |
| What patterns should we follow? | Redux + API proxy | CLAUDE.md |

### Questions Needing User Input

| Question | Why We Need This |
|----------|------------------|
| What's the business priority? | No roadmap docs found |

### Dependencies Identified

| Dependency | Type | Impact | Source |
|------------|------|--------|--------|
| Merchant API v2 | System | Must use new endpoints | codebase |
| Shopify OAuth | External | Must follow their spec | internal docs |

### Dependency Map

[This Feature]
    ├── depends on → Merchant API v2
    ├── depends on → Shopify OAuth
    └── notifies → Billing Service

### Patterns to Follow

- [Pattern 1] - [source file:line]

### Constraints Identified

- [Constraint 1]
```

### Conditional External Research

Launch external research IF:
1. User explicitly requests it, OR
2. Intake indicates external integration, OR
3. Internal explorers find no relevant patterns AND work type is Opportunity/Enablement or Integration/Connection

When triggered, ask user first:
```
I believe external research would help because [reason].
Should I search for: [specific topics]?
[Yes] [No] [Let me specify]
```

### Time Budget

Target: <10 minutes for exploration

| Activity | Time |
|----------|------|
| Intake review | ~30 sec |
| Codebase exploration | ~2-3 min |
| Internal docs exploration | ~2-3 min (parallel) |
| Synthesis | ~1-2 min |
| **Total** | **~5-6 min** |

If external research triggered: add ~2-3 min

**Guardrail**: If any explorer exceeds 5 minutes, report partial findings and ask user to continue or proceed.

---

## Discovery Brief

The Discovery Brief synthesizes intake + exploration into a reviewable document.

### Brief Structure

```markdown
# Discovery Brief: [Feature Name]

---
id: YYYY-MM-DD-{feature-slug}
feature: {feature-slug}
phase: discovery
document: brief
version: 1
status: draft
created: YYYY-MM-DD
work_frame: [selected type]
stakeholders: [list]
---

## Work Frame

**Type**: [Problem/Solution | Opportunity/Enablement | etc.]
**Summary**: [One sentence]

---

## Stakeholder Impact

[For each stakeholder from intake, with exploration findings added]

---

## Context

[Detailed context combining intake + exploration]

---

## Business Justification

[From intake + any exploration additions]

---

## Scope

**Included:**
- [Item]

**Excluded:**
- [Item]

---

## Dependencies

[From exploration, validated]

---

## Assumptions

| Assumption | Confidence | Needs Validation? |
|------------|------------|-------------------|
| [Item] | High/Med/Low | Yes/No |

---

## Codebase Context

[From exploration]

---

## Open Questions

| Question | Status | Answer/Owner | Date |
|----------|--------|--------------|------|
| [Question 1] | Answered | [Answer summary] | [Date] |
| [Question 2] | Deferred | [Owner name] | [Target date] |
| [Question 3] | Answered | [Answer summary] | [Date] |

**Summary:**
- Answered: X questions
- Deferred: Y questions (with owners assigned)

---

## Exploration Coverage

### What Was Checked

| Area | Source | Findings |
|------|--------|----------|
| [Codebase area 1] | [repo/path] | [What was found] |
| [Internal docs] | [Notion/Brain repo] | [What was found] |
| [External research] | [URLs] | [What was found] |

### What Was NOT Checked (and Why)

| Area | Reason Skipped | Impact |
|------|----------------|--------|
| [Area 1] | [Time constraints / Not relevant / etc.] | [Low/Med/High] |
| [Area 2] | [Reason] | [Impact] |

---

## Bibliography

[From existing work check]

---

## Approval

- [ ] All sections approved
- [ ] No outstanding feedback

**Ready for Definition?** [ ] Yes [ ] No
```

---

## Review Loop

Both intake and brief follow the same review pattern:

```
┌─────────────────────────────────────────┐
│  1. Generate document (v1)              │
└─────────────────┬───────────────────────┘
                  ▼
┌─────────────────────────────────────────┐
│  2. User reviews, adds feedback         │
└─────────────────┬───────────────────────┘
                  ▼
┌─────────────────────────────────────────┐
│  3. Claude responds to each feedback    │
│     item with answers + proposed        │
│     actions                             │
└─────────────────┬───────────────────────┘
                  ▼
┌─────────────────────────────────────────┐
│  4. User marks responses as             │
│     [x] Approved or [ ] Needs changes   │
└─────────────────┬───────────────────────┘
                  │
          ┌───────┴────────┐
          │                │
    All approved?    Needs changes?
          │                │
          ▼                ▼
┌──────────────┐    ┌─────────────────────┐
│ Create v2    │    │ Claude addresses    │
│ with changes │    │ (loop back)         │
└──────┬───────┘    └─────────────────────┘
       ▼
 (Repeat until document-final.md
  has no feedback sections)
```

**Key points:**
- Each version is preserved (v1, v2, v3...)
- Final version has NO feedback sections
- Versions serve as history

---

## Gate Criteria

### What Must Be True to Exit Discovery

1. ✓ Intake - APPROVED (all sections)
2. ✓ Work Frame - SELECTED
3. ✓ Stakeholders - IDENTIFIED with impacts
4. ✓ Business Justification - APPROVED
5. ✓ Scope - APPROVED (in/out defined)
6. ✓ Dependencies - MAPPED
7. ✓ Assumptions - REVIEWED
8. ✓ Open Questions - ADDRESSED or deferred (with disposition logged)
9. ✓ Bibliography - REVIEWED
10. ✓ Exploration Coverage - DOCUMENTED (what was checked + what was not)
11. ✓ Open Questions Disposition - LOGGED (answered, or deferred with owner/date)

### Gate Check Display

```
## Discovery Gate Check

✓ Intake finalized
✓ Work Frame selected
✓ Stakeholders identified
✓ Business Justification approved
✓ Scope approved
✓ Dependencies mapped
✓ Assumptions reviewed
✓ Open Questions addressed
  - Answered: X
  - Deferred: Y (with owner/date)
✓ Bibliography reviewed
✓ Exploration Coverage documented
  - Checked: [list of areas/repos explored]
  - Not checked: [list of areas skipped, with reason]

Discovery is complete. Ready to proceed to Definition?
[Yes, proceed] [No, continue iterating]
```

---

## Edge Cases

### 1. User starts in a code repo

```
You're in VioletDashboard (code repo).
Discovery should be saved to a brain repo.

Which brain repo?
- prism-brain
- beam-brain
- violet-brain
```

### 2. Discovery reveals multiple features

```
Discovery revealed this might be better as separate efforts:
1. [Feature A] - [scope]
2. [Feature B] - [scope]

Should we:
1. Continue as one combined feature
2. Split into separate discoveries
3. Focus on A first, defer B
```

### 3. User wants to skip discovery

```
I understand you want to skip to implementation.
Even brief discovery helps ensure alignment.

Options:
1. Quick discovery (5 min)
2. Skip (will be documented)
3. Let me explain why discovery matters
```

### 4. No brain repo for this domain

```
This doesn't map to an existing brain repo.

Default: violet-brain/inbox/ with processing annotations

Options:
1. Place in violet-brain/inbox (recommended)
2. Place in violet-brain/product/specs
3. Create new brain repo
```

### 5. Intake too sparse

```
The intake has minimal information.

Options:
1. Help me fill in more details (recommended)
2. Proceed with exploration (I'll ask questions)
3. This is intentionally minimal
```

### 6. Discovery takes too long

```
Exploration is taking longer than expected.
Status: [X] answered, [Y] pending

Options:
1. Continue
2. Stop here, proceed with current findings
3. Narrow scope, restart
```

---

## Summary

The Discovery phase ensures we understand work before designing solutions through:

1. **Two-mode intake** - Conversational or direct document editing
2. **Systematic questions** - Who/What/Why/When/Where/How + Exploration Guidance
3. **Existing work check** - Brain repo → Notion → Linear with graceful fallbacks
4. **Focused exploration** - Three explorer types with user guidance
5. **Iterative review** - Documents as feedback interfaces until approved
6. **Clear gates** - Explicit criteria before proceeding to Definition

**Key principle**: Documents are outputs of conversation, not inputs to fill out.
