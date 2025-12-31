# Agents

This directory contains **reference definitions** for roles — templates that product brains can copy and customize.

---

## Important: Plugins First

**Always use violet-ai-plugins skills first.** This directory is supplementary.

### Priority Order

```
1. violet-ai-plugins (skills)     ← START HERE (/v:architect, /v:experience-designer)
   ↓
2. Product brain context          ← Check {product}-brain/patterns/ and /agents/
   ↓
3. This directory                 ← Only if no product customization exists
```

### When to Use This Directory

- **Starting a new product brain** — Copy reference definitions from `references/`
- **No product-specific version exists** — Fall back to generic templates here
- **violet-brain-specific work** — Use `meta/` agents (triage, evals)

### What This Directory Contains

| Subdirectory | Purpose |
|--------------|---------|
| `references/` | Generic role templates (copy to product brains) |
| `meta/` | violet-brain-specific agents (triage, evals) |

---

## Agent Types

### Meta Agents (`/agents/meta/`)

These agents **actively operate** in violet-brain to improve the system:

| Agent | Purpose |
|-------|---------|
| `triage-agent.md` | Manages incubator, assigns work to brains |
| `agent-builder.md` | Creates new agent definitions |
| `agent-improver.md` | Improves agents based on feedback/evals |
| `eval-generator.md` | Creates evaluation scenarios |
| `eval-runner.md` | Runs evals and reports results |
| `skill-creator.md` | Creates new domain skills |
| `skill-extractor.md` | Extracts skills from codebases |

### Customer Intelligence Agents (`/agents/`)

These agents process customer interactions and maintain health tracking:

| Agent | Purpose |
|-------|---------|
| `customer-meeting-extractor.md` | Extracts 13-category intelligence from meetings |
| `references/customer-success-intelligence-agent.md` | Full CS agent with health monitoring, routing, alerts |
| `references/success-agent.md` | Customer health monitoring and escalations |
| `references/support-agent.md` | Tier 1/2 support triage |

### System Health Agents (`/agents/`)

These agents ensure quality across the system:

| Agent | Purpose |
|-------|---------|
| `quality-gate-reviewer.md` | Validates specs meet standards before handoff |
| `context-gap-detector.md` | Identifies missing knowledge before work proceeds |

### Reference Definitions (`/agents/references/`)

These are the **canonical "source of truth"** for what each role should do. Product repos copy and customize these. Each file includes a "Customization" section at the end with guidance on what to change.

| Reference | Purpose |
|-----------|---------|
| `commerce-pm.md` | What a Commerce PM does (channel & merchant focus) |
| `platform-pm.md` | What a Platform PM does (integrations focus) |
| `experience-designer.md` | What an Experience Designer does |
| `architect.md` | What an Architect does |
| `security-reviewer.md` | Security review with blocking authority |
| `backend-engineer.md` | What a Backend Engineer does |
| `frontend-engineer.md` | What a Frontend Engineer does |
| `infrastructure-engineer.md` | What an Infrastructure Engineer does |
| `tech-lead.md` | What a Tech Lead does |
| `qa-engineer.md` | What a QA Engineer does |
| `data-engineer.md` | What a Data Engineer does |
| `support-agent.md` | What a Support Agent does |
| `success-agent.md` | What a Success Agent does |
| `onboarding-agent.md` | What an Onboarding Agent does |
| `finance-analyst.md` | What a Finance Analyst does |
| `finance-engineer.md` | What a Finance Engineer does |
| `customer-docs-agent.md` | What a Customer Docs Agent does |
| `internal-docs-agent.md` | What an Internal Docs Agent does |
| `project-coordinator.md` | What a Project Coordinator does |
| `repo-coordinator.md` | What a Repo Coordinator does |
| `sec-priv.md` | Security & Privacy policy, compliance, vendor assessments |

---

## How to Use Agents

### Customer Intelligence

```
# Extract meeting from Notion (provide URL)
"Extract intelligence from https://notion.so/violetio/Meeting-xxxxx"

# Process customer meeting with full extraction
Invoke: customer-meeting-extractor
Task: Extract meeting at [Notion URL] for customer [name]

# Check customer health
"Review health for customer [name]"
→ Reads: customers/{name}/profile.md + recent extractions

# Run weekly health check
Invoke: customer-success-intelligence-agent
Task: Generate weekly health report for all customers
```

### Incubator & Roadmap

```
Invoke: triage-agent
Task: Process the incubator inbox
```

```
Invoke: triage-agent
Task: Check status of [idea] in exploring
```

### Meta Agents

```
Invoke: agent-builder
Task: Create a new agent for [purpose]
```

### System Health

```
Invoke: quality-gate-reviewer
Task: Review spec at [path] for completeness
```

```
Invoke: context-gap-detector
Task: Analyze [document] for missing context
```

---

## Agent Routing

Quick reference for which agent to use:

| When task is... | Use... | Read first |
|----------------|--------|-----------|
| New idea without home | `agents/meta/triage-agent.md` | `incubator/README.md` |
| Improving agents | `agents/meta/agent-improver.md` | The agent definition |
| Creating new agent | `agents/meta/agent-builder.md` | Reference agents |
| Running evals | `agents/meta/eval-runner.md` | Eval scenarios |
| Generating evals | `agents/meta/eval-generator.md` | Existing evals |
| Creating skills | `agents/meta/skill-creator.md` | Related skills |
| Extracting skills | `agents/meta/skill-extractor.md` | The codebase |
| Validating spec | `quality-gate-reviewer.md` | The spec + standards |
| Finding gaps | `context-gap-detector.md` | The document |
| Security/privacy concern | `agents/references/sec-priv.md` | `shared/skills/sec-priv.md` |
| Vendor assessment | `agents/references/sec-priv.md` | `standards/sec-priv/vendor-requirements.md` |
| Customer meeting extraction | `agents/customer-meeting-extractor.md` | `shared/skills/customer-meeting-extraction.md` |
| Customer health check | `agents/references/customer-success-intelligence-agent.md` | `customers/{customer}/profile.md` |
| Customer issue triage | `agents/references/support-agent.md` | Customer profile + recent extractions |
| Customer escalation | `agents/references/success-agent.md` | Customer health snapshots |

---

## For Product Repos

### Starting a New Product

1. Create `{product}-brain` repo
2. Copy agents from `violet-brain/agents/references/` — these are your agents
3. Follow the "Customization" section at the bottom of each agent file
4. Customize for your product's domain and tech stack

### Customization Guidelines

Each reference agent file includes a "Customization" section with guidance on:
- What to keep as-is (core responsibilities)
- What to modify (product-specific context)
- What to add (domain knowledge)

---

## Related

- `shared/skills/` — Domain knowledge used by agents
- `patterns/agent-discovery-workflow.md` — How to find the right agent
- `SYNC-GUIDE.md` — How to sync agents across brains
