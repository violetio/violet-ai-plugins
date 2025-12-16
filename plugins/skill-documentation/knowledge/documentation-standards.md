# Documentation Standards

## Types of Documentation

| Type | Audience | Purpose |
|------|----------|---------|
| **API Docs** | External developers | How to use our APIs |
| **User Docs** | End users | How to use features |
| **Internal Docs** | Team members | How systems work |
| **Runbooks** | Operations | How to operate/fix |
| **ADRs** | Future devs | Why decisions were made |

## API Documentation

### OpenAPI/Swagger Pattern
```yaml
paths:
  /orders/{orderId}:
    get:
      summary: Get order by ID
      description: |
        Retrieves the full order details including line items,
        shipping information, and current status.
      parameters:
        - name: orderId
          in: path
          required: true
          schema:
            type: integer
          description: The unique order identifier
      responses:
        200:
          description: Order found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        404:
          description: Order not found
```

### Include Examples
```yaml
examples:
  successfulOrder:
    summary: A completed order
    value:
      id: 12345
      status: "COMPLETED"
      total: 99.99
```

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

## README Structure

```markdown
# Project Name

Brief description of what this project does.

## Quick Start

Minimum steps to get running.

## Prerequisites

What you need installed.

## Installation

Step-by-step setup instructions.

## Usage

Common usage examples.

## Configuration

Environment variables and options.

## Architecture

High-level system overview (for complex projects).

## Contributing

How to contribute.

## License

License information.
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
