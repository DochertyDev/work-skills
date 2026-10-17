# Work Skills

A public library of reusable AI agent skills, authored here or collected from sources that permit redistribution. Keep a canonical copy, discover useful workflows, and reuse individual skills across projects and computers.

Browse the [skill folders](.agents/skills/). The included workflow example, [github-issue-to-pr](.agents/skills/github-issue-to-pr/SKILL.md), guides an agent from an existing GitHub issue to a tested, reviewable pull request. Imported skills include [Unslop](.agents/skills/unslop/SKILL.md) for prose editing and [Caveman](.agents/skills/caveman/SKILL.md) for terse chat replies; each retains its upstream license and provenance.

[Session Handoff](.agents/skills/session-handoff/SKILL.md) preserves or resumes task context across sessions. [Requirements Clarity](.agents/skills/requirements-clarity/SKILL.md) turns consequential ambiguity into scoped requirements and acceptance criteria. Both are selectively adapted from softaworks/agent-toolkit under MIT, with pinned provenance and upstream license copies. They require no upstream scripts; live agent behavior remains unverified. Each folder's provenance note records the adaptation and validation limits.

## Repository layout

```text
work-skills/
|-- .agents/skills/
|   |-- caveman/            # SKILL.md, provenance, upstream licenses/notices
|   |-- github-issue-to-pr/ # SKILL.md, LICENSE
|   |-- requirements-clarity/ # SKILL.md, provenance, upstream LICENSE
|   |-- session-handoff/   # SKILL.md, provenance, upstream LICENSE
|   `-- unslop/             # SKILL.md, provenance, LICENSE, agents/openai.yaml
|-- templates/skill-template/SKILL.md
|-- AGENTS.md
|-- README.md
|-- LICENSE
|-- .gitignore
`-- .gitattributes
```

Each skill lives directly under `.agents/skills/` in its own named directory. Categories are metadata, not subdirectories. The template stays under `templates/` so agents do not discover it as a usable skill. The local `docs/` directory is ignored; catalog updates are not required.

## Use locally

```sh
git clone https://github.com/DochertyDev/work-skills.git
cd work-skills
```

In an existing clone, use `git pull --ff-only` to get published updates.

Codex can discover skills in this repository. To use a skill in another project, copy its entire directory into that project's `.agents/skills/` after checking for a name collision. The historical [installation guide](https://github.com/DochertyDev/work-skills/blob/1d33d68/docs/INSTALLATION.md) records earlier location and synchronization checks; it is no longer maintained here. Hermes and Muse integration is unverified.

## Add a skill

Copy [the template](templates/skill-template/SKILL.md) into `.agents/skills/<skill-name>/SKILL.md`, complete it, and validate it. Follow the [agent instructions](AGENTS.md); submit one focused pull request targeting `main` and leave it unmerged. No catalog update is required.

The foundation intentionally has no installer, synchronization scripts, or CI framework. Skills use portable Markdown and Agent Skills frontmatter; agent-specific behavior still needs testing.

## Public content only

Everything committed here, including commit history, must be suitable for public distribution.

- Never include credentials, API keys, tokens, private keys, `.env` files, or private configuration.
- Keep internal project, and personal information out of skills, examples, screenshots, and attachments.
- Use synthetic examples and placeholders instead of real account details, internal URLs, or local user paths.
- Before adding third-party material, verify that redistribution is permitted, record its source and license, and preserve required notices. Public availability alone does not grant permission to copy.
- Review the exact staged diff before every commit and the outgoing commits before every push. Ignore rules help prevent accidents; they do not detect secrets or remove already tracked files.

Original repository content is licensed under the [MIT License](LICENSE). Preserve its copyright and license notice when copying it; original skills include a local license copy so the notice travels with them. Third-party skills and supporting files retain their own licenses and notices, which take precedence for that material; the root MIT license does not relicense imports. See [AGENTS.md](AGENTS.md) for source requirements.
