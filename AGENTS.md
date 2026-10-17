# Repository guidance

This repository is public. Publish only material that is safe and permitted to redistribute.

- Do not copy local agent configuration, installed skills, private prompts, credentials, or personal data into this repository without reviewing the exact content and confirming it is suitable for public release.
- Use synthetic data and portable placeholders in examples. Do not include real internal hosts, account identifiers, or machine-specific paths.
- For third-party skills, verify redistribution rights, record the original source and license, and preserve required attribution and notices. Do not assume a public page is freely licensed.
- Before committing, inspect `git status` and `git diff --cached`, check for sensitive content, and stage files explicitly by name.
- Before pushing, review all outgoing commits. Ignore rules are not a secret scanner.
- After initial setup, make changes on a focused branch and use a pull request. Every PR in this repository must target `main`; never target another feature or foundation branch. Leave pull requests unmerged unless the user authorizes merging.
- Start independent work from current `origin/main`. If work depends on an unmerged PR, still target `main`, explain the dependency, and keep the dependent PR in draft until its prerequisite is merged and its diff is reviewed against updated `main`.
- Set the PR base explicitly (for GitHub CLI, `gh pr create --base main`) and verify GitHub reports `baseRefName: main` after creation and before delivery. Recheck the base before any authorized merge. Correct a wrong target before reporting the PR ready.

## Skill conventions

- Keep canonical skills directly in `.agents/skills/<skill-name>/SKILL.md`. Do not add category directories or duplicate agent-specific copies here.
- Copy `templates/skill-template/SKILL.md` for new skills; complete all replacement guidance before publishing. Keep the template outside skill discovery paths.
- Use unique lowercase kebab-case names of 1-64 characters, matching the directory; no leading, trailing, or consecutive hyphens.
- Require YAML `name` and a nonempty `description` of at most 1024 characters explaining capability and activation. Use supported Agent Skills fields only; put repository classifications in string-valued `metadata`.
- Set `metadata.category` to a descriptive category string and `metadata.source` to `original` or the original public source URL. Original content uses MIT; bundle a copy of the root `LICENSE` with original skills. Imported material retains its upstream terms. Record the upstream revision, modifications, licensing, and attribution in the skill folder; never invent a license grant.
- Write concise, actionable workflows with inputs, outputs, quality checks, and relevant authorization boundaries. Preserve the user's scope and existing work. Skills do not grant credentials or action permissions.
- Keep skills self-contained and portable. Link supporting resources relative to the skill directory. Add `references/`, `scripts/`, or `assets/` only for actual needs; document dependencies and test executable helpers.
- Keep skill paths and provenance consistent with frontmatter. Catalog updates are not required; `docs/` remains ignored.

## Validation and delivery

- Parse YAML frontmatter and check required types, lengths, naming, and string-valued metadata. Check relative links against files included in the contribution, discovery locations, privacy, and realistic activating/non-activating requests. Validate both the example and template after changing the standard; distinguish intentional template prompts from unfinished published skills. Report static review separately from live runtime testing.
- Confirm installation claims against current official documentation. Label untested runtime behavior and unverified agent integrations explicitly.
- Preserve existing ignore rules, line-ending rules, attribution, and public-distribution safeguards. Review every supporting file as carefully as `SKILL.md`.
- Use a focused branch off the intended base, stage explicit file paths, review the full staged diff and all outgoing commits, and create one focused PR. Do not introduce installers, CI, bulk imports, or extra examples without a separate request.
