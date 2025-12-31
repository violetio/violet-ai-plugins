# Agent Reference Versions

> Track reference agent versions and customization status across product brains.

**Current Pattern Version**: 1.2.0 (Thin Wrapper)
**Updated**: 2025-12-19

---

## Reference Agents Status

All agents in `agents/references/` follow the **thin wrapper pattern** (see [patterns/thin-wrapper-agent-pattern.md](../patterns/thin-wrapper-agent-pattern.md)).

### Engineering Reference Agents

| Agent | Pattern Version | Status | Last Updated | Notes |
|-------|---|---|---|---|
| **tech-lead.md** | 1.2.0 | ✅ Stable | 2025-12-19 | Thin wrapper pattern |
| **backend-engineer.md** | 1.2.0 | ✅ Stable | 2025-12-19 | Java/Spring patterns |
| **frontend-engineer.md** | 1.2.0 | ✅ Stable | 2025-12-19 | React/Redux patterns |
| **qa-engineer.md** | 1.2.0 | ✅ Stable | 2025-12-19 | Testing patterns |
| **architect.md** | 1.2.0 | ✅ Stable | 2025-12-19 | System design patterns |
| **infrastructure-engineer.md** | 1.2.0 | ✅ Stable | 2025-12-19 | Kubernetes/Terraform |

### Product Reference Agents

| Agent | Pattern Version | Status | Last Updated | Notes |
|-------|---|---|---|---|
| **pm.md** | 1.2.0 | ✅ Stable | 2025-12-19 | Discovery/Definition |
| **designer.md** | 1.2.0 | ✅ Stable | 2025-12-19 | UX/Figma patterns |

### System Agents

| Agent | Pattern Version | Status | Last Updated | Notes |
|-------|---|---|---|---|
| **support-agent.md** | 1.1.2 | ⚠️ Update Available | 2025-12-10 | Recommend updating to 1.2.0 |
| **success-agent.md** | 1.1.2 | ⚠️ Update Available | 2025-12-10 | Recommend updating to 1.2.0 |
| **onboarding-agent.md** | 1.1.2 | ⚠️ Update Available | 2025-12-10 | Recommend updating to 1.2.0 |

---

## Product Brain Customization Status

### Prism-Brain (v1.2.0) ✅ Current

**Customization Level**: Fully customized using thin wrapper pattern

| Agent Category | Status | Customization | Notes |
|---|---|---|---|
| **Engineering** | ✅ Full | Tech Lead, Frontend, QA, Backend, Architect, Infra | All converted to thin wrappers |
| **Product** | ✅ Full | PM, Designer | Domain-specific workflows |
| **Leadership** | ✅ Full | GM | Strategic decision routing |
| **Domain** | ✅ Partial | Payment Expert, API Expert, Catalog Expert | ~13KB domain agents (separate from plugins) |
| **Operations** | ⚠️ Partial | Project Coordinator | Partially customized |
| **Finance** | ⚠️ Partial | Finance Analyst | Partially customized |
| **Documentation** | ⚠️ Partial | Technical Writer | Partially customized |

**How to Sync**: Use as reference for thin wrapper pattern implementation

### Beam-Brain (v1.2.0) ✅ Current

**Customization Level**: Fully customized using thin wrapper pattern

| Agent Category | Status | Customization | Notes |
|---|---|---|---|
| **Engineering** | ✅ Full | Tech Lead, Frontend, QA, Backend, Architect, Infra | All converted to thin wrappers |
| **Product** | ✅ Full | PM, Designer | Dropship-specific flows |
| **Domain** | ✅ Full | Catalog Expert, Order Expert, Shopify Expert | Deep domain expertise |
| **Operations** | ⚠️ Partial | | Needs customization |
| **Finance** | ⚠️ Partial | | Needs customization |

**How to Sync**: Update similar agents if diverged; pull catalog/order/shopify experts if needed

### Violet-Execution (v1.1.2) ⚠️ Needs Update

**Customization Level**: Partial (leadership/portfolio-level agents)

| Agent Category | Status | Customization | Notes |
|---|---|---|---|
| **Leadership** | ✅ Partial | Strategic planning | Recommend updating pattern to 1.2.0 |
| **PM** | ✅ Partial | Portfolio PM | Recommend updating pattern to 1.2.0 |

**How to Sync**:
- [ ] Update reference agents to v1.2.0 (thin wrapper pattern)
- [ ] Create systems/ docs for domain knowledge
- [ ] Extract patterns from agent files

### Track-and-Field-Brain (v1.1.2) ⚠️ Needs Update

**Status**: Not yet synced with v1.2.0 thin wrapper pattern

**How to Sync**:
- [ ] Review agents/
- [ ] Convert to thin wrapper pattern
- [ ] Create systems/ documentation
- [ ] Update VERSION.md with new status

### Open-Commerce-Brain (v1.1.2) ⚠️ Needs Update

**Status**: Not yet synced with v1.2.0 thin wrapper pattern

**How to Sync**:
- [ ] Review agents/
- [ ] Convert to thin wrapper pattern
- [ ] Create systems/ documentation
- [ ] Update VERSION.md with new status

---

## What Changed in v1.2.0

**From v1.1.2 → v1.2.0**:

### Agent Architecture
- **New**: Thin wrapper pattern (200-250 lines vs 500+)
- **New**: Required Reading section linking to systems/ docs
- **New**: Customization status tracking (✅ vs ⚠️)
- **Removed**: Embedded domain knowledge (moved to systems/)

### Pattern Documentation
- **New**: patterns/thin-wrapper-agent-pattern.md
- **New**: patterns/agent-discovery-workflow.md
- **Updated**: agents/references/README.md with customization status

### Tooling
- **New**: agents/VERSION.md (this file)
- **New**: VERSION.json with sync status across brains
- **New**: SYNC-GUIDE.md for keeping brains aligned

### Implementation Details
- All reference agents now reference systems/ docs instead of embedding patterns
- Agent files reduced 50% in size on average
- Makes pattern updates automatic (change systems/ doc once)

---

## Migration Checklist

### For Product Brains Updating to v1.2.0

```
Goal: Migrate from v1.1.2 (old) to v1.2.0 (thin wrapper pattern)

Preparation:
- [ ] Read patterns/thin-wrapper-agent-pattern.md
- [ ] Identify domain knowledge in current agents
- [ ] List all references to patterns/skills/standards

Implementation per Agent:
- [ ] Extract domain knowledge to systems/{brain}/{DOMAIN}.md
- [ ] Rewrite agent as thin wrapper (250 lines max)
- [ ] Add "REQUIRED READING" section with links
- [ ] Add "Customization Status" (✅ or ⚠️)
- [ ] Create invocation examples
- [ ] Test links to systems/ docs work

Validation:
- [ ] All agent files under 250 lines
- [ ] All pattern references external (in systems/)
- [ ] All links to systems/ docs work
- [ ] Customization status clear

Final:
- [ ] Update agents/VERSION.md with new status
- [ ] Update VERSION.json sync status
- [ ] Create PR with pattern description
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.2.0 | 2025-12-19 | Thin wrapper agent pattern + synchronization system |
| 1.1.2 | 2025-12-10 | Previous stable version |

---

## How Product Brains Track Their Version

**In your product brain's VERSION.json**:

```json
{
  "version": "1.0.0",
  "based_on_violet_brain": "1.2.0",
  "based_on_violet_ai_plugins": "1.0.0",
  "customizations": {
    "agents": "✅ v1.2.0 thin wrapper pattern",
    "patterns": "Custom 9D phase gates",
    "plugins": "All 37 enabled"
  },
  "last_sync": "2025-12-19",
  "sync_status": "Up to date"
}
```

When violet-brain updates to 1.3.0, you'll know exactly what changed and can update selectively.

---

## Questions?

- **How do I update my brain's agents?** → See Migration Checklist above
- **Which agents should I customize?** → See agents/references/README.md
- **How do patterns and systems/ relate?** → See patterns/thin-wrapper-agent-pattern.md
- **How do I stay in sync?** → See SYNC-GUIDE.md

