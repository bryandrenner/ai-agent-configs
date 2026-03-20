---
description: 'Guidelines for individual-project specs to be read automatically by AI agents'
paths:
  - '*/**/AGENTS.md'
  - '*/**/CLAUDE.md'
  - '*/**/CONTRIBUTING.md'
---

Follow this template to create and maintain docs **specific to a project in a monorepo**. It may also be used for root-level docs for single-project (non-monorepo) repos. 

---

# {Project Name} — Product and implementation spec

> **Purpose:** Single-source-of-truth for AI-assisted and human contributors to implement this project from scratch or resume work.
> **Last updated:** {YYYY-MM-DD}

---

## Stack

```yaml
runtime: # e.g. Node 22, .NET 8
languages: # e.g. TypeScript 5.x, C# 12
frameworks:
  frontend: # e.g. React 19, Astro 5
  backend: # e.g. ASP.NET Core 8, Express
  cms: # e.g. Umbraco 14 (if applicable)
styling: # e.g. Tailwind 4, CSS Modules
state: # e.g. Zustand, Redux Toolkit, React Query
auth: # e.g. OpenIddict, @axa-fr/react-oidc
testing:
  unit: # e.g. Vitest, xUnit + NSubstitute
  component: # e.g. Storybook + test-runner
  integration: # e.g. xUnit + WebApplicationFactory
  e2e: # e.g. Playwright
linting: # e.g. ESLint flat config, StyleCop.Analyzers
logging: # e.g. pino + pino-browser, Serilog
monorepo: # e.g. Nx 20 with Project Crystal
infrastructure:
  cloud: # e.g. AWS (us-east-1 primary, us-west-2 DR)
  iac: # e.g. AWS CDK v2 (TypeScript)
  compute: # e.g. ECS Fargate
  database: # e.g. RDS PostgreSQL 16
  ci: # e.g. GitHub Actions
  registry: # e.g. ECR, npm (private)
```

---

## Deployables

```yaml
apps:
  - name: # e.g. Dashboard
    type: # spa | ssr | api | worker
    path: # e.g. apps/dashboard
    port: # local dev port
    features:
      - # major feature 1
      - # major feature 2

  - name: # e.g. Public API
    type: api
    path: # e.g. apps/api
    port:
    features:
      -

libraries:
  - name: # e.g. @org/contracts
    path: # e.g. libs/contracts
    purpose: # shared DTOs/types between frontend and backend
  - name:
    path:
    purpose:
```

---

## Upstream Dependencies

```yaml
dependencies:
  - name: # e.g. PostgreSQL
    purpose: # primary datastore
    docs: # URL
    local: # e.g. Docker compose service, localhost:5432
    env_var: # e.g. DATABASE_URL

  - name: # e.g. Redis
    purpose:
    docs:
    local:
    env_var:

  - name: # e.g. Stripe API
    purpose:
    docs:
    env_var: # e.g. STRIPE_SECRET_KEY
```

---

## Instances

```yaml
environments:
  - name: local
    notes: # e.g. Docker Compose for infra, Nx serve for apps

  - name: test
    base_uris:
      dashboard: # URL
      api: # URL
    deploy_trigger: # e.g. push to main
    observability:
      logs: # e.g. CloudWatch log group ARN/path
      metrics: # e.g. CloudWatch dashboard URL
      traces: # e.g. X-Ray group
    upstream:
      # | Dependency | Environment | URI |
      # | ---------- | ----------- | --- |

  - name: staging
    base_uris:
    deploy_trigger:
    observability:
    upstream:

  - name: production
    base_uris:
    deploy_trigger: # e.g. manual promotion from staging, GitHub release
    observability:
    upstream:
```

---

## Quickstart

### Developer Machine Setup

> One-time prerequisites. Idempotent where possible.

1. {Runtime/SDK install instructions}
2. {Database/Docker install}
3. {CLI tools — e.g. AWS CLI, Nx CLI}
4. {Editor extensions — e.g. ESLint, Prettier, C# Dev Kit}
5. {Auth/VPN/SSL setup if applicable}

### After Cloning (or nuking)

```bash
# 1. Install dependencies
# 2. Copy secrets
# 3. Generate local certs / register them
# 4. Seed database
```

### After Switching Branches

```bash
# 1. Restore dependencies
# 2. Run pending migrations
# 3. Rebuild affected projects (if applicable)
```

### Common Commands

```yaml
quick_checks: # e.g. nx run-many -t lint,typecheck,test
slow_tests: # e.g. nx run-many -t e2e
dev_server: # e.g. nx serve dashboard & nx serve api
build_prod: # e.g. nx run-many -t build --configuration=production
clean: # e.g. nx reset && git clean -fdx -e .env
db_migrate: # e.g. dotnet ef database update
db_seed: # e.g. dotnet run --project tools/seed
db_reset: # e.g. dotnet ef database drop && migrate && seed
debug_service: # e.g. attach to port 5001 via launch.json
debug_test: # e.g. nx test api --test-name-pattern="..." --inspect
clear_cache: # e.g. nx reset
```

---

## Versioning Strategy

<!-- e.g. SemVer for libraries, CalVer for apps, conventional commits, changesets, etc. -->

---

## Codebase Tour

### Directory Organization

```
/
├── apps/
│   ├── {app-name}/        # {type} — {one-line purpose}
│   └── ...
├── libs/
│   ├── {lib-name}/        # {one-line purpose}
│   └── ...
├── tools/                  # scripts, seed data, codegen
├── infra/                  # CDK stacks
├── docs/                   # extended contributor docs
├── .github/                # CI workflows
├── nx.json
└── {root config files}
```

### Architectural Patterns

<!-- Keep this terse. Name the pattern and where it applies. -->

- **{Pattern}** — {where/why}. Example: "Clean Architecture — API project. Domain → Application → Infrastructure → Presentation."
- **{Pattern}** — {where/why}. Example: "Feature-sliced directory structure — Dashboard app. Each feature folder contains components, hooks, api, and tests."
- **{Pattern}** — {where/why}. Example: "CQRS via MediatR — API command/query handlers."

---

## Implementation Recipes

### Add a New Business Entity

1. Domain model in `libs/domain/src/{Entity}.ts` (or `.cs`)
2. Repository interface + implementation
3. Business/validation rules
4. Cascade/deletion behavior on parent
5. Database migration
6. Controller/route actions (CRUD)
7. DTO + validator (e.g. Zod schema or FluentValidation)
8. E2E test of a CRUD workflow
9. OpenAPI/Swagger doc update (or other end-user docs)

### Add a New Library

1. `nx g @nx/{plugin}:library {name} --directory=libs/{name}`
2. Configure `tsconfig`, `project.json` / inferred targets
3. Add path alias in root `tsconfig.base.json`
4. Export public API from `index.ts`
5. Add to consuming app's dependencies

### Add a New App

1. `nx g @nx/{plugin}:application {name} --directory=apps/{name}`
2. Wire up dev/build/test targets
3. Add Dockerfile + CDK stack for deployment
4. Add CI workflow triggers
5. Add to Instances table above

<!-- Add project-specific recipes as needed -->

---

## Gotchas

<!-- Tribal knowledge, easy mistakes, common confusion. Bullet per gotcha. -->

- {Gotcha 1 — e.g. "Nx caches aggressively. If something seems stale, run `nx reset`."}
- {Gotcha 2 — e.g. "The CMS backoffice runs on a separate port and requires a proxy entry in vite.config."}
- {Gotcha 3}

---

## Troubleshooting

<!-- Problem → fix pairs. Keep concise. -->

| Symptom | Cause | Fix |
| ------- | ----- | --- |
| {error message or behavior} | {why} | {command or action} |
| | | |

---

## AI Implementation Notes

> Instructions for Claude Code or other AI agents consuming this spec.

### Phased Build Plan

```yaml
phases:
  - id: 1
    name: # e.g. Scaffold & Infra
    goal: # one sentence
    done_when:
      - # verifiable criterion
      - # verifiable criterion
    key_decisions:
      - # e.g. "Use inferred Nx targets — no explicit project.json unless overriding."

  - id: 2
    name:
    goal:
    depends_on: [1]
    done_when:
      -
    key_decisions:
      -

  # Continue phases...
```

### Constraints & Preferences

<!-- Non-negotiable rules for the AI to follow during implementation. -->

- {Constraint — e.g. "Never use Moq. Use NSubstitute."}
- {Constraint — e.g. "All TypeScript must pass `verbatimModuleSyntax`. No `import type` via value syntax."}
- {Constraint — e.g. "ESLint flat config only. Separate dev overlay from CI config."}
- {Preference — e.g. "Prefer Vitest over Jest."}
- {Preference — e.g. "Prefer explicit return types on exported functions."}

### Context Boundaries

<!-- Help the AI understand what's in-scope vs. out-of-scope for a given session. -->

```yaml
in_scope:
  - # e.g. apps/dashboard, libs/contracts, infra/
out_of_scope:
  - # e.g. legacy migration scripts, third-party fork
reference_only:
  - # e.g. Read libs/domain for types but don't modify
```
