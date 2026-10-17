# Work Skills

A public library of reusable AI agent skills, authored here or collected from sources that permit redistribution. Keep a canonical copy, discover useful workflows, and reuse individual skills across projects and computers.

Start with the [skill catalog](docs/CATALOG.md). The included workflow example, [github-issue-to-pr](.agents/skills/github-issue-to-pr/SKILL.md), guides an agent from an existing GitHub issue to a tested, reviewable pull request. Imported skills include [Unslop](.agents/skills/unslop/SKILL.md) for prose editing and [Caveman](.agents/skills/caveman/SKILL.md) for terse chat replies; each retains its upstream license and provenance.

## Repository layout

```text
work-skills/
|-- .agents/skills/
|   |-- caveman/            # SKILL.md, provenance, upstream licenses/notices
|   |-- github-issue-to-pr/ # SKILL.md, LICENSE
|   `-- unslop/             # SKILL.md, provenance, LICENSE, agents/openai.yaml
|-- docs/
|   |-- CATALOG.md
|   |-- CONTRIBUTING.md
|   `-- INSTALLATION.md
|-- templates/skill-template/SKILL.md
|-- AGENTS.md
|-- README.md
|-- LICENSE
|-- .gitignore
`-- .gitattributes
```

Each skill lives directly under `.agents/skills/` in its own named directory. Categories are metadata and catalog groupings, not subdirectories. The template stays under `templates/` so agents do not discover it as a usable skill.

## Use locally

```sh
git clone https://github.com/DochertyDev/work-skills.git
cd work-skills
```

In an existing clone, use `git pull --ff-only` to get published updates.

Codex can discover skills in this repository. To use a skill in another project, copy its entire directory into that project's `.agents/skills/` after checking for a name collision. See [installation and updates](docs/INSTALLATION.md) for verified Codex and Claude Code locations, synchronization guidance, and compatibility limits. Hermes and Muse integration is unverified.

## Add a skill

Copy [the template](templates/skill-template/SKILL.md) into `.agents/skills/<skill-name>/SKILL.md`, complete it, validate it, and update the catalog. Follow the [contribution guide](docs/CONTRIBUTING.md) and [agent instructions](AGENTS.md); submit one focused pull request targeting `main` and leave it unmerged.

The foundation intentionally has no installer, synchronization scripts, or CI framework. Skills use portable Markdown and Agent Skills frontmatter; agent-specific behavior still needs testing.

## Public content only

Everything committed here, including commit history, must be suitable for public distribution.

- Never include credentials, API keys, tokens, private keys, `.env` files, or private configuration.
- Keep internal project, and personal information out of skills, examples, screenshots, and attachments.
- Use synthetic examples and placeholders instead of real account details, internal URLs, or local user paths.
- Before adding third-party material, verify that redistribution is permitted, record its source and license, and preserve required notices. Public availability alone does not grant permission to copy.
- Review the exact staged diff before every commit and the outgoing commits before every push. Ignore rules help prevent accidents; they do not detect secrets or remove already tracked files.

Original repository content is licensed under the [MIT License](LICENSE). Preserve its copyright and license notice when copying it; original skills include a local license copy so the notice travels with them. Third-party skills and supporting files retain their own licenses and notices, which take precedence for that material; the root MIT license does not relicense imports. See [source requirements](docs/CONTRIBUTING.md#external-sources-and-licensing).
