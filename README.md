# Violet Plugins

Internal Claude Code plugins for Violet.

## Quick Start

### Adding the Marketplace

If your project's `.claude/settings.json` includes this marketplace, it's already available.

**Manual addition:**
```bash
/plugin marketplace add violetio/violet-plugins
```

### Installing Plugins

```bash
# List available plugins
/plugin

# Install a plugin
/plugin install agents@violet

# Uninstall
/plugin uninstall agents
```

## Available Plugins (21)

### Agents (1)
| Plugin | Description |
|--------|-------------|
| `agents` | All role personas: engineering, PM, CS, finance, legal, brand |

### Stack (4)
| Plugin | Description |
|--------|-------------|
| `stack-java-spring` | Java/Spring Boot conventions |
| `stack-python` | Python conventions for SDKs and MCP |
| `stack-typescript-react` | TypeScript/React conventions |
| `stack-nextjs` | Next.js conventions |

### Domain (6)
| Plugin | Description |
|--------|-------------|
| `domain-prism` | Prism checkout orchestration |
| `domain-beam` | Beam dropshipping context |
| `domain-relay` | Relay events and notifications |
| `domain-track-and-field` | Affiliate tracking |
| `domain-open-commerce` | MCP and AI operations |
| `domain-ecom-integrations` | Integration patterns |

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

### Workflows (1)
| Plugin | Description |
|--------|-------------|
| `workflows` | Git, 9D framework, product discovery, review gates |

## Usage Examples

### Engineering Project (Java/Beam/Shopify)
```json
{
  "extraKnownMarketplaces": {
    "violet": { "source": { "source": "github", "repo": "violetio/violet-plugins" }}
  },
  "enabledPlugins": {
    "agents@violet": true,
    "workflows@violet": true,
    "skill-security@violet": true,
    "skill-testing@violet": true,
    "stack-java-spring@violet": true,
    "domain-beam@violet": true,
    "platform-shopify@violet": true
  }
}
```

### Product Brain (PM work)
```json
{
  "enabledPlugins": {
    "agents@violet": true,
    "workflows@violet": true,
    "skill-documentation@violet": true,
    "domain-prism@violet": true
  }
}
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Maintainers

- @violetio/eng-leads
