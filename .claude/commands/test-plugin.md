Validate a plugin before deployment.

Follow the agent-plugin-tester pattern:

1. Ask which plugin to test (or detect from context)
2. Run structure validation:
   - Directory follows `plugins/{category}-{name}/` pattern
   - `.claude-plugin/plugin.json` exists with required fields
   - `knowledge/` directory has at least one .md file
   - Name in plugin.json matches directory name
3. Run content validation for the plugin's category:
   - Agents: Role, System Prompt, Scope, Process, Output Format, Trigger
   - Workflows: Phases with triggers/actions/outputs
   - Skills: Core principles, patterns with examples
   - Domains: Overview, data models, workflows
   - Platforms: Overview, API patterns, authentication
   - Stacks: Overview, project structure, code patterns
4. Check marketplace entry exists in `.claude-plugin/marketplace.json`
5. Verify README.md includes the plugin

Output a test report with pass/fail status and any issues found.
