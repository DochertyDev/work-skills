# Repository guidance

This repository is public. Publish only material that is safe and permitted to redistribute.

- Do not copy local agent configuration, installed skills, private prompts, credentials, or personal data into this repository without reviewing the exact content and confirming it is suitable for public release.
- Use synthetic data and portable placeholders in examples. Do not include real internal hosts, account identifiers, or machine-specific paths.
- For third-party skills, verify redistribution rights, record the original source and license, and preserve required attribution and notices. Do not assume a public page is freely licensed.
- Before committing, inspect `git status` and `git diff --cached`, check for sensitive content, and stage files explicitly by name.
- Before pushing, review all outgoing commits. Ignore rules are not a secret scanner.
- After initial setup, make changes on a focused branch and use a pull request. Leave pull requests unmerged unless the user authorizes merging.

## Skill conventions

- Keep canonical skills directly in `.agents/skills/<skill-name>/SKILL.md`. Do not add category directories or duplicate agent-specific copies here.
- Copy `templates/skill-template/SKILL.md` for new skills; complete all replacement guidance before publishing. Keep the template outside skill discovery paths.
- Use unique lowercase kebab-case names of 1-64 characters, matching the directory; no leading, trailing, or consecutive hyphens.
- Require YAML `name` and a nonempty `description` of at most 1024 characters explaining capability and activation. Use supported Agent Skills fields only; put repository classifications in string-valued `metadata`.
- Set `metadata.category` to a category from `docs/CATALOG.md` and `metadata.source` to `original` or the original public source URL. Record licensing and attribution as described in `docs/CONTRIBUTING.md`; never invent a license grant.
- Write concise, actionable workflows with inputs, outputs, quality checks, and relevant authorization boundaries. Preserve the user's scope and existing work. Skills do not grant credentials or action permissions.
- Keep skills self-contained and portable. Link supporting resources relative to the skill directory. Add `references/`, `scripts/`, or `assets/` only for actual needs; document dependencies and test executable helpers.
- Update `docs/CATALOG.md` when adding, renaming, removing, or changing a skill's purpose or compatibility. Keep paths and provenance consistent with frontmatter.

## Validation and delivery

- Follow `docs/CONTRIBUTING.md` for frontmatter, link, discovery, privacy, and behavioral checks. Validate both the example and template after changing the standard; distinguish intentional template prompts from unfinished published skills.
- Confirm installation claims against current official documentation. Label untested runtime behavior and unverified agent integrations explicitly.
- Preserve existing ignore rules, line-ending rules, attribution, and public-distribution safeguards. Review every supporting file as carefully as `SKILL.md`.
- Use a focused branch off the intended base, stage explicit file paths, review the full staged diff and all outgoing commits, and create one focused PR. Do not introduce installers, CI, bulk imports, or extra examples without a separate request.
