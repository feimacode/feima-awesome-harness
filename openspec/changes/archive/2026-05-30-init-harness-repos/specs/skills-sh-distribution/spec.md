## ADDED Requirements

### Requirement: Skills.sh.json manifest

The harness repo SHALL contain `skills.sh.json` at root following the skills.sh universal distribution schema.

#### Scenario: Skills.sh.json structure
- **WHEN** `skills.sh.json` is created
- **THEN** it SHALL contain `$schema` pointing to `https://skills.sh/schemas/skills.sh.schema.json`
- **AND** it SHALL contain `groupings` array with at least one grouping entry

#### Scenario: Grouping structure
- **WHEN** a grouping is defined in skills.sh.json
- **THEN** it SHALL contain `title`, `description`, and `skills` array
- **AND** each skill name SHALL match a directory name in `skills/`

### Requirement: Skills directory structure

Each skill SHALL be in a subdirectory of `skills/` with a `SKILL.md` file.

#### Scenario: Skill directory naming
- **WHEN** a skill is added to the harness
- **THEN** it SHALL be in `skills/<skill-name>/SKILL.md`
- **AND** the skill-name SHALL be kebab-case
- **AND** it SHALL match the name referenced in skills.sh.json

### Requirement: Universal install command

Users SHALL be able to install skills from the harness repo using `npx skills add`.

#### Scenario: Install from repo
- **WHEN** user runs `npx skills add feima/awesome-harness`
- **THEN** skills.sh CLI SHALL read skills.sh.json
- **AND** it SHALL copy SKILL.md files to `~/.claude/skills/` (and other tool directories)
- **AND** skills SHALL be available in all installed AI tools

### Requirement: Skills.sh.json limited to skills only

The skills.sh.json manifest SHALL only reference skills (SKILL.md files), not agents, flows, or other content types.

#### Scenario: No agents or flows in skills.sh
- **WHEN** skills.sh.json is created
- **THEN** it SHALL NOT reference agents or flows
- **AND** those content types SHALL only be distributed via platform plugin manifests