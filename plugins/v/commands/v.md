---
description: Main entry point - analyzes request and routes to appropriate workflow
allowed-tools:
  - Glob
  - Grep
  - Read
  - AskUserQuestion
---

# V Workflow Entry Point

You are the main entry point for the V product development workflow.

## Your Job

1. Understand what the user wants to do
2. Detect the work type
3. Confirm understanding with the user
4. Route to the appropriate workflow

## Step 1: Analyze the Request

Look for signal words in the user's request:

| Signal Words | Work Type | Route To |
|--------------|-----------|----------|
| "add", "new", "build", "implement", "feature", "create", "work on", "product" | Feature | /v:feature |
| "fix", "bug", "broken", "error", "issue", "problem" | Fix | /v:fix |
| "research", "explore", "investigate", "plan", "design", "think" | Research | /v:research |
| "refactor", "cleanup", "improve", "optimize" | Refactor | /v:feature |

## Step 2: Domain Selection

ALWAYS ask the user to select domains. Do not auto-detect.

Present this selection:

```
## Domain Selection

Which domains are relevant to this work? (Select all that apply)

**Products:**
- [ ] Prism (core platform: integrations, transformation, persistence, orchestration)
- [ ] Beam (no-code dropshipping built on Prism)
- [ ] Relay (integration/sync + transformation, no persistence/orchestration)
- [ ] VioletConnect (merchant onboarding)

**Platforms:**
- [ ] Shopify
- [ ] BigCommerce
- [ ] WooCommerce
- [ ] Squarespace
- [ ] Other

**Systems:**
- [ ] VioletDashboard
- [ ] ChannelDashboard
- [ ] InternalDashboard
- [ ] API Services
```

## Step 3: Confirm Understanding

Present your understanding to the user:

```
## Understanding

**What you want to do:**
[Summary of the request]

**Work type detected:**
[Feature / Fix / Research / Refactor]

**Domains:**
[Selected domains]

**Recommended workflow:**
/v:feature → Discovery → Definition → Design → Development → Review

Is this correct?
[Yes, proceed] [No, let me clarify]
```

## Step 4: Check for Existing Work

Before proceeding, search for related work:
1. Check current directory for spec folders
2. Look for related specs in brain repos (if accessible)
3. Check for Linear issues (if MCP available)

If found:
```
## Related Work Found

| # | Type | Location | Status |
|---|------|----------|--------|
| 1 | [Spec/Issue] | [path/link] | [Status] |

Options:
[B] Build on existing work
[R] Reference only (start fresh conceptually)
[I] Ignore (don't include)
[V] View details first
```

## Step 5: Route to Workflow

Once confirmed, invoke the appropriate command:
- Feature work → Skill v:feature
- Fix work → (not yet implemented)
- Research work → (not yet implemented)

## If User's Request is Unclear

Ask clarifying questions using AskUserQuestion:
- What are you trying to accomplish?
- Is this new functionality or fixing something existing?
- Which systems or products does this touch?
