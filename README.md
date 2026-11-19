# AI Agent Configs

A collection of configurations to standardize AI model behavior across my local workspace. By maintaining these as version-controlled assets, I can sync the same instructions and standards across tools like GitHub Copilot and Claude Code to keep my development workflow consistent.

## Layout

Config is grouped by product family, then by product:

```
claude/                # the Claude family: Claude.ai and Claude Code
  general-purpose/     # Claude.ai
  coding/              # Claude Code
    CLAUDE.md
    rules/
  shared/              # shared between Claude.ai and Claude Code
    plugins/
      drenner-toolkit/
copilot/               # the Copilot family
  general-purpose/
  coding/              # Copilot coding agents (CLI, IDE)
    instructions/
    skills/
  shared/
shared/                # shared between product families
```

`.claude-plugin/marketplace.json` is the one Claude-specific file outside `claude/`. It has to sit at the repo root so the repo can be added as a plugin marketplace.

## Installing GitHub Copilot configuration

```sh
ln -s ~/___PATH_TO___/ai-agent-configs/copilot/coding/* ~/.copilot
```

## Installing Claude Code configuration

Be careful not to overwrite existing files in the `~/.claude` folder. For example, Claude creates a `~/.claude/skills` folder with files in it, so you can't symlink `skills` wholesale.

With that in mind, for each file/folder in `./claude/coding` create a symlink in `~/.claude`:

```sh
ln -s ~/___PATH_TO___/ai-agent-configs/claude/coding/CLAUDE.md ~/.claude/CLAUDE.md
ln -s ~/___PATH_TO___/ai-agent-configs/claude/coding/rules ~/.claude/rules
```

If you're moving an existing install to a new path, use `ln -sfn` so the existing directory symlinks are replaced rather than nested.

## Installing the drenner-toolkit plugin

Add this repo as a plugin marketplace, then install `drenner-toolkit` from it. Either:

- in claude.ai, add `bryandrenner/ai-agent-configs` as a marketplace in plugin settings, which syncs to Claude Code; or
- in Claude Code, run `claude plugin marketplace add bryandrenner/ai-agent-configs`, then `claude plugin install drenner-toolkit@drenner-marketplace`.
