# Changelog

All notable changes to violet-ai-plugins.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)

---

## [0.0.5] - 2025-12-31

### Added
- `rca-analyst` skill with three-test isolation methodology for root cause analysis
- `/v:rca` command to conduct structured RCAs interactively

---

## [0.0.4] - 2025-12-31

### Added
- CHANGELOG.md with full history backfill
- Release process documentation in CONTRIBUTING.md

### Changed
- CLAUDE.md now routes to CONTRIBUTING.md for detailed guidance
- CONTRIBUTING.md rewritten to match current plugin structure

### Fixed
- Version mismatch between plugin.json and marketplace.json

---

## [0.0.3] - 2025-12-30

### Added
- Unified `v` plugin with discovery + definition phases (379e279)
- `/v:help` command for user documentation (bfddd31)

### Changed
- Archived violet-brain agents for reference (1074c2d)

### Fixed
- Product descriptions in domain selection (96932d5)

---

## [0.0.2] - 2025-12-17

### Added
- Domain-violet-dashboard plugin with code patterns (49bfed4)
- v-plugins plugin for plugin management (976d811)
- Brain vs code repo architecture with context awareness (4f2da21)
- Pattern-extraction skill for v-documentation (8539a3e)
- Model selection guidance and agent coordination syntax (64ed834)
- GitHub review response responsibility for v-tech-lead (795cbd5)
- NO MUI policy with design system guidance for v-dashboard (45deb26)
- Screenshot capture to v-frontend UI workflow (3b73677)

### Changed
- Renamed all plugins to v-* prefix (231f221)
- Consolidated convo plugins into single workflow-convo (345c53b)
- Converted knowledge/ to skills/ for Claude Code compatibility (3a43315)

### Fixed
- Activity log update requirements in convo-exit workflow (7199995)
- Updated marketplace.json with consolidated workflow-convo (f78122a)
- Removed complex bash patterns from command prompts (a71425c)

---

## [0.0.1] - 2025-12-16

### Added
- Initial violet-ai-plugins marketplace (f005257, aa59a10)
- 21 bundled plugins for Violet development workflows
- CLAUDE.md with plugin instructions
- Slash commands for day-start, day-end, convo management
- Meta agents, assistant, and conversation management plugins (e856c65)
- Inbox to day-start routine (d9b0bce)
- Conversation management to workflows plugin (1f1a386)
- ROLLOUT guide with next steps and git status (b78da58)

### Changed
- Broke bundled plugins into individual plugins (b5678a1)
- Split documentation standards into external vs internal (b9f6c5f)

### Fixed
- Plugin configuration format in README examples (8cd9d07)
