---
id: 9ad62516-5e2d-4c8d-9147-1a7411793885
---
# Wiki meta file

`.wiki-meta.json` at the bundle root tracks generation state for incremental **update** runs. Navigation order lives in `index.md` listings — see [toc-and-layout.md — Page ordering](toc-and-layout.md#page-ordering).

## Write `.wiki-meta.json`

After generating or changing concept pages, create or update `.wiki-meta.json` in the wiki directory root. In the same assembly pass, refresh root/section `index.md` listings (the canonical TOC) and root `log.md`.

Derive the `commitHash` and `branch` fields by running:

```bash
git rev-parse HEAD          # → commitHash
git rev-parse --abbrev-ref HEAD  # → branch
```

## Fields

| Field | Required | Purpose |
|-------|----------|---------|
| `generatedAt` | Yes      | ISO 8601 timestamp of this generation/update |
| `commitHash` | Yes      | Source commit documented — baseline for the next **update** diff |
| `branch` | Yes      | Branch name when generated |

Do not store page order here. Root and section `index.md` files are the source of truth for TOC sequence.

On a true **update** no-op (no concept content changed), do not rewrite `.wiki-meta.json`.

## Example

```json
{
  "generatedAt": "2025-01-15T10:30:00Z",
  "commitHash": "abc123def456",
  "branch": "main"
}
```
