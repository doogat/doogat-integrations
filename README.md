# doogat-integrations

Integration layer connecting the Doogat domain to external services. Currently provides JIRA integration with assembler-based DTO conversion.

## Install

```bash
pip install doogat-integrations
```

## Features

- **JIRA integration** - Create and sync JIRA issues from project zettels
- **Assemblers** - Convert between domain objects and external service DTOs

## Development

```bash
uv sync --all-groups                        # install deps
pre-commit install --hook-type pre-commit --hook-type post-commit  # setup hooks
uv run pytest                               # run tests
```

## Release

Releases via GitHub Actions (manual trigger):

1. Go to Actions → Release workflow → Run workflow
2. Choose `prerelease` (test.pypi only) or `release` (both pypis + GitHub release)

Version determined from conventional commits (`feat:` → minor, `fix:` → patch).

```bash
uv run semantic-release version --print --noop  # preview next version
```

See [dev/docs/versioning.md](dev/docs/versioning.md) for details.

## License

GPL-3.0
