# Agent Operating Instructions

Welcome. If you are an AI agent reading this, these are the operational rules and context you need to work effectively in this project and any project governed by my best practices.

## Core Directives

1. **Follow the Setup Levels**: When initializing a new project or updating an existing one, strictly adhere to the stages (Functional -> Infrastructure -> Skills).
2. **Reproducible Environments**: 
   - Python: Always use `uv` for environment management, dependency resolution, and running scripts (`uv run`). Do not use `pip` or `venv` directly unless constrained by the system.
   - Node: Always use `bun` as the runtime and package manager. Avoid `npm` or `yarn` when `bun` is available.
3. **Automated Quality**:
   - Install and update `.pre-commit-config.yaml`. Before committing, ensure you run `pre-commit run --all-files` if applicable.
   - Set up linters appropriate to the stack (`ruff` for Python, `eslint`/`biome` for JS/TS).
   - Ensure type checkers are active (`mypy`/`pyright` for Python, `tsc` for JS/TS).

## Engineering principles

These principles apply to all contributions — human and AI alike.

**YAGNI — You Aren't Gonna Need It.**
Implement only what the current task demands. Build for today's requirements; future requirements will arrive with future context.

**Use trusted libraries over reinventing.**
Reach for a well-maintained dependency before writing your own crypto, HTTP client, or token parser. The goal is a working credential manager, not a showcase of custom implementations. When a library exists and is maintained, use it.

**Deep modules over shallow ones.**
Prefer a module with a small surface area and rich internals over a sprawl of thin wrappers. A single `AuthClient` that handles everything cleanly beats a dozen one-method classes. More files is not more modular.

**Single responsibility and separation of concerns.**
Auth authenticates. Vault stores credentials. The CLI presents output. These boundaries are not negotiable — a flow should not write to storage, and storage should not know about OAuth. If a function is hard to name, it's doing too many things.

**Premature optimization is evil.**
Don't add caching, batching, or concurrency before you have a measured performance problem. Simple code that's slow is fixable; complex code that's wrong is not.

**Don't do it just because you can.**
Clever is a cost, not a benefit. If a feature, abstraction, or refactor isn't directly solving a real problem that exists today, skip it.

**Leave it better than you found it.**
Every change is an opportunity to fix a nearby typo, remove a dead import, or clarify a confusing comment — not the whole file, just the immediate vicinity. Small improvements compound.

**Comment the why, not the what.**
Use Google-style docstrings for all public interfaces. Inline comments should explain non-obvious invariants, workarounds, or hidden constraints — not restate what the code already says clearly.

**Update docs alongside code.**
If you change behavior, update the relevant docstring, `README.md`, or this file in the same commit. Documentation debt accumulates faster than technical debt and is harder to pay down later.

## Agent Tooling & Skills

- You are expected to be capable. If the repository lacks specific context, **find and install relevant skills** using `skills.sh`.
- Run `npx autoskills` to auto-detect the tech stack and auto-install the best AI skills for the project.
- For Python environments, you must ensure skills for `uv`, `ty`, and `ruff` are installed:
  ```bash
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill uv
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill ruff
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill ty
  ```

## Git Workflow & Branch Protection

- Use **Conventional Commits** (`feat:`, `fix:`, `chore:`, `docs:`, etc.).
- Ensure your changes pass local linters and type checkers.
- Projects are expected to have branch protection rules on `main` (requiring PRs, reviews, and passing CI checks). Act accordingly by creating branches for your work and providing clear PR descriptions.

## File Generation

- **Do not invent boilerplate.** Only create what is necessary for the current task.
- Ensure all new modules have accompanying tests if applicable.
- Leave the project in a state where another agent can read `README.md` and `AGENTS.md` and immediately know how to run, test, and contribute.
