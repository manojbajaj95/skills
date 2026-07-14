---
id: 6447d76f-f915-423f-b966-8e298d0912ae
---
# Init and update

How to **init** a new `wiki/` or **update** an existing one with small surgical edits. Apply [toc-and-layout.md](toc-and-layout.md), [page-writing.md](page-writing.md), and [wiki-meta.md](wiki-meta.md) (index: [structure-and-format.md](structure-and-format.md)). Wire the agent pointer with [agents-pointer.md](agents-pointer.md).


---

## Route: init vs update

| Condition | Action |
|-----------|--------|
| No `wiki/`, or `wiki/` empty / no concept `.md` files | **Init** |
| `wiki/` has content (even without `.wiki-meta.json`) | **Update** — never overwrite from scratch |
| User said "generate" / "rebuild" but wiki exists | **Update**, and say you will not wipe |
| User insists on starting over | Refuse wipe; offer update, or ask them to delete `wiki/` themselves first |


---

## Init

### 1. Survey (targeted)

Build a mental model without reading every file.

**Structural scan** (when present): `README.md`, `AGENTS.md`, `CONTRIBUTING.md`, package manifests, `docs/`, entry points, CI/build/lint config, top-level and key subdirectory listings.

Map: purpose, major subsystems, key data flows, external dependencies, build/test commands.

**Deep scan** (signals only): feature flags, routes, API groups, `src/features|modules|domains/`, services/workers, non-obvious domain dirs.

**Coverage:** Classify subsystems into Tier 1 / 2 / 3 per [toc-and-layout.md — Subsystem coverage tiers](toc-and-layout.md#subsystem-coverage-tiers-init). Prefer a maintainable first wiki over exhaustive coverage — lean toward fewer solid Tier 1 pages; expand Tier 2/3 later via **update**.

**Ignore rules:** Read `.wikiignore` at repo root and respect all `.gitignore` patterns before opening source files ([wikiignore.md](wikiignore.md)). Never read `.env`, `.agents/`, `.cursor/`, or other excluded paths. Document only that config exists and where non-sensitive setup lives.

Produce a compact **survey context** (repo summary, architecture, topics, patterns, glossary seeds, directory map) for later steps / sub-agents.

### 2. Write `wiki/_plan.md`

Before creating concept pages, write a plan file at `wiki/_plan.md`:

```markdown
# Wiki init plan

## Survey summary
…

## Planned pages
| Path | Title | Type | Why / source evidence |
| --- | --- | --- | --- |
| overview/overview.md | … | Overview | … |

## Open questions
- …
```

This plan is the work queue. Do not leave it in the tree when finished.

### 3. Execute the plan

Follow [toc-and-layout.md](toc-and-layout.md) and [page-writing.md](page-writing.md):


1. Create listing `index.md` files and concept pages with required frontmatter + `type`.
2. Section intros use `overview.md` (not content `index.md`).
3. Foundation first: `overview/overview.md`, `architecture.md`, `getting-started.md`, `glossary.md`, then `how-to-contribute/patterns-and-conventions.md`.
4. Then lens / data / remaining pages from the plan (sub-agents OK for parallel depth).

### Execution DAG (init)

```
1. SURVEY → survey_context
2. WRITE wiki/_plan.md
3. FOUNDATION pages (sequential)
4. LENS + DATA pages (parallel as needed)
5. REMAINING pages (parallel as needed)
6. ASSEMBLY — listings, log.md, .wiki-meta.json, AGENTS pointer, repo-root .wikiignore
7. DELETE wiki/_plan.md
```

### Sub-agent prompt (when used)

```
You are writing wiki page(s) for [repo].

## Shared Context
[survey_context]

## Your Assignment
Pages: …
Criticality: critical | normal
Content brief: …
Relevant source paths: …

## Related Pages (link — do not duplicate)
- …

## Rules
- Follow toc-and-layout + page-writing (frontmatter + type, overview.md for section intros, listings in index.md)
- Max nesting: 2 levels from a lens root
- Critical: may add sub-pages with overview.md + listing index.md
- Cross-link related pages; prefer bundle-absolute links (/path.md)
- Write under wiki/
- Respect .wikiignore and .gitignore; never document secrets or read .env
```

### 4. Assemble (init)

* Cross-link audit; full repo-root paths in file mentions
* Refresh every directory `index.md` listing from child frontmatter
* Write root `log.md` (Creation entry for this commit)
* Write `.wiki-meta.json` (`commitHash`, `generatedAt`, `branch`)
* Ensure AGENTS/CLAUDE wiki pointer ([agents-pointer.md](agents-pointer.md))
* Write repo-root `.wikiignore` when missing ([wikiignore.md](wikiignore.md))
* **Delete** `**wiki/_plan.md**`
* Report pages created and any caveats

## Update

Surgical maintenance only. Preserve accurate structure and wording.

### 1. Locate baseline


1. Require an existing `wiki/` with content; otherwise send the user to **init**.
2. Read `.wiki-meta.json` when present. Prefer `commitHash` as baseline.
3. If meta/commit missing: infer from `git log` on `wiki/`, or treat as "update carefully from current tree" without a wipe.

### 2. Find what changed

```bash
git diff --stat <wikiCommit> HEAD -- . ':!*.lock' ':!package-lock.json' ':!*.generated.*'
git status
git diff --stat  # uncommitted
```

If git fails (shallow clone, bad hash): inspect likely-touched areas from the user focus or recent file mtimes still **edit in place**, never delete the wiki tree.

If the diff is empty and there are no relevant uncommitted changes → **no-op**: say the wiki is current; do not touch meta, `log.md`, or pages.

### 3. Soft edit budget

| Source change scale | Wiki edit budget |
|---------------------|------------------|
| No relevant changes | **No-op** — zero file writes |
| \~1–5 source files  | Prefer **1–2** wiki pages; avoid root listing churn unless navigation changed |
| Moderate / focused feature | Only pages made wrong or incomplete by the diff |
| Large diff          | Still **no wipe**. Batch surgical edits; prioritize inaccurate pages. Skip refresh of lore/stats unless clearly warranted (see [toc-and-layout.md](toc-and-layout.md)) |

Additional rules:

* Prefer replacing a stale sentence over adding paragraphs.
* **No formatting-only edits** (tables, blank lines, list order, polish) unless that block is already changing for accuracy.
* Do not refresh `by-the-numbers.md` / `lore.md` / fun-facts unless the delta makes them materially wrong (stats always OK to refresh if you are already editing for a substantial sync and the user asked for a full refresh).
* Remove concept pages only when the underlying subsystem is gone; update parent listings.

### 4. Optional `wiki/_plan.md` (update)

For non-trivial updates, write a short plan:

```markdown
# Wiki update plan

Baseline: <commitHash>
## Edits
| Page | Change | Why (source evidence) |
| --- | --- | --- |
```

Execute it, then **delete** `**wiki/_plan.md**`. Skip the plan file for a one-line fix.

### 5. Edit in place

* Update only planned pages; keep frontmatter `type` / refresh `timestamp` when content changes.
* Pass existing page content into any sub-agent so it preserves structure.
* After content changes: refresh affected `index.md` listings, prepend root `log.md`, update `.wiki-meta.json` `commitHash` when source commit advances.
* Ensure AGENTS/CLAUDE pointer still present.
* If you made **no** content edits → do not rewrite meta or log (true no-op).

### 6. Report

List pages changed and why, or that the wiki was already current.


---

## Shared assembly checklist

After init or a content-changing update:


1. Concept pages have frontmatter + `type`
2. Every directory has listing `index.md`; intros are `overview.md`
3. Root `log.md` updated (init: required; update: only if content changed)
4. `.wiki-meta.json` written/updated when content changed
5. AGENTS/CLAUDE pointer present
6. `wiki/_plan.md` deleted
7. Repo-root `.wikiignore` present ([wikiignore.md](wikiignore.md))
8. No secrets documented
