# CLAUDE.md

Integration layer connecting the Doogat domain to external services. Currently provides JIRA integration with assembler-based DTO conversion.

## Quick Start

```bash
uv sync --all-groups                        # install deps
pre-commit install --hook-type pre-commit --hook-type post-commit  # setup hooks
uv run pytest                               # run tests
./dev/bin/refresh-docs                      # rebuild docs (strict)
release                                     # trigger release workflow (TUI)
```

## Architecture

```text
src/doogat/integrations/
└── jira/
    ├── jira.py              # JIRA integration entry point
    └── assemblers/          # Domain ↔ JIRA DTO conversion
        └── project_zettel_jira_issue.py
```

**Key patterns:**

- **Assembler pattern**: convert between domain objects and external service DTOs
- Depends on buvis-pybase (adapters) and doogat-core (entities)

## Code Conventions

**Type hints** - modern style, no `Optional`:

```python
from __future__ import annotations
def foo(path: Path | None = None) -> list[str]: ...
```

**Imports**:

- Explicit `__all__` in `__init__.py`
- `TYPE_CHECKING` guards for type-only imports
- Platform-specific conditional imports in adapters

**Docstrings**: Google format

## Testing

- pytest + pytest-mock
- Tests in `tests/` mirror `src/` structure
- Mock subprocess calls heavily
- Class-based test organization
- **No unused imports/variables**: don't add `noqa: F401` or `noqa: F841` - either use the import/variable or remove it
- If a test creates an object just for side effects (e.g., testing `__init__`), add an assertion to use it: `assert issubclass(Foo, Base)`

```python
@pytest.fixture
def assembler():
    return ProjectZettelJiraIssueDTOAssembler(defaults=make_defaults())

class TestToDtoMapping:
    def test_enhancement_mapping(self, assembler): ...
```

## Release

```bash
release  # TUI: pick release type, bump level, confirm, watch
```

Triggers the GitHub Actions release workflow. Alternative: Actions tab → Release → Run workflow.

Version determined from conventional commits (`feat:` → minor, `fix:` → patch).

See `dev/docs/versioning.md` for details.
