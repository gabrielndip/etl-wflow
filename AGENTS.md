# Repository Guidelines

## Project Structure & Module Organization
- Source code lives under `src/etl_sys/` (package name: `etl_sys`).
- Jobs/pipelines: `src/etl_sys/jobs/` and `src/etl_sys/pipelines/`.
- Configs and schemas: `configs/` and `schemas/` (keep examples in `configs/example/`).
- Tests: `tests/` mirroring `src/` (e.g., `tests/jobs/test_daily_ingest.py`).
- Utilities and scripts: `scripts/` (CLI helpers, data checks).
- Data/artifacts should not be committed; use `data/` in `.gitignore` for local runs.

## Build, Test, and Development Commands
- `make install` — set up env (deps, pre-commit hooks).
- `make lint` — run static checks (ruff/flake8, isort, mypy).
- `make format` — apply formatting (black + isort).
- `make test` — run `pytest` with coverage.
- `make run JOB=daily_ingest` — run a job entrypoint (see `src/etl_sys/jobs/`).
If `make` is unavailable, use: `pip install -r requirements.txt`, `pytest -q`, `black .`, `ruff check .`.

## Coding Style & Naming Conventions
- Python 3.11+, 4-space indentation, UTF-8.
- Formatting: black; Imports: isort; Lint: ruff/flake8; Types: mypy (strict on `src/etl_sys`).
- Naming: modules/files `snake_case.py`; classes `PascalCase`; functions/vars `snake_case`; constants `UPPER_SNAKE`.
- Keep functions cohesive; prefer pure utilities in `src/etl_sys/lib/`.

## Testing Guidelines
- Framework: `pytest` with `pytest-cov`.
- Place tests in `tests/` mirroring paths; name files `test_*.py` and tests `test_*`.
- Use fixtures in `tests/conftest.py`; avoid network/IO in unit tests.
- Coverage target: ≥85% on `src/etl_sys`; run `pytest --maxfail=1 -q`.

## Commit & Pull Request Guidelines
- Use Conventional Commits (e.g., `feat: add s3 extractor`, `fix: handle empty batch`).
- One logical change per PR; keep diffs small and focused.
- PRs must include: summary, rationale, screenshots/logs for flows, and linked issue.
- Update docs/config examples when behavior or interfaces change.

## Security & Configuration Tips
- Never commit secrets or real data. Load config via env or `.env` (gitignored).
- Validate config with Pydantic models in `src/etl_sys/config.py`.
- Prefer parameterized credentials and least-privilege IAM for cloud resources.
