---
name: override
description: "Override security block with logged justification"
---

# Security Override Command

> Override a security block with mandatory logging and audit trail.

**CRITICAL**: All overrides are logged to `violet-brain/security-overrides/`

---

## When to Use

Only when:
1. Security-reviewer blocked a PR
2. Engineer/tech-lead believes finding is incorrect or not applicable
3. User explicitly requests override

**This is rare** - most blocks should be fixed, not overridden.

---

## Execution

### Step 1: Verify Security Block Exists

```bash
# Check if PR has security block
gh pr view {number} --json reviews --jq '.reviews[] | select(.state == "CHANGES_REQUESTED")'
```

Confirm that security-reviewer actually blocked this PR.

### Step 2: Prompt for Justification

Present to user:

```
🚨 SECURITY OVERRIDE REQUEST

PR: #{number}
Blocked By: security-reviewer
Finding: {vulnerability_description}
Severity: {Critical|High}

This override will be logged for security audit.

Justification (required):
_____________________________________________________

Examples of valid justifications:
- "Test key for dev environment, not used in production"
- "False positive - this input is system-generated, not user-controlled"
- "Already validated upstream in AuthMiddleware (see line 45)"

⚠️  Override will be logged to:
violet-brain/security-overrides/YYYY-MM-DD-pr-{number}-override.md

Continue with override?
[✓] Yes - Override and document
[✗] No - Cancel override
```

### Step 3: Generate Override Document

If user confirms, create:

**File**: `violet-brain/security-overrides/YYYY-MM-DD-pr-{number}-override.md`

```markdown
# Security Override: PR #{number}

**Date**: YYYY-MM-DD HH:MM:SS UTC
**Repository**: {repo-name}
**PR**: {github-url}
**Overridden By**: @{username}

## Security Finding (BLOCKED)

**Severity**: {Critical|High}
**Vulnerability**: {type}
**File**: {path}:{line}
**Finding**: {description}

## Override Justification

{User's explanation}

## Context

**Commit**: {hash}
**Branch**: {branch}
**Security Report**: pr-learnings/{repo}/YYYY-MM-DD-pr-{number}-extraction.md

## Audit

**Review Due**: {30 days from now}
**Status**: Pending Review
```

### Step 4: Commit Override to violet-brain

```bash
cd violet-brain

# Create override document
mkdir -p security-overrides

# Write document (created in step 3)

# Stage and commit
git add security-overrides/YYYY-MM-DD-pr-{number}-override.md

git commit -m "sec: log override for PR #{number} ({brief_justification})

Override requested by @{username}
Severity: {severity}
Finding: {vulnerability_type}

Justification: {one_line_summary}

Review due: {30_days_from_now}"

git push origin main
```

### Step 5: Confirm Override

```
✅ Security Override Logged

PR #{number} override has been documented.

Override Document:
violet-brain/security-overrides/YYYY-MM-DD-pr-{number}-override.md

⚠️  Security team will review this override.

You may now proceed with PR approval.
```

---

## Override Document Schema

```markdown
# Security Override: PR #{number}

**Date**: {ISO timestamp}
**Repository**: {repo}
**PR**: {url}
**Overridden By**: @{username}

## Security Finding (BLOCKED)

**Severity**: {severity}
**Vulnerability**: {CWE reference}
**File**: {path}:{line}
**Finding**: {detailed description}

## Override Justification

{Multi-line justification from user}

## Context

**Commit**: {hash}
**Branch**: {branch}
**Security Report**: {path to extraction if exists}

## Audit

**Review Due**: {date 30 days out}
**Status**: Pending Review
**Reviewed By**: {empty until reviewed}
**Review Date**: {empty until reviewed}
**Review Notes**: {empty until reviewed}
```

---

## Audit Process

**Monthly Security Review**:

```bash
# List all overrides
ls violet-brain/security-overrides/

# Find critical overrides
grep -l "Severity: Critical" violet-brain/security-overrides/*.md

# Find pending reviews
grep -l "Status: Pending Review" violet-brain/security-overrides/*.md

# Find overdue reviews
# (Custom script to check Review Due dates)
```

**After Review**:

Security team updates override document:

```markdown
## Audit

**Review Due**: 2025-01-22
**Status**: Reviewed - Accepted
**Reviewed By**: @security-team-member
**Review Date**: 2025-01-20
**Review Notes**: Confirmed test key, verified not in production config.
```

---

## Valid Override Reasons

### Acceptable Justifications

| Reason | Example |
|--------|---------|
| **Test/Dev credentials** | "Test API key for local dev, not used in production" |
| **False positive** | "Input is system-generated UUID, not user-controlled" |
| **Validated elsewhere** | "Input validated in AuthMiddleware before reaching this code" |
| **Temporary workaround** | "Temporary fix for incident, proper fix tracked in INT-456" |
| **Legacy code being deprecated** | "This endpoint deprecated in 2 weeks, full rewrite in INT-789" |

### Unacceptable Justifications

| Reason | Why Invalid |
|--------|-------------|
| "It's urgent" | Urgency doesn't make code secure |
| "We'll fix it later" | Without tracking, later never comes |
| "It's just internal" | Internal code can still be exploited |
| "No one will find it" | Security through obscurity doesn't work |

---

## Cancelling Override

If user chooses "No - Cancel override":

```
Override cancelled.

To fix the security issue:
1. Review finding: {vulnerability_description}
2. Apply remediation: {remediation_steps}
3. Push fix to PR branch
4. Request security re-review: /v-security-review:review
```

---

## Success Criteria

- [ ] Security block verified
- [ ] User provided justification
- [ ] Override document created
- [ ] Document committed to violet-brain
- [ ] Audit trail established
- [ ] User informed of review process

---

*Command Version: 1.0.0*
