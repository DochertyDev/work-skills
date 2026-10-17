# Installation and updates

## Clone the canonical repository

```sh
git clone https://github.com/DochertyDev/work-skills.git
cd work-skills
git status --short
git pull --ff-only
```

For an existing clone, check status before pulling; preserve local edits. If a fast-forward fails, inspect divergence rather than resetting or overwriting work. These commands use the checked-out branch; switch to the published default branch for released skills when safe. A skill being developed in an unmerged PR is not yet published on that branch.

Browse the [catalog](CATALOG.md), read the selected `SKILL.md` and supporting files, and check licensing and required permissions before use. Installation does not grant credentials or authorize the actions described by a skill.

## Codex

The [official skills documentation](https://learn.chatgpt.com/docs/build-skills) confirms repository discovery from `.agents/skills/` between the working directory and Git root, and user discovery at `~/.agents/skills/`.

- In this clone, launch Codex in the repository to discover its completed skills.
- In another project, copy the entire selected folder, such as `.agents/skills/github-issue-to-pr/`, into that project's `.agents/skills/`.
- For personal reuse across projects, place that folder in `~/.agents/skills/`.

Check whether the destination exists before copying; stop on collisions and compare contents. Never overwrite or merge directories blindly. Invoke with `$github-issue-to-pr` and an issue URL. Codex also supports symlinked skill folders; links require OS support and a stable clone location. Restart Codex if changes do not appear.

## Claude Code

The [official Claude Code skills documentation](https://code.claude.com/docs/en/skills) confirms project skills at `.claude/skills/<skill-name>/SKILL.md` and personal skills at `~/.claude/skills/<skill-name>/SKILL.md`.

Copy the entire selected skill folder into one of those locations after checking for collisions. Invoke with `/github-issue-to-pr` and an issue URL. Keep any project copy in the consuming project; do not duplicate canonical skills into `.claude/` in this repository. The canonical `.agents/skills/` layout alone is not a claim of Claude Code discovery.

## Compatibility status

| Agent | Status |
| --- | --- |
| Codex | Discovery locations and invocation verified against official documentation on 2026-10-17; no live installation or issue-to-PR trial performed. |
| Claude Code | Installation locations and invocation verified against official documentation on 2026-10-17; no live trial performed. |
| Hermes | Integration and installation unverified; consult current agent documentation before adapting. |
| Muse | Integration and installation unverified; consult current agent documentation before adapting. |
| Other Agent Skills consumers | Standard-format content is intended to be portable; discovery, permissions, and tools require consumer-specific verification. |

## Keep copies synchronized

Treat this repository as canonical. For each installation, keep a local record of source repository URL, skill path, installed commit (`git rev-parse HEAD` in this clone), destination, and any local modifications. Keep machine-specific records outside this public repository.

After updating the canonical clone, compare the full installed folder with the new source, review changed instructions and dependencies, and preserve local modifications before deliberately replacing a copy. Do not automatically overwrite it. Recheck discovery and a relevant task after updates. With a symlink, upstream edits become active immediately: review changes before updating the linked clone, and check for broken links after moving it. Avoid duplicate installations of the same skill.

Copy only completed skill directories, including their supporting files and required notices. Do not copy `templates/skill-template/`, repository `AGENTS.md`, or private local configuration into skill locations.

Automated installation, synchronization, plugins, and Hermes / Muse adapters are future enhancements outside this foundation.

## Imported writing skills

Copy each entire skill folder so its provenance, licenses, notices, and invocation policy travel with it.

- **Unslop:** explicitly request `$unslop` in Codex or `/unslop` in Claude Code with the text to edit. The Codex policy preserves upstream explicit invocation. On other hosts, the body instruction requests explicit use, but harness-level enforcement has not been verified.
- **Caveman:** request `$caveman` in Codex or `/caveman` in Claude Code. It applies a terse voice to chat replies until `stop caveman` or `normal mode`. The standalone import excludes hooks, proxies, extra modes, and companion commands; status uses the upstream fallback when the host does not track modes. No token-savings percentage is promised.

Unslop edits prose into full, readable sentences; Caveman compresses chat replies and keeps persisted documents in plain prose. Apply the style requested for the current output rather than mechanically combining opposing editing rules. These are usage instructions, not live runtime certification; trials remain deferred.
