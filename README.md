# Violet AI Plugins

AI-powered knowledge and agent plugins for Violet, compatible with Claude Code and other AI systems.

## Quick Start

### Adding the Marketplace

If your project's `.claude/settings.json` includes this marketplace, it's already available.

**Manual addition:**
```bash
/plugin marketplace add violetio/violet-ai-plugins
```

### Installing Plugins

```bash
# List available plugins
/plugin

# Install a plugin
/plugin install agent-backend-engineer@violet

# Uninstall
/plugin uninstall agent-backend-engineer
```

## Available Plugins (53)

### Agents (26)

| Plugin | Description |
|--------|-------------|
| `agent-architect` | System architecture and technical design |
| `agent-backend-engineer` | Server-side implementation and APIs |
| `agent-brand-guidelines` | Brand voice and visual identity guidelines |
| `agent-commerce-pm` | Commerce product management |
| `agent-contract-analyst` | Contract review and legal analysis |
| `agent-customer-docs-agent` | Customer-facing documentation |
| `agent-customer-success` | Customer success and relationship management |
| `agent-data-engineer` | Data pipelines and analytics infrastructure |
| `agent-experience-designer` | UX/UI design and user experience |
| `agent-finance-analyst` | Financial analysis and reporting |
| `agent-finance-engineer` | Financial systems and cost engineering |
| `agent-frontend-engineer` | Frontend implementation and UI |
| `agent-infrastructure-engineer` | Infrastructure, DevOps, and platform reliability |
| `agent-internal-docs-agent` | Internal documentation and knowledge base |
| `agent-onboarding-agent` | Customer onboarding specialist |
| `agent-partnership-manager` | Partnership development and management |
| `agent-platform-pm` | Platform product management |
| `agent-project-coordinator` | Project coordination and task management |
| `agent-qa-engineer` | Quality assurance and testing |
| `agent-security-privacy` | Security and privacy engineering |
| `agent-support-agent` | Customer support and issue resolution |
| `agent-tech-lead` | Technical leadership and code review |
| `agent-plugin-developer` | Create and maintain Claude Code plugins |
| `agent-plugin-tester` | Validate and test plugins before deployment |
| `agent-feedback-collector` | Systematically gather and act on user feedback |
| `agent-assistant` | Personal assistant for daily routines and productivity |

### Workflows (7)

| Plugin | Description |
|--------|-------------|
| `workflow-9d-framework` | 9D product development framework |
| `workflow-convo-start` | Conversation start protocols for understanding and alignment |
| `workflow-convo-capture` | Mid-conversation context capture for continuity |
| `workflow-convo-exit` | Conversation exit protocol for debrief and improvement |
| `workflow-git-conventions` | Git workflow and commit conventions |
| `workflow-product-discovery` | Product discovery and validation process |
| `workflow-review-gates` | Quality gates and review processes |

### Stack (4)

| Plugin | Description |
|--------|-------------|
| `stack-java-spring` | Java/Spring Boot conventions |
| `stack-python` | Python conventions for SDKs and MCP |
| `stack-typescript-react` | TypeScript/React conventions |
| `stack-nextjs` | Next.js conventions |

### Domain (7)

| Plugin | Description |
|--------|-------------|
| `domain-prism` | Prism checkout orchestration |
| `domain-beam` | Beam dropshipping context |
| `domain-relay` | Relay events and notifications |
| `domain-track-and-field` | Affiliate tracking |
| `domain-open-commerce` | MCP and AI operations |
| `domain-ecom-integrations` | Integration patterns |
| `domain-violet-dashboard` | VioletDashboard code patterns |

### Platform (5)

| Plugin | Description |
|--------|-------------|
| `platform-shopify` | Shopify API patterns |
| `platform-bigcommerce` | BigCommerce API patterns |
| `platform-woocommerce` | WooCommerce API patterns |
| `platform-rithum` | Rithum B2B patterns |
| `platform-squarespace` | Squarespace API patterns |

### Skills (4)

| Plugin | Description |
|--------|-------------|
| `skill-security` | Security fundamentals |
| `skill-testing` | Testing patterns |
| `skill-observability` | Logging, tracing, metrics |
| `skill-documentation` | Documentation standards |

## Usage Examples

### Engineering Project (Java/Beam/Shopify)

Create `.claude/settings.json`:
```json
{
  "enabledPlugins": {
    "agent-backend-engineer@violet": true,
    "agent-architect@violet": true,
    "agent-tech-lead@violet": true,
    "workflow-git-conventions@violet": true,
    "workflow-conversation-management@violet": true,
    "skill-security@violet": true,
    "skill-testing@violet": true,
    "stack-java-spring@violet": true,
    "domain-beam@violet": true,
    "platform-shopify@violet": true
  },
  "extraKnownMarketplaces": {
    "violet": {
      "source": {
        "source": "github",
        "repo": "violetio/violet-ai-plugins"
      }
    }
  }
}
```

### Product Brain (PM work)

Create `.claude/settings.json`:
```json
{
  "enabledPlugins": {
    "agent-commerce-pm@violet": true,
    "agent-experience-designer@violet": true,
    "workflow-9d-framework@violet": true,
    "workflow-product-discovery@violet": true,
    "workflow-conversation-management@violet": true,
    "skill-documentation@violet": true,
    "domain-prism@violet": true
  },
  "extraKnownMarketplaces": {
    "violet": {
      "source": {
        "source": "github",
        "repo": "violetio/violet-ai-plugins"
      }
    }
  }
}
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Maintainers

- @violetio/eng-leads
