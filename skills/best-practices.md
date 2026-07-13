---
name: best-practices
description: Apply standardized project setup and best practices (Functional, Infrastructure, Skills). Use when setting up or reviewing a repository.
---

# Best Practices & Standardized Project Setup

When reviewing, initializing, or cleaning up a repository, you must enforce the following standardized practices. Do not replace working conventions unless there is a clear reason, but ensure the repository meets these baselines.

## Stage 1: Functional Setup
Ensure the baseline functionality and documentation exist:
- **README.md**: Must explain what the repo is for, how to run it, and how to test it.
- **License**: Ensure an appropriate LICENSE file is present.
- **Tests Setup**: Ensure a testing framework is configured (e.g., `pytest` for Python, `vitest` for Node).
- **Type Checker**: Enforce type checking (e.g., `mypy`/`pyright` for Python, `tsc` for Node).
- **Linter**: Enforce modern linters (`ruff` for Python, `eslint`/`biome` for Node).

## Stage 2: Repository & Agent Infrastructure
Set up tooling for reproducibility and AI agent friendliness:
- **AGENTS.md**: Ensure this file exists to guide future LLM agents on repo conventions.
- **Pre-commit Hooks**: Set up `.pre-commit-config.yaml` to enforce formatting and linting automatically. Run `pre-commit run --all-files` before finalizing changes.
- **Branch Protection Rules**: Expect the repository to have branch protection on `main` (requires PRs, reviews, and passing CI). Create feature branches for work.
- **Conventional Commits**: Always use Conventional Commits (`feat:`, `fix:`, `chore:`, etc.) for commits and PR titles.
- **Reproducible Dev Environments**:
  - **Python**: Always use `uv` for environment management, dependency resolution, and running scripts (`uv run`). Do not use `pip` or `venv` directly unless constrained.
  - **Node/Frontend**: Always use `bun` as the preferred JavaScript runtime and package manager.

## Stage 3: Installing Relevant Skills
Ensure the repository is equipped with the right agent skills.
- Use `npx autoskills` to auto-detect the tech stack and auto-install the best AI agent skills.
- For **Python** projects, actively install these skills if missing:
  ```bash
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill ruff
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill uv
  npx skills add https://github.com/astral-sh/claude-code-plugins --skill ty
  ```
- For **FastAPI** backend apps, add:
  ```bash
  npx skills add https://skills.sh/fastapi/fastapi/fastapi
  ```

## File Generation Directives
- **Do not invent boilerplate.** Only create what is necessary for the current task.
- Leave the project in a state where another agent or human can read `README.md` and `AGENTS.md` and immediately know how to operate.

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
