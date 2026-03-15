---
description: 'Python coding conventions and guidelines'
paths:
  - '**/*.py'
---

# Python Development

> Target Python 3.12+ and prefer modern language features. Adjust if the project requires an older runtime.

## Core Principles

- Prioritize readability and clarity over cleverness.
- Write idiomatic, concise Python that follows community conventions.
- Use type hints on all function signatures and module-level variables.
- Break complex functions into smaller, focused functions with descriptive names.
- Handle edge cases explicitly and provide clear exception handling.

## Type Annotations

- Use built-in generic syntax: `list[str]`, `dict[str, int]`, `tuple[int, ...]`, `set[float]`.
- Use union syntax with `|`: `str | None` instead of `Optional[str]`.
- Import from `typing` only for constructs without built-in equivalents (`TypeVar`, `Protocol`, `TypeAlias`, `Literal`, `TypeGuard`).
- Use `Self` (from `typing`) for methods that return their own type.
- Add `-> None` return annotations to functions that return nothing.

## Code Style and Formatting

- Follow **PEP 8** for style conventions.
- Use 4 spaces for indentation.
- Use **ruff** for linting and formatting (replaces flake8, black, and isort).
- Configure in `pyproject.toml` under `[tool.ruff]`.
- Keep line length to 88 characters (ruff default) unless the project specifies otherwise.
- Use blank lines to separate functions, classes, and logical sections.
- Order imports: standard library, third-party, local — let ruff handle import sorting.

## Static Type Checking

- Use **mypy** or **pyright** for static analysis.
- Enable strict mode where practical (`strict = true` in `pyproject.toml`).
- Avoid `Any` unless interfacing with untyped libraries; prefer `object` or `Unknown` with narrowing.

## Documentation

- Provide docstrings following **PEP 257** on all public modules, classes, and functions.
- Use Google-style or NumPy-style docstrings consistently within a project.
- Place module-level docstrings at the top of the file, after imports.
- Keep docstrings focused on what the function does and any non-obvious behavior.

```python
def calculate_area(radius: float) -> float:
    """Calculate the area of a circle.

    Args:
        radius: The radius of the circle. Must be non-negative.

    Returns:
        The area calculated as pi * radius^2.

    Raises:
        ValueError: If radius is negative.
    """
    if radius < 0:
        raise ValueError(f"Radius must be non-negative, got {radius}")
    return math.pi * radius**2
```

## Project Structure

- Use `pyproject.toml` as the single source for project metadata, dependencies, and tool configuration.
- Manage virtual environments with `venv` or `uv`.
- Prefer `src/` layout for distributable packages; flat layout is acceptable for applications and scripts.

```
project/
├── pyproject.toml
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── core.py
│       └── utils.py
└── tests/
    ├── conftest.py
    ├── test_core.py
    └── test_utils.py
```

## Async Programming

- Use `async`/`await` with `asyncio` for I/O-bound concurrency.
- Prefer `asyncio.TaskGroup` (Python 3.11+) over `asyncio.gather` for structured concurrency.
- Use `async for` with async iterators and `async with` for async context managers.
- Avoid mixing blocking calls with async code; use `asyncio.to_thread` for CPU-bound or legacy sync operations.

## Error Handling

- Catch specific exceptions; avoid bare `except:` or `except Exception:` unless re-raising.
- Fail fast: validate inputs at function entry points.
- Use custom exception classes for domain-specific errors.
- Include context in error messages (what failed and with what input).
- Use `raise ... from e` to preserve exception chains.

## Testing

- Use **pytest** as the default test framework.
- Name test files `test_*.py` and test functions `test_*`.
- Use fixtures (`conftest.py`) for shared setup and teardown.
- Use `pytest.mark.parametrize` for data-driven tests.
- Aim for high coverage on critical paths; use `pytest-cov` to measure.
- Mock external dependencies with `unittest.mock` or `pytest-mock`.
- Test edge cases: empty inputs, boundary values, invalid types, large datasets.

## Security

- Never use `eval()`, `exec()`, or `compile()` on untrusted input.
- Validate and sanitize all external input before processing.
- Use parameterized queries for database access; never interpolate user input into SQL strings.
- Avoid `pickle` for untrusted data; prefer JSON or other safe serialization formats.
- Use `secrets` module for cryptographic randomness, not `random`.
- Pin dependency versions and audit with `pip-audit` or `safety`.

## Performance

- Profile before optimizing; use `cProfile` or `py-spy`.
- Prefer generators and iterators for large sequences.
- Use list/dict/set comprehensions over manual loops where readable.
- Leverage `functools.lru_cache` or `functools.cache` for expensive pure function calls.
- Use `dataclasses` or `attrs` for structured data instead of plain dicts.
- Consider `__slots__` for classes with many instances.

## Naming Conventions

- `snake_case` for functions, methods, variables, and module names.
- `PascalCase` for classes and type aliases.
- `UPPER_SNAKE_CASE` for module-level constants.
- Prefix private members with a single underscore (`_internal_method`).
- Avoid abbreviations unless universally understood (`url`, `id`, `db`).

<!-- Adapted from: https://github.com/github/awesome-copilot/blob/main/instructions/python.instructions.md -->
