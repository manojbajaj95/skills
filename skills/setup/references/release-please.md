---
id: 68297f8e-81ac-4945-adc1-769b729b1708
---
# Release Please + changelog

Use this when the user wants automated release PRs and changelog generation as part of repo setup, or when a library/monorepo clearly needs versioned releases.

Base the work on the official `googleapis/release-please` project and `release-please-action`.

## When it applies

* Libraries and versioned packages
* Monorepos with one or more releasable components
* Apps that intentionally ship SemVer tags and changelogs

Skip for marketing sites, docs-only repos, and internal apps that do not publish versioned releases — unless the user asks anyway.

## Inspect first

* `.github/workflows/release-please.yml` (or similar)
* `release-please-config.json`
* `.release-please-manifest.json`
* `CHANGELOG.md`
* package metadata (`package.json`, `pyproject.toml`, etc.)
* existing tags and whether Conventional Commits are already in use

## Strategy

Before writing config, determine:

* single-package vs monorepo / multi-component
* which path(s) should release
* which file is the version source of truth
* whether changelog automation already exists

`release-please` expects Conventional Commit style history for good SemVer and changelog quality (`fix:` → patch, `feat:` → minor, `!` / `BREAKING CHANGE` → major). If history is messy, say so and still set up for future commits rather than rewriting history.

## Minimal setup

Usually add:


1. GitHub Actions workflow using the official `release-please-action`
2. `release-please-config.json`
3. `.release-please-manifest.json` when using manifest mode (monorepos / multiple components)

Prefer manifest-based config for monorepos. Do not invent monorepo complexity for a single package.

## Quality and validation

* Keep release note categories meaningful; avoid noisy internal-only wording in changelog-facing entries
* Confirm workflow paths, config paths, and versioned files match the real layout
* Confirm triggers match the default branch
* Explain how releases will work after merge

## Official references

* https://github.com/googleapis/release-please
* https://github.com/googleapis/release-please-action
