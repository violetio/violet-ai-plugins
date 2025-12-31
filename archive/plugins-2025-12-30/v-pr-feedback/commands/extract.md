---
name: extract
description: "STAGE 0+1: Capture raw PR data and extract 13-category taxonomy"
---

# PR Feedback Extract Command

> Capture raw PR data and extract feedback using 13-category taxonomy.

**Stages**: STAGE 0 (Capture Raw) + STAGE 1 (Extract)

---

## Input Required

- **PR Number**: e.g., #1703
- **Repository**: Auto-detected from current directory or specified

## Execution

### STAGE 0: Capture Raw PR Data

**Purpose**: Preserve ALL PR data verbatim before any processing.

```bash
# Fetch complete PR details
gh pr view {number} --json number,title,body,state,url,createdAt,closedAt,mergedAt,author,reviewDecision,reviews,comments,commits,files,labels,assignees,reviewRequests,timelineItems
```

**Save to**: `violet-brain/pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-raw.md`

**Use template**: `violet-brain/pr-learnings/_templates/raw-template.md`

**Include verbatim** (DO NOT interpret or summarize):
1. PR Metadata: Number, title, author, dates, status, review decision
2. PR Description: Complete body text
3. Linked Issues: Linear (INT-XXX) and GitHub issues
4. Changed Files: List with additions/deletions
5. Commits: All commit messages
6. Review Comments: ALL reviews with inline comments
7. Conversation Comments: All PR comments with threads
8. Bot Feedback: Graphite Bot, linters, formatters
9. CI/CD Results: Test results, build logs
10. Check Runs: Status of all automated checks
11. Labels, Assignees, Reviewers: Complete metadata
12. Timeline: Complete event history

### STAGE 1: Extract Using 13-Category Taxonomy

**Input**: Read from `violet-brain/pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-raw.md`

**Use template**: `violet-brain/pr-learnings/_templates/extraction-template.md`

Extract information into these 13 categories:

| # | Category | What to Extract |
|---|----------|-----------------|
| 1 | **Sentiment Analysis** | PR health, reviewer tone, approval likelihood, risk level |
| 2 | **Action Items** | Critical/Important/Nice-to-have, with owner and source |
| 3 | **Bugs & Code Issues** | Lint violations, type errors, logic errors, formatting |
| 4 | **Feature Requests** | Suggested improvements, architectural enhancements |
| 5 | **Documentation Needs** | Missing docs, unclear code, README updates |
| 6 | **Process Improvements** | CI/CD issues, workflow problems, tool suggestions |
| 7 | **Technical Context** | Architecture concerns, design patterns, dependencies |
| 8 | **Stakeholder Context** | Reviewers, domain experts, escalations |
| 9 | **Timeline & Commitments** | Merge deadlines, release blockers, urgent issues |
| 10 | **Expansion Signals** | Future work, refactoring opportunities |
| 11 | **Competitor Intelligence** | Alternative approaches, tools mentioned |
| 12 | **Questions & Answers** | Resolved Q&A, unanswered questions |
| 13 | **Key Insights** | Learnings, surprises, patterns to document |

**Save to**: `violet-brain/pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md`

### Present Gate 1

After extraction completes, present to user:

```
✅ Captured raw PR #{number} data from {repo}
✅ Extracted feedback using 13-category taxonomy

Extraction Summary:
- Sentiment: {sentiment} ({approval status with issue count})
- Action Items: {critical} critical, {important} important, {nice-to-have} nice-to-have
- Bugs: {count} ({breakdown by type})
- Key Insights:
  1. {insight 1}
  2. {insight 2}
  3. {insight 3}
- Recommendations:
  * {recommendation 1}
  * {recommendation 2}

Saved:
- Raw: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-raw.md
- Extraction: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚦 GATE 1: Review Extraction

Does this extraction look accurate and complete?

[✓] Approve - Continue to propose learnings
[✗] Modify - Edit extraction before continuing
[⊗] Reject - Stop here, don't proceed to learning integration
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Handle Gate 1 Response

**User chooses Approve** (✓):
- Proceed to STAGE 2 → Invoke `/v-pr-feedback:integrate`

**User chooses Modify** (✗):
- Ask: "What would you like to modify?"
- Options: User provides feedback → Re-extract, or user edits file directly
- After modifications, re-prompt Gate 1

**User chooses Reject** (⊗):
- Confirm: "Extraction saved but learning integration skipped. Proceed to fix code?"
- If yes → Skip to STAGE 3 → Invoke `/v-pr-feedback:fix`
- If no → Stop, user can resume later

---

## Extraction Guidelines

### Be Comprehensive
- Extract **all** feedback, not just `@claude` comments
- Include bot feedback (automated checks are valuable)
- Don't skip "minor" issues (patterns emerge from small issues)

### Be Specific
- Include file:line references for all code issues
- Quote exact error messages from bots/CI/CD
- Use actual severity levels from tools (error vs warning)

### Be Neutral
- Don't editorialize or interpret
- Present feedback as-is from reviewers
- Distinguish facts from opinions

### Be Forward-Looking
- Identify patterns (not just individual issues)
- Recommend brain updates based on patterns
- Highlight learnings for future PRs

---

## Multi-PR Sessions

If user requests multiple PRs (e.g., "Review PR #1703, #1704, #1705"):

1. Create session file: `violet-brain/pr-learnings/{repo}/sessions/YYYY-MM-DD-HHMM-session.md`
2. Extract each PR separately (save raw + extraction)
3. Present consolidated Gate 1 with all PR summaries
4. Proceed to batch learning integration (analyze patterns across all PRs)

---

## Bot Feedback Patterns

| Bot | What It Provides | Extract As |
|-----|------------------|------------|
| **Graphite Bot** | PR structure, commit hygiene | Process Improvements |
| **ESLint** | Code style, unused vars | Bugs (Lint Violations) |
| **TypeScript** | Type errors, mismatches | Bugs (Type Errors) |
| **CI/CD** | Test failures, build errors | Bugs or Process Improvements |
| **Prettier** | Formatting issues | Bugs (Formatting Issues) |

---

## Success Criteria

- [ ] All PR feedback sources extracted (human + bot)
- [ ] 13 categories populated with relevant data
- [ ] File:line references for all code issues
- [ ] Patterns identified (not just individual issues)
- [ ] Brain update recommendations are specific
- [ ] Extraction saved to correct location
- [ ] User receives clear summary with Gate 1 prompt

---

## Next Steps

After Gate 1 approval:
- **Approved**: Run `/v-pr-feedback:integrate` for STAGE 2
- **Rejected**: Optionally run `/v-pr-feedback:fix` for STAGE 3

---

*Command Version: 1.0.0*
