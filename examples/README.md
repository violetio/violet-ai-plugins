# Example Settings Configurations

This directory contains example `.claude/settings.json` configurations for different Violet repositories.

## How to Use

1. Copy the appropriate example file for your repository
2. Place it at `.claude/settings.json` in your project root
3. Commit to version control (plugins configuration should be committed!)
4. Add `.claude/settings.json` to your CODEOWNERS file requiring eng-leads approval

## Examples by Repository

### BeamService
**Plugins**: Java/Spring, Beam domain, Shopify platform, core skills
```bash
cp examples/BeamService-settings.json /path/to/BeamService/.claude/settings.json
```

### prism-brain
**Plugins**: TypeScript/React, Next.js, Prism domain, core skills
```bash
cp examples/prism-brain-settings.json /path/to/prism-brain/.claude/settings.json
```

### EcomSyncService
**Plugins**: Java/Spring, all platform integrations, ecom-integrations domain, core skills
```bash
cp examples/EcomSyncService-settings.json /path/to/EcomSyncService/.claude/settings.json
```

### violet-brain
**Plugins**: All domain plugins (no stack-specific), core skills
```bash
cp examples/violet-brain-settings.json /path/to/violet-brain/.claude/settings.json
```

### violet-execution
**Plugins**: Python, core skills
```bash
cp examples/violet-execution-settings.json /path/to/violet-execution/.claude/settings.json
```

## Plugin Categories

### Always Include
- **agents**: Role personas for all projects
- **skill-***: Security, testing, observability, documentation
- **workflows**: Git conventions, 9D framework, review gates

### Stack-Specific (choose one)
- **stack-java-spring**: For Java/Spring Boot services
- **stack-python**: For Python SDKs and MCP servers
- **stack-typescript-react**: For React dashboards
- **stack-nextjs**: For Next.js applications

### Domain-Specific (choose relevant)
- **domain-prism**: Checkout orchestration
- **domain-beam**: Dropshipping and product sync
- **domain-relay**: Events and notifications
- **domain-track-and-field**: Affiliate tracking
- **domain-open-commerce**: MCP and AI operations
- **domain-ecom-integrations**: Integration patterns

### Platform-Specific (choose relevant)
- **platform-shopify**: Shopify API patterns
- **platform-bigcommerce**: BigCommerce enterprise features
- **platform-woocommerce**: WooCommerce/WordPress
- **platform-rithum**: Rithum B2B marketplace
- **platform-squarespace**: Squarespace commerce

## Governance

Changes to `.claude/settings.json` in any repo should:
1. Go through PR review
2. Require approval from `@violetio/eng-leads` (add to CODEOWNERS)
3. Be documented in the PR with rationale for plugin additions/removals

## Testing Locally

To test with a local copy of violet-ai-plugins before pushing to GitHub:

1. Clone violet-ai-plugins locally
2. Temporarily update the marketplace source:
```json
{
  "plugins": {
    "marketplace": {
      "source": "file:///absolute/path/to/violet-ai-plugins/.claude-plugin/marketplace.json"
    }
  }
}
```
3. Test your changes
4. Revert to GitHub URL before committing
