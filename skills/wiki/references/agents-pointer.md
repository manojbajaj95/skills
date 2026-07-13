# Agents pointer

Keep a short pointer to the wiki in top-level agent instruction files so future agents read `wiki/` first.

## Rules

- Only touch **top-level** `AGENTS.md` and/or `CLAUDE.md`. Never edit nested copies.
- If one or both exist, add or update the wiki section in them. If both exist, use the same section in both.
- If neither exists, create top-level `AGENTS.md` containing only this section.
- On **update**, refresh only if the section is missing or semantically stale. Preserve surrounding content. Replace in place — never duplicate. No formatting-only edits.
- Do not expand this into a long essay; the wiki itself holds the knowledge.

## Exact section

Use this block (do not reword — stable text makes dedupe reliable):

```markdown
## Wiki

This repository has documentation in the /wiki directory.

Start here:
- [Wiki index](wiki/index.md)

When working in this repository, read the wiki index first, then follow its links to the relevant overview, architecture, and domain pages.
```
