# Setup a repository

Skill for initializing a repository with the baselines you care about — without applying every tool to every repo.

## What it sets up (when appropriate)

- README.md / LICENSE
- Tests, type checkers, linters
- AGENTS.md and short CONTRIBUTING.md
- Pre-commit hooks
- CI
- GitHub branch-protection expectations
- Stack-appropriate agent skills
- Optional: release-please, uv trusted publishing to PyPI

## How it behaves

1. Infers repo shape (monorepo, marketing/landing/blogs, backend, library, docs, or a mix)
2. Infers tech stack and infra from the tree
3. Confirms a plan when anything is ambiguous
4. Applies only what fits; defers release/publish unless warranted

Detailed guides live under `references/`.
