---
description: 'GitHub Actions CI/CD workflow standards and best practices'
applyTo: '**/.github/workflows/*.yml, **/.github/workflows/*.yaml'
---

# GitHub Actions

> Guidelines for writing secure, maintainable, and efficient GitHub Actions workflows.

## Workflow Structure

- Place workflow files in `.github/workflows/` with descriptive kebab-case names (e.g., `ci-build.yml`, `deploy-production.yml`).
- Start every workflow with a clear `name:` that describes its purpose.
- Keep workflows focused on a single concern; split unrelated CI and CD logic into separate files.
- Use comments to explain non-obvious trigger conditions or job logic.

```yaml
name: CI Build and Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
      - name: Build
        run: npm ci && npm run build
```

## Triggers

- Use `push` and `pull_request` for standard CI; scope to specific branches and paths to avoid unnecessary runs.
- Use `workflow_dispatch` for manual triggers; define `inputs` with descriptions and defaults.
- Use `workflow_call` for reusable workflows; define `inputs` and `secrets` explicitly.
- Use `schedule` with cron syntax for periodic tasks; prefer off-peak hours.
- Use path filters (`paths:`, `paths-ignore:`) to skip workflows unaffected by the change.

## Security

### Action Pinning

- **Pin all third-party actions to a full SHA**, not a tag or branch. Tags are mutable and can be overwritten by a compromised upstream.
- Add a comment with the tag name for readability.

```yaml
# Good: pinned to SHA with tag comment
- uses: actions/setup-node@1d0ff469b7ec7b3cb9d8673fde0c81c44821de2a # v4.2.0
  with:
    node-version: 22

# Bad: mutable tag reference
- uses: actions/setup-node@v4
```

### Token Permissions

- Set `permissions:` at the workflow level with the minimum required scopes.
- Default to `contents: read` and add write permissions only on jobs that need them.
- Never use `permissions: write-all`.

```yaml
permissions:
  contents: read
  pull-requests: write  # only if the workflow comments on PRs
```

### Secrets

- Store all sensitive values (API keys, tokens, passwords) in GitHub Secrets or environment-level secrets.
- Never hardcode secrets in workflow files, step scripts, or repository code.
- Use `${{ secrets.SECRET_NAME }}` to reference secrets; they are masked in logs automatically.
- Prefer environment-level secrets for deployment credentials to restrict access via environment protection rules.
- Avoid passing secrets as environment variables to entire jobs; scope them to the specific step that needs them.

### Untrusted Input

- Never interpolate `${{ github.event.* }}` expressions directly into `run:` scripts; this enables script injection.
- Pass event data through environment variables instead.

```yaml
# Bad: injectable
- run: echo "Title is ${{ github.event.pull_request.title }}"

# Good: safe via environment variable
- env:
    PR_TITLE: ${{ github.event.pull_request.title }}
  run: echo "Title is $PR_TITLE"
```

## Caching

- Use `actions/cache` (or built-in caching in `actions/setup-node`, `actions/setup-python`, etc.) to cache dependency install artifacts.
- Choose cache keys that include the lockfile hash to invalidate on dependency changes.
- Use `restore-keys` for fallback to partial cache hits.

```yaml
- uses: actions/cache@5a3ec84eff668545956fd18022155c47e93e2684 # v4.2.3
  with:
    path: ~/.npm
    key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
    restore-keys: npm-${{ runner.os }}-
```

## Matrix Builds

- Use `strategy.matrix` to test across multiple OS, language, or framework versions.
- Use `fail-fast: false` when you want all matrix combinations to complete even if one fails.
- Use `include` / `exclude` to add or skip specific combinations.

```yaml
strategy:
  fail-fast: false
  matrix:
    os: [ubuntu-latest, windows-latest]
    node-version: [20, 22]
```

## Job Dependencies and Concurrency

- Use `needs:` to define job execution order; keep the dependency graph as shallow as possible.
- Use `concurrency:` to cancel redundant runs on the same branch or PR.

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

## Reusable Workflows

- Extract shared CI logic into reusable workflows (triggered by `workflow_call`).
- Define all inputs, secrets, and outputs explicitly in the reusable workflow.
- Store reusable workflows in a central repository when shared across multiple repos.
- Call reusable workflows with `uses: org/repo/.github/workflows/reusable.yml@sha`.

## Artifacts

- Use `actions/upload-artifact` and `actions/download-artifact` (v4) to pass build outputs between jobs.
- Set `retention-days` to limit storage costs; default to the shortest practical retention.
- Name artifacts descriptively to avoid collisions in matrix builds.

## Environments and Protection Rules

- Use GitHub environments (`environment:` key) for deployment targets (staging, production).
- Configure required reviewers, wait timers, and branch restrictions on production environments.
- Bind deployment secrets to environments rather than the repository level.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy
        env:
          API_KEY: ${{ secrets.DEPLOY_API_KEY }}
        run: ./deploy.sh
```

## Common Patterns

### CI: Build and Test

```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
      - uses: actions/setup-node@1d0ff469b7ec7b3cb9d8673fde0c81c44821de2a # v4.2.0
        with:
          node-version-file: .nvmrc
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build
```

### CD: Deploy on Release

```yaml
on:
  release:
    types: [published]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    permissions:
      contents: read
      id-token: write
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
      - name: Deploy
        run: ./scripts/deploy.sh
```

### PR Status Checks

```yaml
on:
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      checks: write
    steps:
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
      - run: npm ci && npm test
```

## Self-Hosted Runners

- Use self-hosted runners only when required (private network access, specialized hardware, cost optimization).
- Apply labels to distinguish runner capabilities (e.g., `self-hosted`, `gpu`, `arm64`).
- Keep runner images updated and hardened; treat them as ephemeral where possible.
- Never run untrusted workflows (e.g., from forks) on self-hosted runners without additional protections.
