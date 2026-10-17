# Provenance: requirements-clarity

- Upstream project: softaworks/agent-toolkit.
- Copyright holder in upstream license: Leonardo Flores, 2026.
- Original source: https://github.com/softaworks/agent-toolkit/tree/3027f20f3181758385a1bb8c022d4041dfb4de84/skills/requirements-clarity
- Upstream revision: `3027f20f3181758385a1bb8c022d4041dfb4de84`.
- Retrieved: 2026-10-17.
- Original SKILL.md SHA-256 (UTF-8): `c554f7779a6e8ba4f89c8f7b79738d2825aa39c64d2b5dcc36006534ef564924`.
- License: MIT; [LICENSE](LICENSE) is copied verbatim from the pinned repository root. No separate license or NOTICE was present in either reviewed skill directory; the root grant covers these files. The local repository's root license does not replace the upstream notice.

## Adaptation

Retained focused questioning, Why/Simpler lenses, scope boundaries, acceptance criteria, and implementation handoff. Replaced mandatory 100-point scoring, repeated approval rounds, a fixed PRD path, and a host-specific Write tool with consequence-based clarification and proportional specifications. Removed exclusions based solely on code snippets or file paths. Added evidence reuse, labeled reversible defaults, blocked-draft handling, and routing back to already-authorized implementation.

Started from this repository's skill template and added standard license/category/source metadata. The adapted instructions are substantially rewritten; they are not a verbatim upstream installation.

## Dependencies and review limits

Only SKILL.md, this provenance note, and the upstream license are distributed. No scripts, hooks, executables, network clients, or upstream runtime dependencies are bundled or required. Available text/file tools are sufficient; Git inspection is optional for handoffs in Git projects.

The upstream handoff helpers were inspected as text, not executed: they use Python's standard library, local Markdown reads/writes, and Git subprocesses. The scaffold collects project paths and commit/file metadata; the staleness helper trusts a recorded project path, and heuristic scoring/secret patterns do not establish freshness or security. These helpers, their evaluation setup/results, and host-specific references are excluded. The requirements skill has no supporting scripts in its upstream directory.

Validation comprises YAML/link/discovery checks, content review, and synthetic offline workflow walkthroughs. No live Codex session-transfer, automatic-trigger, or cross-agent runtime certification is claimed. No installer or integration changes are included.

Offline walkthroughs covered clarifying export format and row scope, deriving checkable CSV/empty-result criteria after supplied answers, producing a blocked draft without questions, and avoiding activation for a reproducible bug with established expected behavior. File-specific requests can still require clarification when product behavior is ambiguous.
