# General instructions

In this document, the order of ordered lists, and the nesting of lists, are meaningful.

## High-level principles

**All principles and practices are subject to varying applicability and reasonable exceptions.**

In descending order of importance, the implementation should:

1. Maximize value for all downstream consumers. Drive the implementation with these consumers in mind, when applicable, in this order:
  1. External users
    1. End users (non-technical)
    2. Power users
    3. Programmatic consumers (e.g. external developers)
  2. Internal users
    1. Non-technical users (e.g. support, customer success)
    2. Ops and administrators
    3. Programmatic consumers (e.g. internal developers of downstream products)
    4. QA
    5. Contributors (i.e. owning developers of new features, bug fixes, and maintenance)
1. In this order, conform to the conventions of:
  1. the nearby code
  2. the codebase as a whole
  3. the team
  4. the company
  5. contemporary and emerging standards and practices, especially of the platforms, languages, and frameworks being used
  6. my personal implementation principles and practices (see below)

## My personal implementation principles and practices

- Names should use the ubiquitous language of the business / domain.
- Types should model and communicate the domain.
- Models and functions should enforce the invariants of the domain.
  - Required vs. nullable properties
  - The range of valid values (acceptable numeric range, integer vs. floating point, string length, empty/whitespace strings, date-time specificity, etc.)
  - Relation with other entities in aggregates (e.g. uniqueness, ownership, overlapping values)
- Functions and logic should be defensive and fail fast (e.g. guard clauses).
- Favor compile-time (or type-check time) safety over runtime safety.
- Units (e.g. classes, functions) are small, cohesive, and focused on a single responsibility.
- Units operate at one level of abstraction.
- Units are loosely coupled and testable.
- Services are stateless.
- Encapsulate communication with external systems (including the host filesystem, clock) behind mockable interfaces.
- Favor explicitness over ambiguity.
- Include values even if they are defaults, but use comments to indicate "default".
- Favor strictness over looseness.
- Favor thin presentation layers, and rich application and domain layers.
- Apply the "Rule of Three" to avoid overabstraction.
- Favor focused interfaces over god objects.
- When exporting an interface, ensure that all interfaces depended-upon by public contract are public (not internal).
- In code files, order sections like this:
  1. imports
  2. constants
  3. exported interfaces
  4. main entrypoint(s) and exports
  5. private / internal / supporting types and units
- Write comments to:
  1. Explain the rationale of ambiguous (or controversial) design decisions.
  2. Clarify related implementation context that is non-obvious.
  3. Give usage examples where variations are complex.
  4. Summarize the "what" of
    - confusing constraints from external services or libraries
    - potentially intimidating logic (sometimes necessary)
- Provide multiple types of automated tests to cover behaviors comprehensively. As test suites are available, apply the Test Pyramid principle of covering more granular behaviors with quicker tests at the top, and coarser behaviors with heavier tests at the bottom:
  1. **Unit tests** target individual classes or functions.
  2. **Component tests** target interconnected classes and functions with mocked boundaries or in-memory databases
  3. **Integration tests** target the implementation with real connections to external systems
  4. **End-to-end tests** target a running instance in a real deployment environment, or a published library
- Tests should cover, and communicate, the behaviors of their target - not internals.
- Log proactively with appropriate severity:
  1. **fatal** - failures from which the application cannot recover, or data may have been corrupted. Catch and log at a high level, and near crucial or suspicious boundaries.
  2. **error** - a condition that should not occur and that likely requires a fix, but from which the system can recover and continue operating (a likely bug)
  3. **warn** - an anomaly that is expected in the regular course of events, for which an upward trend may indicate an actionable problem (examples: a temporary network failure, a bad request from a user, an unexpectely long running process, a deprecation notice encountered). Catch and log at the site where anomalies are expected (e.g. boundaries with external systems or the filesystem, validation of external or incoming data).
  4. **info** - happy-path milestones that may be worth monitoring, or collecting metrics from, in the regular course of events; for which a downward trend may indicate an actionable problem. Catch and log at the site of the significant event.
  5. **debug** - diagnostic details for active troubleshooting, *selectively enabled*. Log sparingly and selectively where details are likely to help a future troubleshooter.
  6. **trace** - fine-grained diagnostic details that should be collected only when troubleshooting, only in local development. Add trace-level logging statements *only during a troubleshooting effort, and remove them before merging*.
- Enforce strict conventions via automated checks (e.g. strict compiler, typecheck, linting, style-cop configurations).
- Perform safe and superficial formatting automatically on commit or in CI.
- Proactively maintain documentation.
  - Document the product and usage details in `README.md`.
  - Document the implementation for engineering contributors in `CONTRIBUTING.md`.
  - For closed-source repositories, it's acceptable to put all documentation in `README.md`.
  - Extended contributor topics should be placed in separate docs in the `docs/`.
  - Keep documentation specific to the repository, and reference external documentation for broader topics.
- Development scripts and editor configuration (e.g. `launch.json`, `tasks.json`) should support:
  - debugging each service
  - debugging each test suite
  - debugging tests relative to the cursor or open file
  - running any of the above without debugging

## Planning practices

### Concept: Pristine changeset

A **pristine changeset** (or commit) is a changeset that is:
- **atomic** - focused on one goal
- **incremental** - minimal diff and blast radius
- **verified** - zero regressions; passes lint, type check, test, and build targets
- **release-safe** - verified, and all WIP or prototype features dormant

A **pristine changeset**:
- enables faster feedback from code review, QA, and PO signoff
- may represent either a finished feature *or* WIP / prototype (toggleable behind a feature flag)

### Guidelines

- Aim for pristine changesets.
- When a chat / session arrives at a pristine changeset, recommend a commit message, PR title, and PR description.
- Develop Outside-In to avoid gold-plating the backend. User stories drive UI/API implementation, which drive backend implementation.
- Use environment-based feature flags to enable WIP code paths and short-lived branches.
- When introducing non-trivial changes to existing implementation, identify opportunities for **meaningful, high-leverage** refactors and test coverage improvements as a preliminary, pristine changeset. **Ask about these before finalizing a plan.**
- Avoid YAGNI and premature optimization, but acknowledge opportunities for future improvements that are currently out-of-scope.
