Help me create a new plugin for violet-ai-plugins.

Follow the agent-plugin-developer pattern:

1. Ask me what the plugin should do
2. Help me determine the correct category (agent, workflow, skill, domain, platform, stack) using the decision tree
3. Suggest a name following the `{category}-{name}` pattern
4. Create the plugin structure:
   - `plugins/{category}-{name}/.claude-plugin/plugin.json`
   - `plugins/{category}-{name}/knowledge/{topic}.md`
5. Add the plugin to `.claude-plugin/marketplace.json`
6. Update `README.md` with the new plugin

Use the patterns from existing plugins as reference. Ensure the knowledge content follows the category-specific patterns documented in agent-plugin-developer.
