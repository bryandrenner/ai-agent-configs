# AI Agent Configs

A collection of configurations to standardize AI model behavior across my local workspace. By maintaining these as version-controlled assets, I can sync the same instructions and standards across tools like GitHub Copilot and Claude Code to keep my development workflow consistent.

## Installing general GitHub Copilot configuration

```
ln -s ~/___PATH_TO___/ai-agent-configs/general/copilot/* ~/.copilot
```

## Installing general Claude Code configuration

Be careful not to overwrite existing files in the `~/.claude` folder. For example, Claude creates a `~/.claude/skills` folder with files in it, so you can't symlink `skills` wholesale.

With that in mind, for each file/folder in `./general/claude` create a symlink in `~/.claude`.

Example:

```
ln -s ~/___PATH_TO__/ai-agent-configs/general/claude/CLAUDE.md ~/.claude/CLAUDE.md
ln -s ~/___PATH_TO__/ai-agent-configs/general/claude/rules ~/.claude/rules
ln -s ~/___PATH_TO__/ai-agent-configs/general/claude/skills/retro ~/.claude/skills/retro
```
