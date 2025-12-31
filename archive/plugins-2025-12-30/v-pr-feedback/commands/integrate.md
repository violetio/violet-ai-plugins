---
name: integrate
description: "STAGE 2: Analyze patterns and integrate learnings into violet-brain"
---

# PR Feedback Integrate Command

> Analyze patterns from PR extraction and propose brain updates.

**Stage**: STAGE 2 (Learn)

---

## Prerequisites

- Completed extraction from `/v-pr-feedback:extract`
- User approved Gate 1 (extraction accuracy)
- Extraction file exists: `violet-brain/pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md`

## Execution

### Step 1: Pattern Analysis

**Input**: Read extraction file(s) from STAGE 1

**Questions to answer**:
1. Is this issue recurring? (Check past extractions)
2. Could this have been caught earlier? (Pre-PR checklist? Agent review?)
3. Is there a pattern to document? (Common mistake? Best practice?)
4. What would have prevented this? (Better guidance? Automated check?)

**Search for patterns**:
```bash
# Find similar issues in past PRs
grep -r "{issue_type}" violet-brain/pr-learnings/{repo}/

# Count frequency of issue types
grep -r "Type Error" violet-brain/pr-learnings/{repo}/ | wc -l

# Check if already documented
grep -r "{pattern_name}" violet-brain/patterns/
```

**Pattern threshold**: If an issue appears **2+ times**, document it.

### Step 2: Identify Update Targets

Based on patterns found, determine what to update:

| Pattern Type | Update Target |
|--------------|---------------|
| **Pre-PR preventable issue** | `patterns/pr-quality.md` → Pre-PR Checklist |
| **Common code quality issue** | `patterns/code-review-patterns.md` |
| **Missed in agent review** | `agents/references/tech-lead.md` |
| **Security vulnerability** | `agents/references/security-reviewer.md` + `standards/sec-priv/code-security.md` |
| **Process inefficiency** | `patterns/pr-quality.md` → Troubleshooting |
| **Documentation gap** | Repo-specific docs |

### Step 3: Generate Proposed Updates

For each target file, create a proposed change:

```markdown
## Proposed Update: {file_path}

**Reason**: PR #{number} had {issue_description}. This is the {N}th occurrence in {time_period}.

**Change**:
```diff
{before_context}
+ {new_content}
{after_context}
```

**Attribution**: Learned from `pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md`
```

### Step 4: Present Gate 2

Show diffs for ALL proposed updates, grouped by file:

```
✅ Analyzed patterns from PR #{number}
✅ Generated proposed brain updates

Proposed Changes ({N} files):

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. {file_path_1}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{diff_1}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2. {file_path_2}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

{diff_2}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚦 GATE 2: Approve Changes

Review the diffs above. Approve these brain updates?

[✓] Approve - Commit all changes to brain
[✗] Modify - Edit diffs before committing
[⊗] Reject - Skip learning integration (don't update brain)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5: Handle Gate 2 Response

**User chooses Approve** (✓):
1. Apply all proposed updates using Edit tool
2. Commit with attribution message:
   ```
   learn(pr-feedback): integrate patterns from PR #{number}

   Learned from {repo} PR #{number} review:
   - {change_1}
   - {change_2}
   - {change_3}

   Pattern: {pattern_description} appeared {N}x

   Source: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md
   ```
3. Update aggregated learnings: `pr-learnings/{repo}/learnings.md`
4. Proceed to STAGE 3 → Offer to invoke `/v-pr-feedback:fix`

**User chooses Modify** (✗):
- Ask: "Which files would you like to modify?"
- Options: User provides feedback → Regenerate proposals, or user edits files
- Re-prompt Gate 2 with updated diffs

**User chooses Reject** (⊗):
- Confirm: "Learning integration skipped. Proceed to fix code?"
- If yes → Skip to STAGE 3 → Invoke `/v-pr-feedback:fix`
- If no → Stop, user can resume later

---

## Update Targets Reference

### patterns/pr-quality.md
- **When**: Pre-PR preventable issues, process improvements
- **Sections**: Pre-PR Checklist, Troubleshooting, Quality Standards
- **Example**: Add "Check || vs ??" to checklist

### patterns/code-review-patterns.md
- **When**: Common code quality issues, architectural anti-patterns
- **Include**: Context (languages/stacks), problem, example, impact, solution
- **Example**: Document nullish coalescing bug pattern

### agents/references/tech-lead.md
- **When**: Issues missed in tech-lead review
- **Sections**: Review Criteria, Review Process, Common Pitfalls
- **Example**: Add "Verify import order" to criteria

### agents/references/security-reviewer.md
- **When**: New security vulnerabilities discovered
- **Sections**: Security Checklist, Common Vulnerabilities, Blocking Criteria
- **Example**: Add unvalidated redirect detection

### standards/sec-priv/code-security.md
- **When**: Security violations that should be codified
- **Include**: Vulnerability description, bad example, standard, detection method
- **Example**: Document redirect validation requirement

---

## Multi-PR Session Learning

When processing multiple PRs in one session:

1. **Batch Analysis**: Analyze patterns ACROSS all extractions
2. **Consolidated Proposals**: Generate ONE set of brain updates
3. **Single Gate 2**: Present consolidated diffs once
4. **Single Commit**: One learning commit with all PRs referenced

**Benefits**:
- Better signal (patterns clearer across multiple PRs)
- Reduced noise (don't document one-offs)
- Fewer commits (consolidated brain update)

---

## Attribution Standards

Every integration MUST include:

### In Commit Messages
```
learn(pr-feedback): integrate patterns from PR #{number}

Learned from {repo} PR #{number} review:
- {changes}

Pattern: {description} appeared {N}x

Source: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md
```

### In Updated Files
```markdown
- [ ] **Check nullish coalescing** - Use ?? not ||
  - *(Learned from PR #1703)*
```

### In pr-learnings/learnings.md
```markdown
## Pattern: {name}

**First Seen**: PR #{first} ({date})
**Frequency**: {N} occurrences (PRs #{list})
**Status**: Documented in {file_path}
**Resolution**: {what was done}
```

---

## Quality Guidelines

### When to Integrate

✅ **DO integrate when**:
- Pattern appears 2+ times
- Issue could have been prevented with better guidance
- New vulnerability pattern discovered
- Process inefficiency identified

❌ **DON'T integrate when**:
- One-off issue specific to one PR
- Already documented in brain
- Requires product-specific context (belongs in product repo)
- User error, not system gap

### Detail Level

**Too vague** ❌: `- [ ] Check for bugs`

**Too specific** ❌: `- [ ] Check line 23 of specific_file.ts`

**Just right** ✅:
```markdown
- [ ] **Check nullish coalescing** - Use ?? not ||
  - Common issue: Using || prevents page 0 requests
  - How to detect: Search for `page ||` or `data ||`
  - How to fix: Replace with `??` for proper null handling
```

---

## Success Criteria

- [ ] Patterns identified from extraction
- [ ] Update targets determined
- [ ] Proposed changes generated with before/after diffs
- [ ] Gate 2 presented with all diffs
- [ ] User approval received
- [ ] Changes committed with clear attribution
- [ ] Aggregated learnings updated

---

## Next Steps

After Gate 2 approval:
- Offer to run `/v-pr-feedback:fix` for STAGE 3 (code fixes)

---

*Command Version: 1.0.0*
