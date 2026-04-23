---
technologies: [python]
---

# Python Coding Rules

## Style and formatting
- Use ruff for formatting and linting. Run `ruff check . --fix` before committing.
- Prefer `pathlib.Path` over `os.path` for file operations.
- Use f-strings for string formatting.

## Types
- Type-hint all public function signatures.
- Use `from __future__ import annotations` for forward references.
- Use `pydantic` for data validation at system boundaries (API inputs, DynamoDB reads).

## Structure
- One module per clear responsibility. If a file exceeds ~300 lines it probably does too much.
- Prefer standalone functions over classes unless shared state genuinely warrants a class.
- Keep `__init__.py` files empty or minimal.

## Testing
- Use `pytest` with standalone test functions, not test classes.
- Tests live in `tests/` mirroring `src/` structure.
- Use `tmp_path` (pytest fixture) for filesystem tests — never hardcode `/tmp`.
- Mock at system boundaries only (external APIs, AWS services). Don't mock internal code.

## Dependencies
- Manage dependencies with `uv`. Add to `pyproject.toml`, not `requirements.txt`.
- Pin transitive deps in `uv.lock`. Commit the lock file.
