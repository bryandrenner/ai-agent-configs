---
description: 'Guidelines for contributor documentation'
paths:
  - '**/README.md'
  - '**/CONTRIBUTING.md'
  - '**/docs/**'
---

# Guidelines for contributor documentation

Contributor documentation belongs in `CONTRIBUTING.md`. In closed-source repositories, it's acceptable to put it in `README.md`. Extended topics should be placed in separate docs in the `docs/`.

Favor Markdown for contributor documentation.


## Content and structure

Use heading `#` for the title, and different levels of headings (`##`, `###`, etc.) to organize content into sections and subsections.

Documentation for engineering contributors should include:

- **Quickstart**
  - **Getting set up**
    - **Developer machine setup** - prerequisite, on-time setup of the development workstation (e.g. required and recommended installation and configuration of runtimes, databases, CLIs, SDKs, Docker, VPN clients, editors, extensions, etc.)
    - **After cloning or nuking** - steps to set up the repository after cloning or `git clean -fdx` (e.g. copying secrets into `.env`, registering local SSL certificates, pointing local tools to the path)
    - **After switching branches / commits** - steps to take after `git switch`, `git checkout`, `git rebase`, `git pull` (e.g. restoring dependencies, running pending DB migrations, reseeding persisted data, new local-only config in `.env`)
  - **Common commands**
    - to run all quick checks and tests (e.g. linting, type-checking, unit tests, component tests, building for dev/debugging)
    - to run slow tests (e.g. integration tests, E2E tests)
    - to start the development server(s) and watch for changes
    - to create a production artifact / deployment package
    - to clear a cache
    - to reset / migrate / seed a database
    - to debug a service
    - to debug a test
    - to clean (delete build artifacts)
- **Deployables**
  - the deployable product(s) (apps and services), and backend services, implemented in this repository (e.g. Dashboard, Admin Console, Public API)
    - the major features of each deployable
  - the published libraries implemented in this repository (e.g. a shared contracts library, an API client, a utility package)
- **Upstream dependencies** - upstream service dependencies (e.g. databases and 3rd party APIs)
  - name
  - short description or purpose
  - link(s) to documentation
- **Instances**
  - For each app/service, the supported environments (e.g. test, staging, production)
  - For each environment:
    - canonical base URI(s)
    - how to trigger deploy to that environment
    - observational locations (e.g. logs, metrics, dashboards)
    - upstream dependencies (name, targeted environment, canonical URI(s))
      - Example:
        
        | Dependency | Environment | URI                         |
        | ---------- | ----------- | --------------------------- |
        | Foo API    | sandbox     | https://api.sandbox.foo.com |
        | Postgres   | local       | https://localhost:5432      |

- **Versioning strategy**
- **Codebase tour**
  - high-level directory organization
  - high-level architectural patterns
- **Gotchas** - easy mistakes to avoid, common points of confusion, tribal / institutional knowledge
- **Implementation recipes**
  - Example: Checklist to add a new business entity:
    - add a domain model
    - add a repository and/or property on parent entities
    - add business rules for related entities
    - add deletion behavior for when parent deleted
    - add a database migration
    - add controller actions for CRUD
    - add DTO validator(s)
    - add an E2E test of a CRUD workflow
    - add documentation to the OpenAPI spec (or other end-user docs)
- **Troubleshooting** - common errors and how to fix them
