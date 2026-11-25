# Contributing to RAGDB

Thanks for helping improve RAGDB. Small, focused pull requests are easier to review and merge.

## Before you start

- Read the [Code of Conduct](CODE_OF_CONDUCT.md).
- For security issues, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.
- Search existing issues and pull requests before opening a new one.
- Do not include private knowledge-base data, API keys, signed URLs, or personal configuration files.

## Development setup

RAGDB targets Python 3.11.

```powershell
git clone https://github.com/Qorq-byte/RAG_db_test.git
cd RAG_db_test
uv sync --frozen
Copy-Item config.example.toml config.toml
```

Use isolated temporary data directories for tests. Do not run development commands against a personal library.

## Making changes

1. Create a focused branch from the current default branch.
2. Explain the user-facing problem and intended behavior before changing code.
3. Reuse existing application services, domain contracts, and adapters where possible.
4. Add or update tests for behavior changes and failure paths.
5. Update the relevant README, user guide, or acceptance record when behavior or setup changes.
6. Keep commits small and describe each commit with an imperative subject.

## Checks

Run the checks that match your change. Before opening a pull request, run the full suite when practical.

```powershell
uv run pytest -q
uv run python -m compileall -q src tests
git diff --check
```

If a check requires Ollama, Supabase, a cloud provider, or Windows GUI behavior, record the exact environment and result. Do not claim an external integration was tested when only a mock or offline test was run.

## Pull requests

A pull request should include the problem, the resulting behavior, tests run, and any external dependency that could not be verified. Keep unrelated formatting and generated files out of the change. Maintainers may ask for a smaller split when a pull request covers independent concerns.

By contributing, you agree that your contribution is provided under the repository's [MIT License](LICENSE).
