# General instructions

**All guidelines are context-dependent and subject to reasonable exceptions.**

## High-level principles

In descending order of importance, planning and implementing:

1. Considers the product's possible consumers, in this order:
   1. External users
      1. End users (non-technical)
      2. Power users
      3. Programmatic consumers (e.g. external devs)
   2. Internal users
      1. Non-technical users (e.g. support, customer success)
      2. Ops and administrators
      3. Programmatic consumers (e.g. internal devs of downstream products)
      4. QA
      5. Contributors (i.e. owning devs)
2. Conforms to the conventions of:
   1. the nearby code
   2. the codebase as a whole
   3. the team
   4. contemporary practices, esp. of the platforms, languages, and frameworks
   5. my personal guidelines (see below)

## My personal guidelines

- Identifiers use the ubiquitous language of the business / domain.
- Types and logic enforce the invariants of the domain. For example:
  - Required vs. optional properties
  - Valid vs. out-of-range attributes
  - Correct vs. incorrect relationships with other entities
- Logic is defensive and fails fast (e.g. guard clauses).
- Favor compile time enforcement over checks at run time.
- Units (e.g. classes, functions) are small and focused on a single responsibility.
- Units operate at one level of abstraction.
- Units are loosely coupled and testable.
- Services are stateless.
- Encapsulate communication with external systems (including host system resources) behind mockable interfaces.
- Favor explicitness.
- Favor strictness.
- Favor thin presentation layers, rich service and domain layers.
- Apply the "Rule of Three" to avoid overabstraction.
- Favor focused interfaces over god objects.
- Implement entrypoints and exports near the start of the file, and private/helper members later in the file.
- Write comments to:
  1. Explain the rationale of ambiguous (or controversial) design decisions.
  2. Clarify related implementation context that is non-obvious.
  3. Give usage examples where variations are complex.
  4. Summarize the "what" of
    - confusing constraints from external services or libraries
    - potentially intimidating logic (sometimes necessary)
- Cover behaviors comprehensively with multiple types of automated tests. Apply the Test Pyramid:
  1. **Unit tests** target individual classes or functions.
  2. **Component tests** target interconnected classes and functions with mocked boundaries or in-memory databases
  3. **Integration tests** target the implementation with real connections to external systems
  4. **End-to-end tests** target a running instance in a real deployment environment
- Tests focus on the externally-appreciable behaviors of their target.
- Log proactively with appropriate severity:
  1. **fatal** - failures from which the app cannot recover, or when data may have been corrupted
  2. **error** - a condition that should not occur and that likely requires a fix, but from which the system *can* recover
  3. **warn** - an anomaly that is expected in the regular course of events, for which an upward trend may indicate an actionable problem (examples: a temporary network failure, a bad request from a user, an unexpectely long running process, a deprecation notice encountered)
  4. **info** - happy-path milestones that may be worth monitoring, or collecting metrics from, in the regular course of events; for which a downward trend may indicate an actionable problem
  5. **debug** - diagnostic details for active troubleshooting, *selectively enabled*. Log sparingly and selectively where details are likely to help a future troubleshooter.
  6. **trace** - fine-grained diagnostic details that should be collected only when troubleshooting, only in local development. Add trace-level logging statements *only during a troubleshooting effort, and remove them before merging*.
- Enforce strict conventions via automated checks (e.g. strict compiler, typecheck, linting, "style cop" configs).
- Perform safe formatting automatically on commit or in CI.
- Proactively maintain docs.
  - Explain the product and usage in `README.md`.
  - Explain dev workflows and implementation concepts for contributors in `CONTRIBUTING.md` or `README.md`.
  - Explain lengthier dev topics in `docs/`.
  - Constrain docs to repository-specific info, and liberally reference external docs for broader topics.
- Local dev config (e.g. `launch.json`, `tasks.json`, `package.json` scripts, etc.) supports debugging, and running-without-debugging:
  - each app or service
  - each test suite
  - specific tests by cursor location (or focused file)

## Planning practices

### Concept: Pristine changeset

A **pristine changeset** (or commit) is a changeset that is:
- **atomic** - focused on one goal
- **incremental** - minimal diff / blast radius
- **verified** - updates test coverage per the changes; builds; passes lint, type check, and tests
- **release-safe** - prerelease, WIP, or prototype features gated and dormant

A **pristine changeset**:
- enables faster feedback from code review, QA, and PO signoff
- may represent either a finished feature, WIP, *or* prototype

### Guidelines

- Aim for pristine changesets.
- When a chat / session results in a pristine changeset, recommend a commit message, PR title, and PR description.
- Implement Outside-In to avoid gold-plating the backend. User stories drive UI/API implementation, which drive backend implementation.
- Use environment-based feature flags to enable WIP code paths and short-lived branches.
- When making non-trivial changes to existing implementation, consider prerequisite **low-effort, high-impact** refactors and test coverage. **Ask about these before finalizing a plan.**
- Avoid premature optimization, but acknowledge opportunities for future improvements that are currently out-of-scope.
