# Violet AI Plugins Rollout Guide

## Immediate Next Steps (Resume Here)

1. **Create GitHub repository**: Create `https://github.com/violetio/violet-ai-plugins` (if not already created)
2. **Push to GitHub**: Run `git push -u origin main` to publish the initial commit
3. **Begin pilot rollout**: Start with BeamService (see Phase 3 below)

The local repo has been initialized with all files committed (commit hash: `aa59a10`). Remote `git@github.com:violetio/violet-ai-plugins.git` is configured but the repository doesn't exist on GitHub yet.

## Current Status

The violet-ai-plugins repository is complete with:
- ✅ 21 plugins across 6 categories
- ✅ Comprehensive knowledge files
- ✅ Marketplace configuration
- ✅ Example settings for all major repos
- ✅ Agents reorganized into 8 categories (engineering, product, customer-success, finance, legal, brand, documentation, security)
- ✅ Customer Docs Agent enhanced with Gitbook/Mintlify conventions and templates
- ✅ Local git repository initialized with initial commit (aa59a10)
- ⏳ **NEXT: Create GitHub repository and push to origin**

## Prerequisites for GitHub URL to Work

The GitHub URL in the example settings will work once:

1. **Repository is pushed to GitHub**: `https://github.com/violetio/violet-ai-plugins`
2. **Repository is public** OR **users have access** (if private)
3. **Main branch exists** with the `.claude-plugin/marketplace.json` file

## Testing the Setup

### Before Pushing to GitHub (Local Testing)

To test locally before publishing:

```bash
# In any Violet repo (e.g., BeamService)
cd /path/to/BeamService

# Create .claude directory if it doesn't exist
mkdir -p .claude

# Create settings.json with LOCAL path
cat > .claude/settings.json << 'EOF'
{
  "plugins": {
    "marketplace": {
      "source": "file:///Users/brandonschulz/code/violetio/violet-ai-plugins/.claude-plugin/marketplace.json"
    },
    "installed": [
      "agents",
      "stack-java-spring",
      "domain-beam",
      "platform-shopify",
      "workflows"
    ]
  }
}
EOF

# Test with Claude Code
# Claude Code should now load the plugins from your local violet-ai-plugins directory
```

### After Pushing to GitHub

Once the repo is on GitHub, update settings to use the GitHub URL:

```json
{
  "plugins": {
    "marketplace": {
      "source": "https://raw.githubusercontent.com/violetio/violet-ai-plugins/main/.claude-plugin/marketplace.json"
    },
    "installed": ["agents", "stack-java-spring", "domain-beam"]
  }
}
```

## Step-by-Step Rollout

### Phase 1: Initialize Git Repository

```bash
cd /Users/brandonschulz/code/violetio/violet-ai-plugins

# Initialize git if not already done
git init

# Add all files
git add .

# Create initial commit
git commit -m "feat: initial violet plugins marketplace

- 21 plugins across 6 categories (agents, stack, domain, platform, skills, workflows)
- Comprehensive knowledge files for all plugins
- Platform plugins: Shopify, BigCommerce, WooCommerce, Rithum, Squarespace
- Example settings for all major Violet repos"
```

### Phase 2: Push to GitHub

```bash
# Create repo on GitHub first at: https://github.com/violetio/violet-ai-plugins
# Then add remote and push

git remote add origin git@github.com:violetio/violet-ai-plugins.git
git branch -M main
git push -u origin main
```

### Phase 3: Pilot Rollout (One Repo at a Time)

Start with one repo to validate the system works:

#### Option A: BeamService (Recommended first)

```bash
cd /path/to/BeamService

# Copy example settings
cp /path/to/violet-ai-plugins/examples/BeamService-settings.json .claude/settings.json

# Test Claude Code loads plugins correctly
# Work on a small task to verify plugins are active

# Create PR
git checkout -b feat/add-claude-plugins
git add .claude/settings.json
git commit -m "feat: add Claude Code plugin configuration"
git push origin feat/add-claude-plugins

# Get PR approved by eng-leads
# Merge when validated
```

#### Option B: Test with Local Path First

If you want to test before GitHub push:

```bash
cd /path/to/BeamService
mkdir -p .claude

# Use LOCAL marketplace URL for testing
cat > .claude/settings.json << 'EOF'
{
  "plugins": {
    "marketplace": {
      "source": "file:///Users/brandonschulz/code/violetio/violet-ai-plugins/.claude-plugin/marketplace.json"
    },
    "installed": [
      "agents",
      "stack-java-spring",
      "domain-beam",
      "domain-ecom-integrations",
      "platform-shopify",
      "skill-security",
      "skill-testing",
      "skill-observability",
      "skill-documentation",
      "workflows"
    ]
  }
}
EOF

# Test thoroughly
# Once validated, update to GitHub URL and commit
```

### Phase 4: Add CODEOWNERS

In each repo, update `.github/CODEOWNERS`:

```
# .claude/settings.json requires eng-leads approval
/.claude/settings.json @violetio/eng-leads
```

### Phase 5: Rollout to All Repos

After pilot validation, roll out to remaining repos:

1. **EcomSyncService** (all platform plugins)
2. **prism-brain** (TypeScript/React, Prism domain)
3. **violet-brain** (all domains)
4. **violet-execution** (Python)
5. Remaining repos as needed

## Validation Checklist

For each repo after rollout:

- [ ] `.claude/settings.json` created with correct plugins
- [ ] CODEOWNERS updated to require eng-leads approval
- [ ] Claude Code successfully loads plugins (no errors in console)
- [ ] Test that domain knowledge is available (ask Claude a platform-specific question)
- [ ] Test that agents are available (try "act as backend-engineer")
- [ ] Test that workflows are available (try /git command)
- [ ] PR created and approved
- [ ] Changes merged to main

## Access Requirements

### For Public Repository
- No special access needed
- Anyone can use the plugins via the GitHub URL

### For Private Repository
- Users need read access to violetio/violet-ai-plugins repo
- GitHub personal access token may be required
- Claude Code may prompt for authentication

## Troubleshooting

### "Failed to load marketplace"
- Check that GitHub URL is correct
- Verify repo is public OR user has access
- Check that .claude-plugin/marketplace.json exists in repo

### "Plugin not found"
- Verify plugin name is spelled correctly in installed array
- Check that plugin exists in marketplace.json
- Ensure plugin directory has plugin.json file

### "Knowledge files not loading"
- Check that plugin.json has correct knowledge path
- Verify knowledge files exist in plugin directory
- Check file permissions

### Plugins not updating
- Claude Code caches plugins - may need to restart
- Check Claude Code version supports plugin marketplace
- Verify marketplace URL is accessible

## Success Metrics

Track these after rollout:

1. **Adoption**: Number of repos with plugins configured
2. **Usage**: Frequency of agent/workflow commands
3. **Quality**: Reduction in platform-specific errors
4. **Feedback**: Developer satisfaction with plugin knowledge

## Next Steps

After successful rollout:

1. **Monitor usage**: Track which plugins are most valuable
2. **Iterate**: Add/update knowledge based on team feedback
3. **Expand**: Add plugins for new platforms/domains
4. **Document**: Update this guide with lessons learned
