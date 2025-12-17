# Internal Documentation Standards

Standards for internal documentation in `prism-brain`, code repositories, and system architecture.

## Types of Documentation

| Type | Audience | Purpose |
|------|----------|---------|
| **Internal Docs** | Team members | How systems work |
| **Runbooks** | Operations | How to operate/fix |
| **ADRs** | Future devs | Why decisions were made |

## Code Comments

### When to Comment
- **Why**, not what (code shows what)
- Complex algorithms
- Non-obvious business rules
- Workarounds with context

### Comment Patterns
```java
// GOOD - Explains why
// Shopify returns 200 OK even for deleted products,
// so we check userErrors for "Product does not exist"
if (hasUserError(response, "Product does not exist")) {

// BAD - Explains what (obvious from code)
// Check if product exists
if (product != null) {
```

## Architecture Decision Records (ADRs)

### Template
```markdown
# ADR-001: Use MongoDB for BeamService

## Status
Accepted

## Context
We need to choose a database for BeamService that can handle
flexible document structures and high write throughput.

## Decision
We will use MongoDB as the primary database.

## Consequences
### Positive
- Flexible schema for evolving data models
- Good write performance
- Native JSON document support

### Negative
- No ACID transactions across documents
- Different query patterns than SQL

## Alternatives Considered
- PostgreSQL with JSONB
- DynamoDB
```

## Runbook Structure

```markdown
# Runbook: High Order Failure Rate

## Symptoms
- Order success rate drops below 95%
- Alerts firing in #alerts-orders channel

## Severity
High - Customer-facing impact

## Investigation Steps
1. Check order service logs for errors
2. Verify database connectivity
3. Check external service status (payment, shipping)

## Resolution Steps
1. If database issue: [Link to DB runbook]
2. If payment service: Contact payment team
3. If unknown: Escalate to on-call

## Escalation
- Primary: #eng-orders
- After hours: PagerDuty

## Post-Incident
- Create incident report
- Update this runbook if needed
```

## Writing Guidelines

### Clarity
- Use simple, direct language
- One idea per sentence
- Active voice preferred

### Formatting
- Use headings for structure
- Use lists for steps or options
- Use code blocks for technical content
- Use tables for comparisons

### Maintenance
- Date when last verified
- Mark deprecated content clearly
- Link to related docs

## Repositories

- `prism-brain` - Product specifications and architectural decisions
- Code repositories - Architecture, decisions, and operational runbooks
- Uses standard git workflow (branch → PR → merge)
- No auto-deployment; documentation lives in version control
