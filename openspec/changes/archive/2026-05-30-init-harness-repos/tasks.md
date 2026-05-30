## 1. feima-awesome-harness Setup

- [x] 1.1 Remove existing `catalogs/` directory (Model A architecture - catalog metadata lives in catalog service)
- [x] 1.2 Create `.claude-plugin/marketplace.json` with feima harness marketplace metadata
- [x] 1.3 Create `.claude-plugin/plugin.json` with skills, agents, flows directory references
- [x] 1.4 Create `.cursor-plugin/plugin.json` with displayName, skills, agents, commands, hooks references
- [x] 1.5 Create `.codex-plugin/plugin.json` with interface section (displayName, category, capabilities, brandColor, composerIcon, logo)
- [x] 1.6 Create `skills.sh.json` with $schema and groupings array
- [x] 1.7 Create `assets/` directory for Codex icons/branding
- [x] 1.8 Create placeholder asset files (app-icon.png, brand-small.svg) in assets/
- [x] 1.9 Create `commands/` directory (referenced by Cursor plugin.json)
- [x] 1.10 Create `hooks/` directory with placeholder hooks-cursor.json (referenced by Cursor plugin.json)

## 2. feima-awesome-harness Placeholder Content

- [x] 2.1 Create `skills/incident-triage/SKILL.md` placeholder skill
- [x] 2.2 Create `agents/incident-commander.agent.md` placeholder agent
- [x] 2.3 Create `flows/war-room-triage.flow.yaml` placeholder flow
- [x] 2.4 Create `prompts/status-update.prompt.md` placeholder prompt
- [x] 2.5 Validate skills.sh.json references match created skill directory name
- [x] 2.6 Validate all plugin.json files reference correct directory paths

## 3. feima-harness-catalog Setup

- [x] 3.1 Create `catalogs/feima-awesome-harness/` directory
- [x] 3.2 Create `catalogs/feima-awesome-harness/catalog.json` with feima content metadata
- [x] 3.3 Create `catalogs/community/` directory (empty, for future contributors)
- [x] 3.4 Create `scripts/` directory
- [x] 3.5 Create `scripts/build-index.js` that merges catalog.json files into index.json
- [x] 3.6 Create placeholder `index.json` with feima-awesome-harness entry
- [x] 3.7 Create `.github/` directory structure
- [x] 3.8 Create `.github/workflows/build-index.yml` GitHub Actions workflow
- [x] 3.9 Create `README.md` explaining catalog registration process

## 4. Validation

- [x] 4.1 Verify feima-awesome-harness plugin.json schemas match superpowers reference
- [x] 4.2 Verify skills.sh.json schema is valid against skills.sh schema URL
- [x] 4.3 Verify catalog.json schema follows CATALOG_ECOSYSTEM.md format
- [x] 4.4 Test that all directory references in plugin.json files exist
- [x] 4.5 Test that skills.sh.json skill names match skills/ directory names