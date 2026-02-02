# Ecosystem Detection

Sub-agent analyzes project configuration to detect test and format commands.

## Detection Sources (Priority Order)

1. **Lock files**: `pnpm-lock.yaml`, `yarn.lock`, `package-lock.json`, `Gemfile.lock`, `poetry.lock`, `Cargo.lock`, etc.
2. **Config files**: `package.json` (scripts), `mix.exs`, `Makefile`, `pyproject.toml`, `Cargo.toml`, `go.mod`, etc.
3. **CI configs**: `.github/workflows/*.yml`, `.gitlab-ci.yml`, `.circleci/config.yml`
4. **README**: May document test/format commands

## Supported Ecosystems

| Indicator | Test | Format |
|-----------|------|--------|
| package.json | Check scripts for `test`, `test:*` | Check scripts for `fmt`, `format`, `lint:fix` |
| mix.exs | `mix test` | `mix format` |
| Gemfile | `bundle exec rspec` or `rake test` | `bundle exec rubocop -a` |
| pyproject.toml | `pytest` | `ruff format` or `black` |
| Cargo.toml | `cargo test` | `cargo fmt` |
| go.mod | `go test ./...` | `go fmt ./...` |
| Makefile | `make test` | `make fmt` or `make format` |

## Other Ecosystems

For ecosystems not listed above, sub-agent analyzes CI config and project structure to determine appropriate commands.

## Monorepo Handling

Sub-agent determines whether to run tests at root or in the affected package based on:
- Location of changed files
- Package manager workspace configuration
- CI workflow structure

## Command Not Detected

If test or format command cannot be determined:
- Display warning to user
- Skip the command (do not fail the workflow)
- Proceed to next step
