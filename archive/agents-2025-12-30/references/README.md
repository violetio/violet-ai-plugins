# Agent References

> Canonical role definitions — copy to your product repo and customize.

**Updated in v1.2.0**: All references now follow the **thin wrapper pattern** (see [patterns/thin-wrapper-agent-pattern.md](../patterns/thin-wrapper-agent-pattern.md)).

---

## What Are These?

These are **thin wrapper role definitions** (200-250 lines) that reference domain knowledge in `systems/` docs rather than embedding it. This design:

- ✅ Makes pattern updates automatic (change systems/ doc once)
- ✅ Reduces agent file bloat (50% smaller than v1.1.2)
- ✅ Maintains single source of truth for patterns
- ✅ Scales across multiple products efficiently

**To use an agent in your product repo:**
1. Copy the reference file to your product repo
2. Create `systems/{brain}/` docs with your domain knowledge
3. Customize the agent to reference YOUR domain docs
4. Follow the "Customization" section at the bottom of each file

---

## Customization Status

| Status | Meaning | What to Do |
|--------|---------|-----------|
| ✅ **Fully Customized** | Reference ready to use | Copy to your brain, adapt domain references |
| ⚠️ **Needs Customization** | Template not yet adapted | Complete customization checklist before use |
| 🏗️ **In Development** | Currently being improved | Check back soon |

---

## Pattern: Thin Wrapper Architecture

Each reference agent follows this structure:

```markdown
# {Role} - {Example Brain}

## Skill Invocation
Invokes: Skill v-{role}

## REQUIRED READING
- [Domain Doc 1](../../systems/{brain}/doc1.md)
- [Domain Doc 2](../../systems/{brain}/doc2.md)

## {Brain}-Specific Context
[Product constraints, decisions, SLOs]

## {Brain}-Specific Workflows
[Unique workflows for this product]
```

**See**: [Thin Wrapper Pattern Guide](../patterns/thin-wrapper-agent-pattern.md)

---

## How to Use

### Starting a New Product

```bash
# Example: Set up PM agent for prism-brain
cp violet-brain/agents/references/commerce-pm.md prism-brain/agents/product/pm.md

# Edit the file:
# 1. Replace "Violet" with "Prism"
# 2. Fill in the Customization section at the bottom
# 3. Add your product-specific context
```

### Customization Checklist

Each reference file has a "Customization" section at the end with:
- **Required Customizations** — What must change for your product
- **Product Context to Add** — Checklist of product-specific details

---

## Reference Categories

### Product Team
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [commerce-pm.md](commerce-pm.md) | Channel & merchant product management | ✅ | `agents/product/pm.md` |
| [platform-pm.md](platform-pm.md) | Platform integration product management | ✅ | `agents/product/platform-pm.md` |
| [experience-designer.md](experience-designer.md) | UX flows, states, edge cases | ✅ | `agents/product/designer.md` |

### Engineering Team
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [architect.md](architect.md) | System design, ADRs, technical decisions | ✅ | `agents/engineering/architect.md` |
| [tech-lead.md](tech-lead.md) | Code review, engineering standards | ✅ | `agents/engineering/tech-lead.md` |
| [backend-engineer.md](backend-engineer.md) | Server-side implementation | ✅ | `agents/engineering/backend.md` |
| [frontend-engineer.md](frontend-engineer.md) | Client-side implementation | ✅ | `agents/engineering/frontend.md` |
| [data-engineer.md](data-engineer.md) | Data pipelines, analytics infrastructure | ✅ | `agents/engineering/data.md` |
| [qa-engineer.md](qa-engineer.md) | Testing, quality assurance | ✅ | `agents/engineering/qa.md` |

### Operations Team
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [support-agent.md](support-agent.md) | Customer support, ticket resolution | ⚠️ | `agents/operations/support.md` |
| [success-agent.md](success-agent.md) | Customer health, retention | ⚠️ | `agents/operations/success.md` |
| [onboarding-agent.md](onboarding-agent.md) | New customer setup | ⚠️ | `agents/operations/onboarding.md` |

### Finance Team
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [finance-analyst.md](finance-analyst.md) | Financial reporting, forecasting | ⚠️ | `agents/finance/analyst.md` |
| [finance-engineer.md](finance-engineer.md) | Infrastructure costs, billing systems | ⚠️ | `agents/finance/engineer.md` |

### Documentation Team
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [customer-docs-agent.md](customer-docs-agent.md) | External docs, customer communication | ⚠️ | `agents/documentation/customer-docs.md` |
| [internal-docs-agent.md](internal-docs-agent.md) | Internal docs, runbooks, tooling | ⚠️ | `agents/documentation/internal-docs.md` |

### Coordination
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [project-coordinator.md](project-coordinator.md) | Task orchestration, status tracking | ⚠️ | `agents/coordination/project-coordinator.md` |
| [repo-coordinator.md](repo-coordinator.md) | Cross-repo coordination | ⚠️ | `agents/coordination/repo-coordinator.md` |

### Infrastructure
| Reference | Purpose | Status | Copy To |
|-----------|---------|--------|---------|
| [infrastructure-engineer.md](infrastructure-engineer.md) | DevOps, deployment, monitoring | ✅ | `agents/infrastructure/infra.md` |

---

## 9D Framework Agent Mapping

Which agents are involved in each phase of the [9D Framework](../../patterns/9d-framework.md):

| Phase | Primary Agents | Supporting Agents |
|-------|----------------|-------------------|
| **Discovery** | commerce-pm, platform-pm | support-agent, context-gap-detector |
| **Definition** | commerce-pm, experience-designer | quality-gate-reviewer, finance-analyst |
| **Design** | architect, tech-lead | backend-engineer, frontend-engineer |
| **Development** | backend-engineer, frontend-engineer, qa-engineer | tech-lead, data-engineer |
| **Documentation** | customer-docs-agent, internal-docs-agent | experience-designer, support-agent |
| **Delivery** | tech-lead, infrastructure-engineer | customer-docs-agent, support-agent |
| **Data** | data-engineer, finance-analyst | commerce-pm, platform-pm |
| **Debrief** | project-coordinator | All involved agents |
| **Deprecation** | commerce-pm, backend-engineer | customer-docs-agent, support-agent |

### Agent Aliases

Some phase skills use shorthand invocations. Here's the mapping:

| Alias | Maps To |
|-------|---------|
| `@pm-agent` | commerce-pm or platform-pm |
| `@devops` | infrastructure-engineer |
| `@customer-docs` | customer-docs-agent |
| `@internal-docs` | internal-docs-agent |
| `@quality-gate` | quality-gate-reviewer |

---

## Updating References

When updating these files:

1. **Consider all products** — Changes affect everyone who copies these
2. **Keep generic** — Product-specific details go in product repos
3. **Update Customization section** — Ensure it stays relevant
4. **Communicate changes** — Major changes should be announced

---

## Contributing

Found a better pattern? PRs welcome!

- Keep references comprehensive — they should answer any question about the role
- Stay generic — product-specific details belong in product repos
- Update the Customization section when adding new configurable aspects

---

*Maintained by violet-brain meta agents and human contributors.*
