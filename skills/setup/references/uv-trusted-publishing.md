# Uv trusted publishing (GitHub Actions → PyPI)

Use this when setting up a **publishable Python package** to publish from GitHub Actions with `uv` and PyPI trusted publishing (OIDC), without long-lived PyPI API tokens in secrets.

## When it applies

- The repo (or a package in a monorepo) is a real Python library/distribution
- `pyproject.toml` defines publishable package metadata and a build backend
- The user wants CI publishing to PyPI (and optionally TestPyPI)

Do **not** add this for apps, scripts-only repos, marketing sites, or docs unless the user explicitly wants a published package.

## Inspect first

- `pyproject.toml` (name, version, build system)
- whether `uv build` already works
- existing `.github/workflows/` publish or release jobs
- whether release-please (or other release automation) already owns version bumps

## Confirm with the user when unclear

- PyPI vs TestPyPI first
- which branch/tag/release event should trigger publish
- whether a PyPI project and trusted publisher are already configured
- how publishing should relate to release-please, if both are in scope

## Preferred pattern

- `permissions: id-token: write`
- a GitHub Environment (commonly `pypi`) that matches PyPI trusted publisher settings
- `astral-sh/setup-uv`
- `uv build`
- `uv publish`

Trusted publishing means GitHub Actions presents an OIDC token that PyPI trusts for this repo/environment — no static PyPI token in GitHub secrets.

Keep the publish workflow minimal and aligned with the repo's release flow (tags, GitHub Release, or after a release PR merges). Do not invent a complex pipeline if a single publish job is enough.

## Validate before finishing

- `uv build` succeeds for the package layout
- workflow has `id-token: write`
- environment name matches what must be configured on PyPI
- package metadata is complete enough to publish
- call out any required **PyPI-side** trusted publisher configuration the user must do in the browser

## Official references

- https://docs.astral.sh/uv/guides/package/
- https://github.com/astral-sh/setup-uv
- https://github.com/astral-sh/trusted-publishing-examples
