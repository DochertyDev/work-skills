# Provenance: session-handoff

- Upstream project: softaworks/agent-toolkit.
- Copyright holder in upstream license: Leonardo Flores, 2026.
- Original source: https://github.com/softaworks/agent-toolkit/tree/3027f20f3181758385a1bb8c022d4041dfb4de84/skills/session-handoff
- Upstream revision: `3027f20f3181758385a1bb8c022d4041dfb4de84`.
- Retrieved: 2026-10-17.
- Original SKILL.md SHA-256 (UTF-8): `8964e1b846170ac9cdbaa4c3c510e3493e08917620761cbb492c46d8d3567e64`.
- License: MIT; [LICENSE](LICENSE) is copied verbatim from the pinned repository root. No separate license or NOTICE was present in either reviewed skill directory; the root grant covers these files. The local repository's root license does not replace the upstream notice.

## Adaptation

Retained create/resume modes, decision rationale, progress evidence, ordered next steps, and predecessor links. Replaced mandatory Python scripts and .claude storage with manual evidence checks and a portable destination fallback. Removed proactive milestone/file-count triggers, fixed quality scores, and unconditional chain traversal. Added task-identity selection, stale-state reconciliation, protection for unrelated changes, historical-data trust boundaries, and private/chat-only output. No persistent-memory writes or automatic publication.

Started from this repository's skill template and added standard license/category/source metadata. The adapted instructions are substantially rewritten; they are not a verbatim upstream installation.

## Dependencies and review limits

Only SKILL.md, this provenance note, and the upstream license are distributed. No scripts, hooks, executables, network clients, or upstream runtime dependencies are bundled or required. Available text/file tools are sufficient; Git inspection is optional for handoffs in Git projects.

The upstream handoff helpers were inspected as text, not executed: they use Python's standard library, local Markdown reads/writes, and Git subprocesses. The scaffold collects project paths and commit/file metadata; the staleness helper trusts a recorded project path, and heuristic scoring/secret patterns do not establish freshness or security. These helpers, their evaluation setup/results, and host-specific references are excluded. The requirements skill has no supporting scripts in its upstream directory.

Validation comprises YAML/link/discovery checks, content review, and synthetic offline workflow walkthroughs. No live Codex session-transfer, automatic-trigger, or cross-agent runtime certification is claimed. No installer or integration changes are included.

Offline walkthroughs covered saving an export task with pending checks, resuming after a rename/revision change while preserving unrelated staged edits, distinguishing two different task handoffs, and rejecting an ordinary status update as a trigger. Recorded commands and deployment approvals were treated as historical data rather than execution authority.
