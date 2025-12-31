---
description: Run only the Discovery phase (for iterating on existing discovery)
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

# Discovery Phase (Granular)

You are running ONLY the Discovery phase. Use this to iterate on an existing discovery or start a new one.

## Resume Check

First, check for existing discovery work:

1. Look for `01-discovery/` folders in current directory or `product/specs/*/01-discovery/`
2. If found, present the Resume Chooser:

```
Found existing Discovery work:

| # | Document | Status | Updated |
|---|----------|--------|---------|
| 1 | intake-v2.md | review | 2025-01-10 |
| 2 | brief-v1.md | draft | 2025-01-08 |

Which would you like to do?

[1/2] - Resume specific document
[S] - Show me a summary of each
[R] - Resume the most recently updated
[F] - Start fresh (archive existing drafts)
```

**Rules:**
- NEVER auto-resume if multiple candidates exist
- Always require explicit user selection
- Show dates from frontmatter `updated` field; if missing, use file mtime with "(mtime)" note
- "Start fresh" requires confirmation

## Start Fresh Confirmation

If user selects "Start fresh":

```
You selected "Start fresh". This will archive the following draft/review files:

| File | Status | Will Be Moved To |
|------|--------|------------------|
| intake-v2.md | review | archive/intake-v2_2025-01-15.md |
| brief-v1.md | draft | archive/brief-v1_2025-01-15.md |

Note: Approved files (*-final.md) will NOT be archived.

Proceed? [Y] Yes, archive and start fresh  [N] Cancel
```

Archive rules:
- ONLY archive files with `status: draft` or `status: review`
- NEVER archive files with `status: approved`
- Archive location: `01-discovery/archive/`
- Archive naming: `{original-name}_{archive-action-date}.md`

## Discovery Flow

### If Starting Fresh or No Existing Work

Follow the full Discovery flow:

1. **Context Check** - Brain repo vs code repo
2. **Intake Mode Selection** - Guide me vs I'll fill it out
3. **Intake Questions** - WHAT/WHO/WHY/WHERE/WHEN/HOW
4. **Create Spec Directory** - `YYYY-MM-DD-{feature-slug}/01-discovery/`
5. **Generate intake-v1.md** - With YOUR FEEDBACK sections
6. **Existing Work Check** - Brain repo, Notion, Linear
7. **Exploration** - Codebase, Internal Docs, External Research (if needed)
8. **Generate brief-v1.md** - Synthesize findings
9. **Review Loop** - Iterate until approved
10. **Gate Check** - All 11 criteria

### If Resuming

1. Read the document being resumed
2. Check for existing feedback in YOUR FEEDBACK sections
3. If feedback exists, respond to it and generate next version
4. If no feedback, ask user what they want to change
5. Continue review loop until approved

## Intake Document Template

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
updated: YYYY-MM-DD
domains: []
work_frame:
stakeholders: []
author: claude
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

[For each stakeholder: checkbox, impact/objectives]

---

## WHY: Business Justification

**Why does this matter?**
[Business justification]

**What happens if we don't do this?**
[Consequences of inaction]

---

## WHERE: Domains & Location

[Products, Platforms, Systems checkboxes]

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

## Exploration Guidance

**Specific codebases or repos to examine:**
[e.g., "Look at VioletDashboard/pages/merchants for similar patterns"]

**Files or areas to explicitly AVOID:**
[e.g., "Don't look at the legacy merchant module"]

**Specific questions for explorers to answer:**
[e.g., "How does the current pre-registration flow handle OAuth?"]

---

## Project Management

**Linear:** [Issue/Project links or "No Linear yet"]
**Notion:** [Related pages or "No Notion pages"]

---

## Approval

- [ ] WHAT approved
- [ ] WHO approved
- [ ] WHY approved
- [ ] WHERE approved
- [ ] WHEN approved
- [ ] HOW approved
- [ ] Exploration Guidance reviewed

**Ready to proceed to Existing Work Check?** [ ] Yes [ ] No
```

## Discovery Brief Template

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
updated: YYYY-MM-DD
work_frame: [selected type]
stakeholders: [list]
domains: []
author: claude
---

## Work Frame

**Type**: [Problem/Solution | Opportunity/Enablement | etc.]
**Summary**: [One sentence]

---

## Stakeholder Impact

[For each stakeholder with exploration findings]

---

## Context

[Detailed context from intake + exploration]

---

## Business Justification

[From intake + exploration additions]

---

## Scope

**Included:**
- [Item]

**Excluded:**
- [Item]

---

## Dependencies

[From exploration]

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
| [Question] | Answered/Deferred | [Answer or Owner] | [Date] |

---

## Exploration Coverage

### What Was Checked
| Area | Source | Findings |
|------|--------|----------|
| [Area] | [repo/path] | [What was found] |

### What Was NOT Checked (and Why)
| Area | Reason Skipped | Impact |
|------|----------------|--------|
| [Area] | [Reason] | [Low/Med/High] |

---

## Bibliography

[From existing work check]

---

## Approval

- [ ] All sections approved
- [ ] No outstanding feedback

**Ready for Definition?** [ ] Yes [ ] No
```

## Gate Criteria

Before proceeding to Definition, verify:

1. ✓ Intake - APPROVED
2. ✓ Work Frame - SELECTED
3. ✓ Stakeholders - IDENTIFIED with impacts
4. ✓ Business Justification - APPROVED
5. ✓ Scope - APPROVED (in/out defined)
6. ✓ Dependencies - MAPPED
7. ✓ Assumptions - REVIEWED
8. ✓ Open Questions - ADDRESSED or deferred (with owner/date)
9. ✓ Bibliography - REVIEWED
10. ✓ Exploration Coverage - DOCUMENTED
11. ✓ Open Questions Disposition - LOGGED
