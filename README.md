# Best Practices Skill

This repository hosts the `best-practices` AI agent skill. It acts as a single source of truth for standardizing repository setup, enforcing reproducible environments (`uv`, `bun`), and ensuring quality checks (`pre-commit`, linters).

## Usage

To use this skill in your project via `skills.sh`, run:

```bash
npx skills add https://github.com/manojbajaj95/best-practices --skill best-practices
```

This will provide your AI agent with all the necessary context to format, lint, and structure your repository according to standardized best practices.

## Included Practices

- **Functional**: Enforces `README.md`, `LICENSE`, tests, type checking, and modern linters.
- **Infrastructure**: Enforces `AGENTS.md`, `pre-commit` hooks, branch protection awareness, and reproducible environments (`uv` for Python, `bun` for Node).
- **Skills**: Automates the installation of stack-specific agent skills via `autoskills`.
