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

| Name | Description | Category | Source / provenance | Compatibility |
| --- | --- | --- | --- | --- |
| [caveman](../.agents/skills/caveman/SKILL.md) | Terse chat voice that removes filler while preserving technical facts. | `ai-agent-operations` | Julius Brussee, [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman); Apache-2.0 with upstream notices. [Pinned source and changes](../.agents/skills/caveman/PROVENANCE.md). | Core instruction-only skill; persists until stopped. No hooks, extra modes, proxy, or token-savings guarantee. Live agent behavior untested. |

## documentation-analysis

| Name | Description | Category | Source / provenance | Compatibility |
| --- | --- | --- | --- | --- |
| [unslop](../.agents/skills/unslop/SKILL.md) | Edit prose to remove AI writing patterns while preserving meaning and tone. | `documentation-analysis` | Lauren Tan (poteto), [pstack in cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop); MIT. [Pinned source and changes](../.agents/skills/unslop/PROVENANCE.md). | Explicit invocation preserved in Codex policy and portable body guidance. No pstack plugin or scripts required. Other harness policy enforcement and live agent behavior untested. |

## project-workflow-management

No skills yet.

## Maintain this index

For each added skill, add one entry in its primary category with name, purpose, category, original source and license or permission status, relative `SKILL.md` link, and known compatibility limits. Match `metadata.category` and `metadata.source` to the entry. Update links and metadata together when renaming or recategorizing; remove entries when deleting skills. Retain the simple empty state for unused categories. The authoring template is not a catalog skill.

See [contributing](CONTRIBUTING.md) for validation and source requirements and [installation](INSTALLATION.md) for supported locations.
