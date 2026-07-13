---
name: setup
description: >-
  Initialize or scaffold a repository with standardized project baselines
  (README, license, tests, lint/types, AGENTS.md, pre-commit, CI, skills).
  Use when the user asks to set up a new repo, bootstrap project tooling,
  or apply your standard repo setup — not for routine feature work or
  code review. Prefer this skill whenever they say "set up this repo",
  "scaffold", "initialize the project", or want release-please / pre-commit /
  PyPI trusted publishing as part of initial setup.
---

# Repository Setup

Help the user bring a repository to a solid baseline. Prefer understanding and confirmation over spraying every tool into every repo.

## Mindset: not eager

Setup is easy to overdo. A marketing site does not need PyPI publishing; a docs repo may not need a heavy test matrix; a monorepo needs different release config than a single library.

- Infer what you can from the tree, then **state your working assumptions**.
- If repo type, stack, hosting, or whether a step applies is ambiguous, **ask before changing files**.
- Apply only baselines that fit this repo. Skip or defer the rest and say why.
- Do not invent boilerplate. Leave the repo so a human or agent can operate from `README.md` and `AGENTS.md`.

## Phase 1: Understand the repository

Inspect the workspace before proposing a plan. Read package manifests, lockfiles, existing CI, README, and directory layout.

### Repo shape (pick the best fit; confirm if unsure)

| Shape | Signals |
| --- | --- |
| Monorepo | Multiple packages/apps, workspaces, `apps/` + `packages/`, turborepo/pnpm-workspaces |
| Landing / marketing / blogs | Content-heavy, CMS or MDX, little backend, static or SSG |
| Backend | API/server entrypoints, OpenAPI, workers, auth, DB migrations |
| Library | Publishable package metadata, `src/` + exports, consumers expected |
| Documentation | Docs framework, mostly markdown, few runtime deps |

Repos can be mixed (e.g. monorepo with a marketing site and an API). Name the mix explicitly.

### Tech stack (detect; do not hard-code a short list)

Use whatever the repo actually uses. Frameworks like Next.js, Astro, or FastAPI are examples, not an exclusive set. Note language(s), package manager, frameworks, test runners, and **infrastructure** when visible (database, hosting, IaC, container setup).

If detection conflicts (e.g. both `package.json` and `pyproject.toml` with no clear primary app), ask which surface this setup pass should target.

## Phase 2: Confirm the plan

Present a short plan:

1. What you think the repo is (shape + stack + infra)
2. Baselines you will apply now
3. Optional steps you will skip or ask about (release automation, publishing, branch protection, extra skills)

Wait for confirmation when anything material is unclear or when optional tooling would change release/publish behavior.

## Phase 3: Functional baselines

Ensure these exist and match the stack — create or wire them only when missing or broken for this repo type:

- **README.md** — what the repo is, how to run it, how to test it
- **LICENSE** — appropriate license file if the user wants one (ask if missing and intent is unclear)
- **Tests** — a real test runner for the stack (e.g. pytest, vitest) when the repo has executable code worth testing
- **Type checking** — project-appropriate (e.g. mypy/pyright, `tsc`)
- **Linter / formatter** — project-appropriate (e.g. ruff, eslint/biome)

Prefer the repo's existing tools over replacing them. For new Python work prefer `uv`; for new JS/TS prefer `bun` unless the repo already standardized on something else.

## Phase 4: Agent and repo infrastructure

- **AGENTS.md** — conventions future agents need (commands, layout, non-obvious rules)
- **CONTRIBUTING.md** — human/agent contribution norms when useful; keep it short. Point contributors at engineering principles rather than pasting a long essay into every repo
- **Conventional Commits** — use and document `feat:`, `fix:`, `chore:`, etc. for commits and PR titles
- **Branch protection** — expect `main` to require PRs, reviews, and passing CI; create feature branches for work. Configure via GitHub only when the user asks and access is available
- **Pre-commit** — see [references/pre-commit.md](references/pre-commit.md)
- **CI** — minimal workflows that match the stack (lint, typecheck, test) when none exist

## Phase 5: Optional release and publish tooling

Only when the repo shape warrants it and the user agrees:

- **release-please / changelog** — libraries, versioned apps, monorepos with releasable packages. See [references/release-please.md](references/release-please.md)
- **uv trusted publishing** — publishable Python packages only. See [references/uv-trusted-publishing.md](references/uv-trusted-publishing.md)

Do not add publishing to apps, marketing sites, or docs-only repos unless the user explicitly wants it.

## Phase 6: Relevant agent skills

Equip the repo with skills that match the stack.

- Prefer `npx autoskills` to detect and install sensible defaults when available
- For Python projects, consider Astral skills when missing (`ruff`, `uv`, `ty`)
- For FastAPI backends, consider the FastAPI skill when relevant

Install only what clearly helps this repo. Ask before bulk-installing skills.

## Done criteria

Finish when:

- Assumptions and skips are stated
- Applied baselines match the confirmed plan
- `README.md` and `AGENTS.md` are enough to operate the repo
- Optional release/publish steps are either correctly wired or explicitly deferred
