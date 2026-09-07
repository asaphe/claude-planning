# claude-planning

Claude Code plugin: phase-gated planning/RFC workflow.

## Conventions

- This repo IS the plugin — `.claude-plugin/plugin.json` lives at the root,
  alongside a self-referencing `.claude-plugin/marketplace.json` so it can be
  installed directly with `/plugin marketplace add`.
- No personal-machine paths (`/Users/`, `~/`, `.dotfiles`), personal Google
  Drive references, or hardcoded company-internal identifiers (account IDs,
  tenant IDs, internal service names) — this is meant to be installed by
  anyone, on any machine.
- Run `claude plugin validate .` locally before pushing — CI runs the same
  check, but locally is where a manifest typo costs nothing to fix.
