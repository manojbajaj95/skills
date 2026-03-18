# Installation Guide for Agents

Use this document when setting up a repository with my preferred defaults.

The goal is not to force every repo into the same shape. The goal is to quickly recognize what kind of project this is, preserve existing good conventions, and add the right level of structure for humans and agents to work well together.

## Primary Goal

When you initialize or clean up a repo for me, optimize for:

- clear setup instructions
- agent-friendly documentation
- lightweight but useful structure
- conventions that match the project type
- minimal ceremony

Do not add generic boilerplate just because it is common.

## Project Types I Commonly Use

Most repos I create fall into one of these buckets:

1. Frontend app
2. FastAPI backend app
3. Python library
4. Playground or hacking repo for scripts, experiments, and analytics

Your first job is to classify the repo correctly.

## Step 1: Identify The Repo Type

Inspect the existing files and infer the closest project type.

### Frontend app

Strong signals:

- `package.json`
- `next.config.*`
- `vite.config.*`
- `src/` with React, Next.js, or frontend code
- `app/` for Next.js
- Tailwind, TypeScript, ESLint, Vitest, Playwright

### FastAPI backend app

Strong signals:

- `fastapi` in dependencies
- `pyproject.toml`
- `requirements.txt`
- `app/main.py`, `main.py`, or API routers
- `uvicorn`, `pydantic`, `alembic`, `sqlalchemy`

### Python library

Strong signals:

- `pyproject.toml`
- package directory with importable module
- `tests/`
- no obvious app entrypoint
- focus on reusable functions, classes, SDKs, or tooling

### Playground / hacking / scripting / analytics repo

Strong signals:

- notebooks
- scripts folder
- ad hoc data processing
- one-off analyses
- experiments and prototypes
- mixed utilities without a polished product structure

If the repo does not fit perfectly, choose the closest type and explain the assumption in your final summary.

## Step 2: Inspect Before Editing

Before adding anything, check:

- existing `README.md`
- top-level structure
- lockfiles and package manager
- test setup
- lint and format setup
- CI config
- any contributor or agent docs

If the repo already has a good pattern, work with it.

Do not replace working conventions unless there is a clear reason.

## Step 3: Install The Minimum Useful Docs

For almost every repo, create or improve:

- `README.md`
- agent instructions if the repo needs them
- setup notes that explain how to run, test, and work in the repo

Every setup should make it easier for the next agent to answer:

- What is this repo for?
- How do I run it?
- How do I test it?
- What conventions matter here?
- What should I avoid changing casually?

## Installing Skills

When a repo would benefit from agent skills, install them explicitly instead of assuming they are already available.

Use this command pattern:

```bash
npx skills add <skill-repo-url>
```

Example:

```bash
npx skills add https://github.com/supabase/agent-skills
```

For custom skills from this repo, use this pattern:

```bash
npx skills add https://github.com/manojbajaj95/best-practices --skill <skill_name>
```

Example:

```bash
npx skills add https://github.com/manojbajaj95/best-practices --skill release-please-changelog
```

Use one command per skill source unless the skill tool supports a different flow in that repo.

After installing skills:

- verify they are available to the agent
- mention them in repo setup notes if they are important to ongoing work
- use them when they clearly match the task

## Shared Repo Baselines

For every repo, configure these directly as part of setup instead of treating them as skills:

- use Conventional Commits
- set up `pre-commit` if it fits the repo tooling

Add these expectations to the repo's agent instruction file, usually `AGENTS.md` or `CLAUDE.md`.

The agent instructions should explicitly say:

- use Conventional Commit style messages and PR titles when making changes
- keep commit prefixes clear, such as `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`
- use breaking change markers when appropriate
- run `pre-commit` before finishing when the repo has it configured
- update or maintain `.pre-commit-config.yaml` as part of repo setup when useful

If neither `agents.md` nor `CLAUDE.md` exists, create the one that best fits the repo's current conventions and put these baseline instructions there.

## All Python Projects

For every Python project, install these skills:

- [ruff](https://skills.sh/astral-sh/claude-code-plugins/ruff)
- [uv](https://skills.sh/astral-sh/claude-code-plugins/uv)
- [ty](https://skills.sh/astral-sh/claude-code-plugins/ty)

Use these commands:

```bash
npx skills add https://github.com/astral-sh/claude-code-plugins --skill ruff
npx skills add https://github.com/astral-sh/claude-code-plugins --skill uv
npx skills add https://github.com/astral-sh/claude-code-plugins --skill ty
```

This applies to:

- FastAPI backend apps
- Python libraries
- playground, scripting, and analytics repos that are primarily Python-based

## Default Expectations By Repo Type

### 1. Frontend App

Optimize for developer speed, visual consistency, and safe UI changes.

Usually include:

- clear local dev instructions
- build and test commands
- lint and format commands
- notes on UI patterns or design system if present
- environment variable setup if needed
- deployment notes if they are simple and important

Install these skills for frontend projects:

- [vercel-react-best-practices](https://skills.sh/vercel-labs/agent-skills/vercel-react-best-practices)
- [web-design-guidelines](https://skills.sh/vercel-labs/agent-skills/web-design-guidelines)
- [shadcn](https://skills.sh/shadcn/ui/shadcn)
- [vercel-composition-patterns](https://skills.sh/vercel-labs/agent-skills/vercel-composition-patterns)

Helpful structure:

- `src/`, `app/`, `components/`, or existing framework layout
- `docs/` only if there is enough complexity to justify it
- agent notes for UI review, testing, and product constraints

Agent focus:

- preserve existing visual language
- avoid generic redesigns
- respect accessibility and responsiveness
- explain where UI components live
- use the installed frontend skills when making or reviewing UI changes

### 2. FastAPI Backend App

Optimize for clarity, local run instructions, and safe API changes.

Usually include:

- setup instructions for Python environment
- install command
- local server run command
- test command
- migration instructions if relevant
- environment configuration notes
- API layout overview if the repo is non-trivial

Install these skills for FastAPI backend projects:

- install all Python project skills listed above
- [fastapi](https://skills.sh/fastapi/fastapi/fastapi)
- [postgresql-optimization](https://skills.sh/github/awesome-copilot/postgresql-optimization)
- [supabase-postgres-best-practices](https://skills.sh/supabase/agent-skills/supabase-postgres-best-practices)

Helpful structure:

- `app/`
- `tests/`
- `docs/` for architecture, API notes, or operations if needed

Agent focus:

- identify app entrypoint
- identify dependency management approach
- document how to run the API locally
- call out database and migration expectations
- use the installed FastAPI skill when making backend changes

### 3. Python Library

Optimize for packaging clarity, testability, and clean usage examples.

Usually include:

- install instructions
- editable install instructions if appropriate
- test command
- lint or typecheck command if present
- short usage example
- package layout overview

Install all Python project skills listed above for Python libraries.

Also install these custom skills for Python libraries:

- `npx skills add https://github.com/manojbajaj95/best-practices --skill uv-trusted-publish-github-action`
- `npx skills add https://github.com/manojbajaj95/best-practices --skill release-please-changelog`

Helpful structure:

- library package directory
- `tests/`
- `docs/` only if the public API or architecture needs explanation

Agent focus:

- make the package purpose obvious
- document public usage before internal details
- keep setup simple and reproducible

### 4. Playground / Hacking / Scripts / Analytics Repo

Optimize for flexibility without chaos.

Usually include:

- what kinds of work happen here
- how scripts are organized
- how to install dependencies
- how to run common scripts or notebooks
- where data, outputs, and experiments should live

Helpful structure:

- `scripts/`
- `notebooks/`
- `data/` if appropriate
- `outputs/` if appropriate
- `docs/` only when experiments need context

Agent focus:

- do not over-engineer
- prefer lightweight conventions
- separate disposable experiments from reusable scripts
- keep README practical

If this repo is primarily Python-based, also install all Python project skills listed above.

## Recommended Setup Flow

Use this sequence:

1. Identify the repo type
2. Inspect existing conventions
3. Decide what is missing
4. Update or create `README.md`
5. Add agent-facing guidance if the repo will benefit from it
6. Add lightweight structure only where needed
7. Verify commands and docs match the actual repo

## Agent Documentation Guidance

If you add agent instructions, they should be operational, not philosophical.

Good agent docs should include:

- project purpose
- key directories
- run, test, and lint commands
- commit and PR conventions
- `pre-commit` expectations if configured
- stack-specific constraints
- review expectations
- any product or architecture gotchas

Keep them short enough to scan quickly.

## Deployment

If deployment work is part of repo setup, ask where the project should be deployed before proceeding.

Do not assume the deployment platform if it is not already obvious from the repo.

Ask a direct question like:

- Where should this project be deployed?

If the answer is Vercel:

- install [deploy-to-vercel](https://skills.sh/vercel-labs/agent-skills/deploy-to-vercel)
- use the Vercel deployment skill for setup and deployment work

If the answer is Cloudflare:

- install [wrangler](https://skills.sh/cloudflare/skills/wrangler)
- use the Cloudflare skill for setup and deployment work

If the repo already clearly targets one platform, follow the existing convention and document it in the repo setup notes.

## What Not To Do

Avoid these failure modes:

- adding heavy process to a simple repo
- inventing architecture that the project does not need
- replacing existing conventions without cause
- adding placeholder docs that say nothing useful
- creating many folders before there is real content for them

## Baseline Checklist

Before finishing, confirm:

- the repo type is identified
- setup docs match the real commands and tools
- README explains the repo clearly
- agent guidance is practical
- changes fit the actual complexity of the project
- no unnecessary boilerplate was added

## Final Output Standard

A successful setup should leave the repo in a state where another agent can open it and quickly understand:

- what the project is
- how to run it
- how to test it
- how to contribute safely
- what style of repo it is supposed to be

If there is uncertainty, make the best reasonable choice, implement it, and clearly state your assumption.
