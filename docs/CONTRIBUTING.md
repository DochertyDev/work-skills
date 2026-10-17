# Contributing skills

Read [AGENTS.md](../AGENTS.md) before editing. Keep one focused contribution per branch and PR. Every PR must target `main`; leave PRs unmerged unless authorized.

## Add or update a skill

1. Check the [catalog](CATALOG.md) for overlap and inspect the working tree. Fetch current refs and create a dedicated branch from `origin/main` without disturbing existing work. For dependent work, follow the PR targeting rules below.
2. Copy [the template](../templates/skill-template/SKILL.md) into `.agents/skills/<skill-name>/SKILL.md`. Use a unique lowercase kebab-case name matching the directory. Keep all skills immediately under `.agents/skills/`.
3. Replace the template's authoring prompts. Write `description` around what the skill does and the concrete requests that should activate it. For example, an issue-to-PR request should activate `github-issue-to-pr`; a request only to explain an issue should not. Keep the procedure concise, actionable, and focused on decisions an agent needs help making.
4. Choose one category from the catalog. Set string-valued `metadata.category` and `metadata.source` (`original` for work authored here; original public URL for sourced material). These are repository conventions within standard `metadata`, not new top-level fields. Record upstream revision and modifications for imported material, using metadata or a bundled provenance note when useful.
5. Add supporting files only when necessary: `references/` for conditional detail, `scripts/` for executable helpers, `assets/` for output resources. Link them relative to the skill root, document dependencies, and test helpers. Keep the skill usable when copied as a whole folder; do not rely on files elsewhere in this repository or local agent configuration.
6. Run the checks below, update the catalog entry, and review the complete diff for scope and public suitability.
7. Inspect `git status`, stage explicit paths, and inspect `git diff --cached` before committing. Review every outgoing commit before pushing. Open a PR explicitly targeting `main`, with purpose, activation examples, validation results, provenance, and compatibility limitations. Verify the base on GitHub before handing it over.

## PR targeting rules

All PRs target `main`, including work that depends on another open PR. Do not use another feature or foundation branch as a PR base.

For dependent work, explain the prerequisite in the description and keep the PR in draft. Once the prerequisite merges, update the working branch from current `main` using a method that preserves existing work, review the resulting diff, and rerun relevant validation before marking it ready. Do not merge prerequisites or rewrite published history without authorization.

With GitHub CLI, supply `--base main` when creating a PR and verify its stored target afterward:

```sh
gh pr view PR_NUMBER --json baseRefName
```

Replace `PR_NUMBER` with the created PR number. The result must report `main`. Check again before an authorized merge; if the target is wrong, correct it and review the updated diff before proceeding.

## Format and quality checks

The [Agent Skills specification](https://agentskills.io/specification) defines the format. Verify current rules when changing the standard. Repository rules:

- Start `SKILL.md` with a YAML mapping enclosed by `---` lines, followed by useful Markdown instructions.
- Require `name` (1-64 characters, lowercase letters, digits, hyphens; no leading, trailing, or repeated hyphens; matches directory) and `description` (1-1024 characters, nonempty).
- Keep classifications and provenance in `metadata` with string keys and values. Quote values that YAML might interpret as numbers, dates, or booleans. Supported optional standard fields include `license`, `compatibility`, and experimental `allowed-tools`; avoid agent-specific extensions unless needed and documented. Tool declarations do not replace user authorization.
- Parse frontmatter with a YAML parser, such as an already available `yaml.safe_load` in Python; check required types, lengths, name matching, and metadata against the catalog. Do not add a dependency framework solely to validate a Markdown contribution. If a parser is unavailable, use a trusted existing validator or report the limitation.
- Check every relative Markdown link from its containing file, every catalog path, and each referenced resource. Confirm only completed skills are in discovery paths; keep the template under `templates/` and never install it as a skill.
- Walk through a realistic activation request and a nearby non-activation request. Verify inputs, missing-access behavior, observable outputs, and authorization boundaries. Run a bounded practical trial where appropriate, using synthetic inputs; report static review separately from live agent testing.
- Review every included file for secrets, private prompts, personal information, machine-specific paths, internal hosts, proprietary content, and unfinished placeholders. Review scripts and assets too. Run `git diff --check` and review the final diff for unrelated changes.

For template changes, parse its YAML and check its structure and directory/name match; authoring replacement prompts are intentional only there. Do not mistake template validity for readiness to install.

## External sources and licensing

Public availability alone does not permit copying. Before importing any third-party material:

- Verify the exact material's license or obtain explicit redistribution permission, including scripts, assets, and documentation.
- Record the original source URL, applicable license and upstream revision; preserve required attribution, copyright, license text, and notices in the skill folder so they travel with copies.
- Use the optional `license` frontmatter field for the actual grant or a bundled license-file reference. Record changes and any restrictions in a provenance note when necessary.
- If rights are unclear or incompatible, do not import the material; link to the original source instead and explain the limitation in the PR.
- Never copy installed skills, agent configuration, private prompts, or credentials merely because they are locally readable. Review exact content and its rights before publication.

Original repository content uses the [MIT License](../LICENSE). For a new original skill, set `license: MIT` and copy the root `LICENSE` into its directory, retaining the notice and adding any applicable contributor notices. Do the same when completing the template so installed copies carry the grant. For imported skills, use the actual upstream license and bundle its required notices instead; do not replace them with MIT or assume the root license overrides them. `source: original` records provenance, not a separate license grant.
