---
name: wiki
description: >-
  Maintain an in-repo markdown wiki under wiki/: init a new wiki, surgically
  update it after code changes, or answer questions from it. Use for /wiki init,
  /wiki update, /wiki ask, "generate the wiki", "refresh the docs", "how does X
  work in this repo" when wiki/ exists, deep-wiki style docs, or keep the wiki
  in sync — never wipe an existing wiki; prefer small incremental edits. Also
  use when installing CI that runs wiki update on push.
disable-model-invocation: true
---

# Wiki

In-repo markdown wiki under `wiki/`. Three triggers — pick one from the user request:

| Trigger | When | Reference |
| --- | --- | --- |
| **init** | No usable `wiki/` yet; user asks to create/generate/bootstrap the wiki | [references/init-and-update.md](references/init-and-update.md) (init path) + [references/structure-and-format.md](references/structure-and-format.md) |
| **update** | `wiki/` exists; user asks to refresh/sync/update after code changes | [references/init-and-update.md](references/init-and-update.md) (update path) + [references/structure-and-format.md](references/structure-and-format.md) |
| **ask** | User asks how/where/why about the repo and `wiki/` should be the primary source | [references/ask.md](references/ask.md) |

**Install CI** (auto-update on push): [references/install.md](references/install.md).

## Hard rules

- **Never wipe** an existing `wiki/`. If it exists with real content, use **update** (even if the user said "generate" or "rebuild"). Only **init** when there is no usable wiki.
- Prefer **small surgical edits** on update. No formatting-only churn. No-op is success when nothing relevant changed.
- Respect `.wikiignore` at repo root and all `.gitignore` patterns — never read excluded paths. See [references/wikiignore.md](references/wikiignore.md).
- Never read or document secrets, credentials, or `.env` files (`.env.example` placeholders are OK).

## Init (summary)

1. If `wiki/` already has content → switch to **update**.
2. Survey the repo (targeted, not every file).
3. Write `wiki/_plan.md` (pages, evidence, open questions).
4. Execute the plan using [toc-and-layout.md](references/toc-and-layout.md) and [page-writing.md](references/page-writing.md).
5. Assemble: listings, root `log.md`, `.wiki-meta.json`.
6. Ensure top-level `AGENTS.md` / `CLAUDE.md` keep a short wiki pointer ([agents-pointer](references/agents-pointer.md)).
7. Write repo-root `.wikiignore` when missing ([wikiignore](references/wikiignore.md)).
8. Delete `wiki/_plan.md`. Report what was created.

## Update (summary)

1. Diff against `.wiki-meta.json` `commitHash` (and uncommitted changes).
2. Soft budget: few source files changed → touch few wiki pages; skip cosmetic edits.
3. Optional short `wiki/_plan.md` for the edit list; delete it when done.
4. Edit in place. Refresh listings/`log.md`/meta **only if content changed**.
5. Keep the AGENTS/CLAUDE wiki pointer present ([agents-pointer](references/agents-pointer.md)).
6. If nothing relevant changed, leave every file untouched and say the wiki is current.

## Ask (summary)

Answer from `wiki/` first; cite pages; verify source only when the wiki looks stale or incomplete. See [ask.md](references/ask.md).

## Output

`wiki/` with concept pages (frontmatter + `type`), listing `index.md` files, root `log.md`, and `.wiki-meta.json`. Plus a short pointer in top-level `AGENTS.md` and/or `CLAUDE.md`.
