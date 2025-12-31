---
name: Security Review
description: Security review with blocking authority for critical vulnerabilities
---

# Security Review Skill

> Protect Violet codebases from security vulnerabilities through dedicated security review.

**Authority**: BLOCKING for Critical/High severity findings

---

## Overview

The Security Review skill provides comprehensive security review with **blocking authority** for critical vulnerabilities. Tech Lead cannot override security blocks - issues must be fixed.

**Key Principle**: Security is not optional. Critical vulnerabilities MUST be fixed before code merges.

---

## When to Use

**MANDATORY Invocation**:
- Tech Lead agent MUST invoke before approving any PR
- Cannot be skipped or bypassed
- Required for all code changes, regardless of size

**Additional Invocations**:
- When PR feedback extraction identifies security issues
- When new security standards are added
- When security incident requires code audit

---

## Commands

| Command | Purpose |
|---------|---------|
| `/v-security-review:review` | Run security checklist and generate report |
| `/v-security-review:override` | Override with logged justification |

---

## Severity Levels

### Critical (BLOCKS MERGE)

**Definition**: Vulnerabilities that pose immediate, severe risk

| Vulnerability | Example | Impact |
|---------------|---------|--------|
| **Secrets in code** | API keys, passwords, tokens hardcoded | Immediate credential exposure |
| **SQL Injection** | User input in raw SQL queries | Database compromise |
| **XSS** | Unescaped user content in HTML | Account takeover |
| **Unencrypted PII** | SSN, credit cards in logs/storage | Compliance violation |
| **Authentication bypass** | Missing auth checks | Unauthorized access |
| **Authorization bypass (IDOR)** | No resource ownership validation | Data exposure |
| **Remote code execution** | Unsafe deserialization, eval() | Full system compromise |
| **Cryptographic failures** | Weak algorithms, hardcoded keys | Data exposure |

**Action**: **BLOCK merge** until fixed and re-reviewed

### High (BLOCKS MERGE)

**Definition**: Significant vulnerabilities requiring immediate attention

| Vulnerability | Example | Impact |
|---------------|---------|--------|
| **Missing input validation** | No validation on user input | Injection attacks |
| **Insecure dependencies** | Known CVEs in dependencies | Exploitable vulnerabilities |
| **Weak authentication** | No rate limiting, weak passwords | Account compromise |
| **Information disclosure** | Error messages expose system details | Aids attackers |
| **Missing HTTPS** | Sensitive data over HTTP | Man-in-the-middle attacks |
| **Session fixation** | Session ID not regenerated | Session hijacking |

**Action**: **BLOCK merge** until fixed and re-reviewed

### Medium (ADVISORY)

**Definition**: Issues that should be fixed but don't block merge

| Issue | Example | Recommendation |
|-------|---------|----------------|
| **Missing security headers** | No CSP, X-Frame-Options | Add headers |
| **Verbose error messages** | Stack traces to users | Generic messages |
| **Insufficient logging** | No audit trail | Add security logging |
| **Rate limiting gaps** | Some endpoints not limited | Add rate limits |
| **Outdated dependencies** | Non-CVE outdated packages | Update dependencies |

**Action**: **ADVISE** fix, tech-lead decides if blocks

### Low (INFORMATIONAL)

**Definition**: Minor issues or best practices

| Issue | Example |
|-------|---------|
| **Security through obscurity** | Hiding functionality as security |
| **Missing comments** | Complex security logic uncommented |
| **Inconsistent patterns** | Different auth approaches |

**Action**: **INFORM** only, does not block

---

## Security Checklist Categories

### Input Validation
- All user input validated (type, length, format, range)
- Allowlists used over denylists
- Server-side validation (not just client-side)
- Input sanitized before use

### Authentication
- Authentication required for sensitive endpoints
- Strong password requirements enforced
- Rate limiting on auth endpoints
- Secure session management

### Authorization
- Authorization checked on every request
- Principle of least privilege applied
- Object-level permissions verified (IDOR prevention)
- No client-provided role/permission trust

### Data Protection
- Sensitive data encrypted at rest
- TLS 1.2+ for all connections
- Sensitive data not in logs
- Sensitive data not in URLs

### Secrets Management
- No secrets hardcoded in code
- AWS Parameter Store or similar used
- No credentials in environment variables
- No API keys in client-side code

### SQL Injection Prevention
- Parameterized queries used
- No string concatenation in queries
- No dynamic table/column names from user input
- ORM used correctly

### XSS Prevention
- User content escaped before rendering
- Appropriate output encoding
- CSP headers set
- No innerHTML with user content

---

## Security Override Process

**CRITICAL**: Overrides are logged to `violet-brain/security-overrides/` for audit.

When engineer/tech-lead believes block is incorrect:

1. User requests: "Override security block for PR #123"
2. Agent prompts for justification
3. Override document generated and committed
4. Security team reviews monthly

**Override document location**: `violet-brain/security-overrides/YYYY-MM-DD-pr-{number}-override.md`

---

## Reference Files

| File | Purpose |
|------|---------|
| `violet-brain/standards/sec-priv/code-security.md` | Complete security standards |
| `violet-brain/docs/sec-priv/quick-reference.md` | One-page checklist |
| `violet-brain/agents/references/security-reviewer.md` | Security reviewer criteria |

---

*Skill Version: 1.0.0*
