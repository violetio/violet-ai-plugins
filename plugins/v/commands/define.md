---
description: Run the Definition phase of V workflow
allowed-tools:
  - Glob
  - Grep
  - Read
  - Edit
  - Write
  - WebSearch
  - WebFetch
  - Task
  - AskUserQuestion
---

# Definition Phase

You are running the Definition phase of the V workflow.

## Prerequisites

Check for approved Discovery outputs:
1. `01-discovery/intake-final.md` - REQUIRED
2. `01-discovery/brief-final.md` - REQUIRED

If either is missing, inform user: "Discovery must be completed before Definition."

## Resume Handling

If `02-definition/` exists with draft files, present the Resume Chooser:

```
Found existing Definition drafts:

| # | Document | Status | Updated |
|---|----------|--------|---------|
| 1 | [filename] | [status from frontmatter] | [updated from frontmatter] |
| 2 | [filename] | [status from frontmatter] | [updated from frontmatter] |

Note: If `updated` field is missing, show file mtime with "(mtime)" suffix.

Which would you like to do?

[1/2/3] - Resume specific document
[S] - Show me a summary of each
[R] - Resume the most recently updated
[F] - Start fresh (archive existing drafts)
```

NEVER auto-resume. Always require explicit user selection.

## "Start Fresh" Handling

If user selects "Start fresh":

1. Show confirmation with file list:
   ```
   You selected "Start fresh". This will archive the following draft/review files:

   | File | Status | Will Be Moved To |
   |------|--------|------------------|
   | [filename] | [status] | archive/[filename]_[today's date].md |

   Note: Approved files (*-final.md) will NOT be archived.

   Proceed? [Y] Yes, archive and start fresh  [N] Cancel
   ```

2. If confirmed:
   - Create `02-definition/archive/` if not exists
   - Move ONLY files with status: draft or status: review
   - NEVER move files with status: approved
   - Add date suffix using archive action date: `{original-name}_{YYYY-MM-DD}.md`

3. Proceed with fresh requirements-v1.md

## Phase Flow

### Phase 6: Requirements

1. Read Discovery outputs (brief-final.md, intake-final.md)
2. Invoke the requirements-analyst skill
3. Generate requirements-v1.md with `updated` field
4. Facilitate user review loop
5. Iterate until approved
6. Create requirements-final.md (clean, no feedback sections)

### Phase 7: Experience Design

1. Read requirements-final.md + brief-final.md
2. Invoke the experience-designer skill
3. Generate experience-v1.md with Persona x Surface matrix, `updated` field
4. Facilitate user review loop
5. Iterate until approved
6. Create experience-final.md (clean, no feedback sections)

### Phase 8: Feasibility Analysis

1. Read requirements-final + experience-final + codebase context
2. Invoke architect skill (or analyze codebase)
3. Generate feasibility-v1.md with Basis of Estimate + Test Coverage, `updated` field
4. Handle any blockers (pause and discuss with user)
5. Facilitate user review loop
6. Create feasibility-final.md (clean, no feedback sections)

### Phase 9: Gate Check

Verify all criteria met:

```
## Definition Gate Check

✓ Requirements finalized
  - Functional: X requirements (P0: A, P1: B, P2: C)
  - Non-functional: Y requirements
  - Success criteria: Z defined
  - Conflicts resolved: N

✓ Experience Design finalized
  - Personas: X covered
  - Surfaces: Y designed
  - Journeys: Z mapped
  - Flows: A documented
  - Wireframes: B screens
  - API specs: C endpoints

✓ Feasibility Analysis approved
  - Approach: [summary]
  - Blockers: 0
  - Risks: X (accepted)
  - Effort: [size] (confidence: [level])
  - Test coverage: X% existing, Y% to add

Definition is complete. Ready to proceed to Design?
[Yes, proceed] [No, continue iterating]
```

## Blocker Handling

When feasibility analysis finds a blocker:

```
┌─────────────────────────────────────────────────────────────┐
│ BLOCKER IDENTIFIED                                          │
├─────────────────────────────────────────────────────────────┤
│ Description: [What the blocker is]                          │
│                                                             │
│ Impact: [What this prevents us from doing]                  │
│                                                             │
│ Root Cause: [Why this is a blocker]                         │
├─────────────────────────────────────────────────────────────┤
│ OPTIONS:                                                    │
│                                                             │
│ 1. Adjust Requirements                                      │
│    → [Specific change to requirements]                      │
│    → Impact: [What we lose/change]                          │
│                                                             │
│ 2. Adjust Experience Design                                 │
│    → [Specific change to UX]                                │
│    → Impact: [What changes for users]                       │
│                                                             │
│ 3. Accept Risk and Proceed                                  │
│    → Risk: [What could go wrong]                            │
│    → Mitigation: [How we'd handle it]                       │
│                                                             │
│ 4. Investigate Further                                      │
│    → Question: [What we need to learn]                      │
│    → Method: [How to investigate]                           │
└─────────────────────────────────────────────────────────────┘
```

WAIT for user decision before proceeding.

## Directory Structure Created

```
{brain-repo}/product/specs/YYYY-MM-DD-{feature-slug}/
├── 01-discovery/
│   ├── intake-final.md           ✓ From Discovery (required)
│   ├── brief-final.md            ✓ From Discovery (required)
│   └── materials/
├── 02-definition/
│   ├── requirements-v1.md        ← First requirements draft
│   ├── requirements-final.md     ← Approved requirements
│   ├── experience-v1.md          ← First experience design draft
│   ├── experience-final.md       ← Approved experience design
│   ├── feasibility-v1.md         ← First feasibility analysis
│   ├── feasibility-final.md      ← Approved feasibility
│   ├── materials/                ← Wireframes, diagrams
│   └── archive/                  ← Old drafts if "start fresh"
└── 03-design/                    ← Created in next phase
```
