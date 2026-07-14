---
id: cb9044b5-5446-4c0a-95c5-21f211f62fd1
---
# Wiki ignore file

`.wikiignore` at the **repository root** lists paths the wiki must never read when surveying source, verifying pages, or drafting updates. It complements `.gitignore`: agents must treat **both** as off limits.

## Location

| File | Location | Role |
|------|----------|------|
| `.wikiignore` | Repo root | Wiki-specific read exclusions |
| `.gitignore` | Repo root | Also excluded from wiki reads (always) |
| `.wiki-meta.json` | `wiki/` root | Generation baseline — not an ignore file |

Keep `.wikiignore` separate from the `wiki/` bundle. Do not move it under `wiki/` or link it from `index.md`.

## Default exclusions

The skill ships a canonical template at `skills/wiki/.wikiignore`. On **init**, write `.wikiignore` at the repo root when the file is missing. On **update**, do not rewrite an existing `.wikiignore` unless the user asks.

Always skip:


1. **Every pattern in** `**.gitignore**` — parse and respect repo `.gitignore` rules before reading source.
2. **Paths in** `**.wikiignore**` — even when not covered by `.gitignore`.

Built-in wiki-only paths (in the template):

| Path | Why |
|------|-----|
| `.env`, `.env.*` | Secrets and local environment (`.env.example` is allowed) |
| `.agents/` | Local agent config — not repository knowledge |
| `.cursor/` | IDE-local config — not repository knowledge |

## Agent behavior

During **init** survey, **update** diffs, **ask** source verification, and sub-agent page writing:


1. Read `.wikiignore` at repo root when present.
2. Load `.gitignore` and treat matched paths as unreadable.
3. Union both sets — if either excludes a path, do not open it.
4. Never document secret values, credentials, tokens, or private keys from any source.

`.env.example` and similar placeholder configs are OK only when they contain no real secrets.

## Customization

Repos may append project-specific paths to `.wikiignore` (generated artifacts, local overrides, vendor trees not in `.gitignore`, etc.). Use gitignore-style patterns: one pattern per line, `#` comments, `!` negation.

Example additions:

```gitignore
# Local overrides
.local/

# Large generated trees not in .gitignore
dist/
```

Do not add `wiki/` to `.wikiignore` — the wiki reads its own bundle.

## Init checklist

When assembling a new wiki:


1. If `.wikiignore` is missing at repo root, create it from the skill template (`skills/wiki/.wikiignore`).
2. Proceed with survey and page writing using the ignore rules above.
