# Contributing

## Workstation prerequisites

This workstation supports development using macOS, Visual Studio Code, and Claude Code. The DX has not been tested for other OSes, editors, and LLM agents.

It is required that you install [Mise](https://mise.jdx.dev/).

## Common tasks

### Restore dependencies

Run this after cloning, nuking, or switching to a branch or commit with different dependencies:

```sh
mise install
```

## Uncommon tasks

### Format files

This is uncommon, because it happens automatically on commit, and your editor can be configured to format on save.

```sh
mise run format
```
