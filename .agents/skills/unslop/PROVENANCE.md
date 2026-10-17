# Provenance: unslop

- Author: Lauren Tan (poteto).
- Source repository: https://github.com/cursor/plugins
- Source file: https://github.com/cursor/plugins/blob/101b274dd991f694e1f7024be2157e5f3adf07de/pstack/skills/unslop/SKILL.md
- Upstream revision: `101b274dd991f694e1f7024be2157e5f3adf07de`.
- Retrieved: 2026-10-17.
- Original SKILL.md SHA-256: `ccab208e315ce05e2fab8eeee2f7879f1289b5c6844840b2d307adf31c79144f`.
- License: MIT. The repository root MIT license does not replace these upstream terms.

## Local changes

Expanded the activation description; added license, category, source, author, and pinned revision metadata. Moved the upstream `disable-model-invocation: true` extension out of standard frontmatter into Codex `agents/openai.yaml` (`allow_implicit_invocation: false`) and an explicit-invocation instruction in the body. The editing rules and stable rule numbers are unchanged. Other consumers must honor the explicit-use instruction; harness enforcement is not claimed.

Only the standalone skill and necessary licensing/integration files are included. No upstream installer, executable code, hooks, proxy, or runtime is installed or run. No live agent trial or token-savings measurement was performed.

## Licensing evidence

The pstack [license at the pinned revision](https://github.com/cursor/plugins/blob/101b274dd991f694e1f7024be2157e5f3adf07de/pstack/LICENSE) grants MIT rights and credits Lauren Tan. Its full text is preserved verbatim in [LICENSE](LICENSE).
