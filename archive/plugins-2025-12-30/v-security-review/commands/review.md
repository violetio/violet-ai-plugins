---
name: review
description: "Run security checklist and generate blocking/advisory report"
---

# Security Review Command

> Comprehensive security review with blocking authority for critical findings.

**Authority**: BLOCKS MERGE for Critical/High severity

---

## Execution

### Step 1: Fetch Code Changes

```bash
# Get PR diff
gh pr view {number} --json files --jq '.files[] | {path: .path, additions: .additions}'

# OR read files directly
cat {file-path}
```

**Focus on**:
- New files (additions)
- Security-sensitive files (auth, payment, data handling)
- API endpoints
- Database queries
- User input handling

### Step 2: Security Checklist Review

Use `violet-brain/standards/sec-priv/code-security.md` as reference.

#### Input Validation
- [ ] All user input validated (type, length, format, range)
- [ ] Allowlists used over denylists
- [ ] Server-side validation (not just client-side)
- [ ] Input sanitized before use
- [ ] Proper encoding for context (HTML, URL, SQL)

#### Authentication
- [ ] Authentication required for sensitive endpoints
- [ ] Strong password requirements enforced
- [ ] Rate limiting on auth endpoints
- [ ] Secure session management
- [ ] No passwords in plain text
- [ ] Session tokens not in URLs
- [ ] Sessions invalidated on logout

#### Authorization
- [ ] Authorization checked on every request
- [ ] Principle of least privilege applied
- [ ] Object-level permissions verified (IDOR prevention)
- [ ] No client-provided role/permission trust

#### Data Protection
- [ ] Sensitive data encrypted at rest
- [ ] TLS 1.2+ for all connections
- [ ] Sensitive data not in logs
- [ ] Sensitive data not in URLs
- [ ] Data retention limits set

#### Secrets Management
- [ ] No secrets hardcoded in code
- [ ] AWS Parameter Store or similar used
- [ ] No credentials in environment variables
- [ ] No API keys in client-side code

#### SQL Injection Prevention
- [ ] Parameterized queries used
- [ ] No string concatenation in queries
- [ ] No dynamic table/column names from user input
- [ ] ORM used correctly

#### XSS Prevention
- [ ] User content escaped before rendering
- [ ] Appropriate output encoding (HTML, JS, URL)
- [ ] CSP headers set
- [ ] No innerHTML with user content
- [ ] No eval() or Function() with user input

#### Error Handling
- [ ] Generic error messages to users
- [ ] Sensitive information not in errors
- [ ] Errors logged securely
- [ ] Stack traces not exposed

#### Logging & Audit
- [ ] Security events logged
- [ ] Sensitive data not logged
- [ ] Audit trail for sensitive operations
- [ ] Logs tamper-resistant

#### Dependencies
- [ ] Dependencies up to date
- [ ] No known CVEs in dependencies
- [ ] Dependency integrity verified
- [ ] Unused dependencies removed

#### API Security
- [ ] Rate limiting on all endpoints
- [ ] CORS configured correctly
- [ ] API keys validated
- [ ] Request size limits enforced

#### File Uploads
- [ ] File type validation (not just extension)
- [ ] File size limits enforced
- [ ] Files scanned for malware
- [ ] Files stored outside web root

### Step 3: Threat Modeling

For significant changes:

**Questions to ask**:
1. What data is being processed?
2. Who has access to this data?
3. What could an attacker do with this endpoint?
4. What happens if input is malicious?
5. Can this be abused at scale?

### Step 4: Generate Security Report

**Format**:

```markdown
# Security Review: PR #{number}

**Date**: YYYY-MM-DD
**Reviewer**: security-reviewer agent
**PR**: {title}
**Repository**: {repo-name}

---

## Summary

**Overall Risk**: {CRITICAL|HIGH|MEDIUM|LOW}
**Decision**: {BLOCK MERGE|ADVISE FIXES|APPROVE}

{1-2 sentence summary}

---

## Critical Findings (BLOCKS MERGE)

### 1. {Vulnerability Type}

**Severity**: Critical
**File**: {path/to/file.ts}:{line}
**CWE**: {CWE-XXX}

**Issue**: {description}

**Code**:
```typescript
{vulnerable code}
```

**Impact**: {what attacker could do}

**Remediation**:
1. {step 1}
2. {step 2}

---

## Security Checklist Summary

| Category | Status | Issues |
|----------|--------|--------|
| Input Validation | ✓/✗ | {count} |
| Authentication | ✓/✗ | {count} |
| Authorization | ✓/✗ | {count} |
| ...

---

## Blocking Decision

**BLOCKS MERGE**: {YES|NO}
**Reason**: {why}
**Next Steps**: {actions required}
```

### Step 5: Return Decision

**If BLOCKING (Critical/High)**:
```
🚫 SECURITY REVIEW: BLOCKED

PR #{number} has {count} critical/high severity findings that MUST be addressed.

Tech Lead: DO NOT approve until resolved and re-reviewed.
```

**If ADVISORY (Medium)**:
```
⚠️ SECURITY REVIEW: ADVISORY

PR #{number} has {count} medium severity issues that SHOULD be addressed.

Tech Lead: Proceed with caution or request fixes.
```

**If APPROVED**:
```
✅ SECURITY REVIEW: APPROVED

PR #{number} passed security review with no blocking issues.
```

---

## Common Vulnerabilities Detection

### Secrets in Code
```bash
grep -r "api[_-]?key" --include="*.ts" --include="*.js"
grep -r "password\s*=" --include="*.ts"
grep -r "secret" --include="*.ts"
```

### SQL Injection
Look for:
- String concatenation in queries
- Template literals with user input in SQL
- Raw queries with unparameterized input

### XSS
Look for:
- innerHTML with user content
- dangerouslySetInnerHTML in React
- eval() or Function() with user input
- No output encoding

### IDOR
Look for:
- Resource ID from request not validated
- No ownership check before returning data
- Direct object references without access control

---

## Success Criteria

- [ ] All code changes reviewed against checklist
- [ ] Critical/High findings identified and reported
- [ ] Blocking decision communicated clearly
- [ ] Remediation guidance provided
- [ ] Security report generated

---

*Command Version: 1.0.0*
