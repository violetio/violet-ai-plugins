# External Documentation Standards

Standards for customer-facing documentation in the `violetio/docs` repository.

## Types of Documentation

| Type | Audience | Purpose |
|------|----------|---------|
| **API Docs** | External developers | How to use our APIs |
| **User Docs** | End users | How to use features |

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

## Documentation Publishing (GitBook + GitHub Sync)

### Workflow Overview

Violet uses GitBook with GitHub Sync enabled for external documentation (docs repo).

**The Approval → Deployment Process:**

1. **Create PR in GitHub** - Branch with documentation changes
2. **PR Review & Approval** - Reviewer approves the PR (no manual merge needed)
3. **Approval Triggers Auto-Commit** - GitBook automatically commits the PR to main
4. **Auto-Deploy to Production** - Changes deploy immediately to published GitBook

**Key Points:**
- When a PR is "approved" on GitHub, that approval action triggers GitBook to auto-commit and deploy
- **No manual merge step** - The approval IS the deployment mechanism
- Changes appear in production GitBook within seconds of approval

### Why This Matters

- **Fast feedback**: Changes are live immediately after approval
- **PR review is deployment gate**: Reviewers control what ships
- **No merge conflicts**: GitBook handles the commit automatically
- **Single source of truth**: GitHub repo and GitBook stay in sync

### When Adding External Documentation

1. Branch from main in docs repo
2. Add/update documentation files
3. Submit PR for review
4. Upon approval: GitBook auto-commits and deploys
5. Verify changes in live GitBook preview

## Repository

- `violetio/docs` - Customer-facing documentation
- Uses GitBook with GitHub Sync for auto-deployment
