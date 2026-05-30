## ADDED Requirements

### Requirement: Claude Code plugin manifests

The harness repo SHALL contain `.claude-plugin/marketplace.json` and `.claude-plugin/plugin.json` following the Claude Code marketplace schema.

#### Scenario: marketplace.json structure
- **WHEN** `.claude-plugin/marketplace.json` is created
- **THEN** it SHALL contain `name`, `description`, `owner`, and `plugins` array with at least one plugin entry pointing to `source: "./"`

#### Scenario: plugin.json structure
- **WHEN** `.claude-plugin/plugin.json` is created
- **THEN** it SHALL contain `name`, `description`, `version`, `author`, `homepage`, `repository`, `license`, and `keywords` fields
- **AND** it MAY contain `skills`, `agents`, `hooks` directory references

### Requirement: Cursor plugin manifest

The harness repo SHALL contain `.cursor-plugin/plugin.json` following the Cursor IDE plugin schema.

#### Scenario: Cursor plugin.json structure
- **WHEN** `.cursor-plugin/plugin.json` is created
- **THEN** it SHALL contain `name`, `displayName`, `description`, `version`, `author`, `homepage`, `repository`, `license`, and `keywords` fields
- **AND** it SHALL contain `skills`, `agents`, `commands` directory references
- **AND** it MAY contain `hooks` file reference pointing to a specific hooks JSON file

### Requirement: Codex plugin manifest with interface section

The harness repo SHALL contain `.codex-plugin/plugin.json` following the Codex plugin schema including the `interface` section for UI metadata.

#### Scenario: Codex plugin.json structure
- **WHEN** `.codex-plugin/plugin.json` is created
- **THEN** it SHALL contain `name`, `version`, `description`, `author`, `homepage`, `repository`, `license`, and `keywords` fields
- **AND** it SHALL contain `skills` directory reference
- **AND** it SHALL contain `interface` section with `displayName`, `shortDescription`, `longDescription`, `developerName`, `category`, `capabilities`, `defaultPrompt`, `websiteURL`, `privacyPolicyURL`, `termsOfServiceURL`, `brandColor`, `composerIcon`, and `logo` fields

#### Scenario: Codex asset files
- **WHEN** `interface.composerIcon` or `interface.logo` reference asset files
- **THEN** those files SHALL exist in the `assets/` directory

### Requirement: Shared content directories

All plugin manifests SHALL reference the same content directories (`./skills/`, `./agents/`, `./flows/`, `./commands/`, `./hooks/`) ensuring content is defined once and used by all platforms.

#### Scenario: Skills directory reference
- **WHEN** any plugin.json specifies `skills: "./skills/"`
- **THEN** the `skills/` directory SHALL exist at the repo root
- **AND** all plugin.json files SHALL use the same `./skills/` path

#### Scenario: Agents directory reference
- **WHEN** any plugin.json specifies `agents: "./agents/"`
- **THEN** the `agents/` directory SHALL exist at the repo root
- **AND** all plugin.json files SHALL use the same `./agents/` path

### Requirement: Platform-specific directories exist

The harness repo SHALL contain all directories referenced by plugin manifests even if initially empty.

#### Scenario: Required directories
- **WHEN** plugin manifests reference `commands/`, `hooks/`, or `assets/`
- **THEN** those directories SHALL exist at the repo root
- **AND** they MAY be empty initially

### Requirement: No catalog directory in content repo

The content repo SHALL NOT contain a `catalogs/` directory because catalog metadata lives in the catalog service (Model A architecture).

#### Scenario: Catalogs directory removed
- **WHEN** initializing feima-awesome-harness
- **THEN** any existing `catalogs/` directory SHALL be removed
- **AND** no catalog.json file SHALL exist at repo root