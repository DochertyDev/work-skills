# Skill catalog

Canonical skills live directly in `.agents/skills/`. Categories classify them without changing discovery paths. Compatibility below distinguishes documented file locations from runtime testing.

## software-development

| Name | Description | Category | Source / provenance | Compatibility |
| --- | --- | --- | --- | --- |
| [github-issue-to-pr](../.agents/skills/github-issue-to-pr/SKILL.md) | Implement an existing GitHub issue and deliver a validated, reviewable PR. | `software-development` | Original work authored in this repository; no third-party skill copied. MIT; license notice bundled in the skill folder. | Portable instructions; requires Git, project tooling, and authenticated GitHub access. Codex / Claude Code installation locations verified; runtime execution not yet tested. Hermes / Muse unverified. |

## salesforce-certinia

No skills yet.

## integrations-data

No skills yet.

## ai-agent-operations

No skills yet.

## documentation-analysis

No skills yet.

## project-workflow-management

No skills yet.

## Maintain this index

For each added skill, add one entry in its primary category with name, purpose, category, original source and license or permission status, relative `SKILL.md` link, and known compatibility limits. Match `metadata.category` and `metadata.source` to the entry. Update links and metadata together when renaming or recategorizing; remove entries when deleting skills. Retain the simple empty state for unused categories. The authoring template is not a catalog skill.

See [contributing](CONTRIBUTING.md) for validation and source requirements and [installation](INSTALLATION.md) for supported locations.
