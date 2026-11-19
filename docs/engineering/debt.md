# Debt

Deferred decisions, caveats, and improvement opportunities to revisit.

## Claude rules duplicate Copilot instructions

Most files in `claude/coding/rules/` are hand-adapted copies of files in `copilot/coding/instructions/`, drawn from the same awesome-copilot sources. Changes to one aren't reflected in the other. Consider single-sourcing them in `shared/` and generating each family's format from it.

## Marketplace manifest lives outside `claude/`

`.claude-plugin/marketplace.json` has to be at the repo root for the repo to work as a plugin marketplace, so it's the one Claude-specific file outside `claude/`. Revisit if marketplaces ever support a manifest in a subdirectory.
