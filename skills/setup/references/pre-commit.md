---
id: 7b999b33-d39c-4fe2-a678-dda51d155974
---
# Pre-commit checklist

Use this when the setup plan includes git hooks. Keep configuration minimal and aligned with the repo's existing linters and formatters.

## When it applies

* Almost any code repo benefits from pre-commit once lint/format tools exist
* Skip or defer for throwaway experiments if the user does not care about hooks yet

## Checklist

- [ ] `.pre-commit-config.yaml` exists at the repo root (or documented monorepo equivalent)
- [ ] Hooks match tools already chosen for the stack (do not introduce a second formatter)
- [ ] Python: prefer ruff (and related) hooks when ruff is the project linter
- [ ] JS/TS: prefer the project's eslint/biome/prettier setup via hooks rather than a parallel tool
- [ ] Secrets/large-file checks are welcome when they do not fight the stack
- [ ] `pre-commit install` is documented in README or CONTRIBUTING
- [ ] Run `pre-commit run --all-files` once after setup and fix trivial fallout, or note failures for the user

## Guidance

Prefer a short config that enforces what CI already cares about. If the repo has no linter yet, set up lint/format first, then wire hooks — not the other way around.
