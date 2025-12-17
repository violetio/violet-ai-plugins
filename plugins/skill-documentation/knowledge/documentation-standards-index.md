# Documentation Standards Index

Violet maintains separate documentation standards for external (customer-facing) and internal (team-facing) documentation.

## External Documentation

For customer-facing documentation in the `violetio/docs` repository:
- See: [External Documentation Standards](./external-documentation-standards.md)
- Covers: API documentation, user guides, publishing workflows
- Publishing: GitBook with GitHub Sync auto-deployment
- Repository: `violetio/docs`

## Internal Documentation

For internal documentation in `prism-brain` and code repositories:
- See: [Internal Documentation Standards](./internal-documentation-standards.md)
- Covers: Architecture decisions, code comments, runbooks
- Publishing: Standard git workflow, version control
- Repositories: `prism-brain`, code repos

## When to Use Which

| Scenario | Use |
|----------|-----|
| Writing API documentation for external developers | External Standards |
| Documenting user-facing features/workflows | External Standards |
| Writing code comments for future developers | Internal Standards |
| Recording architectural decisions | Internal Standards |
| Creating operational runbooks | Internal Standards |
| Documenting internal systems | Internal Standards |

## Shared Principles

Both external and internal documentation follow these writing principles:
- Use clear, direct language
- One idea per sentence
- Prefer active voice
- Use headings, lists, and tables for structure
- Document maintenance dates
- Link to related documentation
