---
id: bb3abba7-d6a8-489e-b1a3-51998add556f
---
# TOC and layout

Read this when planning the table of contents or deciding what to document on init.

For writing prose and diagrams, see [page-writing.md](page-writing.md). 

For `.wiki-meta.json` (`commitHash` baseline), see [wiki-meta.md](wiki-meta.md).

## Bundle conventions

The wiki is a directory of markdown files. Two filenames are **reserved** at every level and are not concept pages:

| Filename | Role |
|----------|------|
| `index.md` | Directory listing for progressive disclosure (see below). Not a concept page. |
| `log.md` | Update history for that scope (root `log.md` is required on init; nested logs optional). |

Every other `.md` file is a **concept page** and MUST have YAML frontmatter with a non-empty `type` field — except ephemeral workflow scratch files (see `_plan.md` below).

**Security:** Respect repo-root `.wikiignore` and all `.gitignore` patterns ([wikiignore.md](wikiignore.md)). Never read or document secret values, credentials, private keys, tokens, or `.env` files. `.env.example` / sample configs are OK only with placeholders.

### `_plan.md` (ephemeral, not reserved)

During init or non-trivial update, the agent MAY write `wiki/_plan.md` as a temporary work queue. It is **not** a reserved OKF filename, not a concept page, and must **not** be linked from any `index.md`. Delete it before the run finishes — never leave it in the tree. See [init-and-update.md](init-and-update.md).

### Frontmatter (required on concept pages)

```yaml
---
type: <Type name>           # REQUIRED — see type vocabulary below
title: <Display name>       # Recommended
description: <One sentence> # Recommended — used in parent index.md listings
resource: <URI or path>     # Optional — canonical URI or repo path for a concrete asset
tags: [<tag>, …]            # Optional
timestamp: <ISO 8601>       # Optional — last meaningful change
---
```

Rules:

* Concept pages MUST start with a YAML frontmatter block (`---` … `---`).
* `type` is required and non-empty.
* After frontmatter, include a level-1 heading that matches `title` (or the concise page title rules below).
* Reserved files (`index.md`, `log.md`) do **not** use concept frontmatter (no `type`). Listing files normally have no frontmatter.
* Keep `.wiki-meta.json` at the bundle root for generation tracking (`commitHash`, `generatedAt`, `branch`). It is not a concept page.

### Type vocabulary

Use these `type` values unless the repo has a clearer domain-specific label. Consumers should tolerate unknown types; prefer this vocabulary for consistency.

| Type | Use for |
|------|---------|
| `Overview` | Section or project overview concept (`overview.md`) |
| `Architecture` | System architecture pages |
| `Getting Started` | Install / build / run onboarding |
| `Glossary` | Domain vocabulary |
| `Stats` | Quantitative codebase snapshot (`by-the-numbers.md`) |
| `Lore` | Narrative history / eras |
| `Fun Fact` | Trivia / easter eggs |
| `Playbook` | How-to contribute, workflow, debugging, testing, tooling, conventions |
| `Application` | Deployable app / service runtime |
| `System` | Internal building block / subsystem |
| `Feature` | Cross-cutting capability or workflow |
| `Package` | Workspace library / package |
| `Primitive` | Foundational domain object or shared concept |
| `API` | API surface or endpoint group |
| `Deployment` | Deploy / environments / infra |
| `Security` | Auth, trust boundaries, secrets |
| `Monitoring` | Logging, metrics, tracing, alerting |
| `Background` | Design decisions, pitfalls, migration context |
| `Cleanup` | Dead code, TODOs, hotspots, stale deps |
| `Reference` | Configuration, data models, dependency catalogs |
| `Maintainers` | Ownership / contributor mapping |

Do not invent a parallel type registry. If something does not fit, pick the closest type or a short descriptive string (e.g. `CLI Command`).

### `index.md` (listings only)

Every directory MUST contain an `index.md` that lists its children. These files are listings, not prose docs.

```markdown
# Section heading

* [Title 1](overview.md) - short description from the child's frontmatter
* [Title 2](architecture.md) - …
* [Subdirectory](apps/) - short description of the subdirectory
```

* No concept frontmatter on `index.md`.
* Prefer the child's `title` and `description` from frontmatter.
* Generate or refresh listings during assembly so they stay accurate.
* Bundle-root `index.md` lists top-level sections in the page-order sequence below.

### `overview.md` (replaces former content `index.md`)

Where this skill previously used `index.md` as the main content page for a directory, use `overview.md` instead:

| Old path | New path |
|----------|----------|
| `overview/index.md` (project overview content) | `overview/overview.md` |
| `how-to-contribute/index.md` (contribute intro content) | `how-to-contribute/overview.md` |
| `apps/index.md`, `features/index.md`, … (lens intros) | `apps/overview.md`, `features/overview.md`, … |
| `apps/cli/index.md` (complex-app intro content) | `apps/cli/overview.md` |

Each of those directories still has a separate listing `index.md`.

### Root `log.md` (required)

Always write a root `log.md` recording wiki updates for this generation. Newest dates first. Date headings MUST be ISO `YYYY-MM-DD`:

```markdown
# Wiki update log

## 2026-07-13
* **Creation**: Initial wiki generation from commit `abc123`.
* **Update**: Added [CLI](/apps/cli/overview.md) and [Sessions](/features/sessions.md).

## 2026-06-01
* **Update**: Refreshed stats and architecture after auth rewrite.
```

On update runs that change content, prepend a new date section (or extend today's section) rather than rewriting history. Skip `log.md` entirely on a true no-op update. Nested `log.md` files are optional.

## Plan the table of contents

Design a page tree before writing any prose. The wiki has three **content tiers** (structural groupings — not the same as subsystem coverage tiers below):


1. **Always-present pages** — overview, stats, lore, contribute, reference
2. **Organizational lenses** — apps, systems, features, packages, primitives
3. **Conditional sections** — api, deployment, security, monitoring, background, cleanup

### Always-present pages

These pages appear in every wiki, in this order:

Also always present at the bundle root: `index.md` (top-level listing) and `log.md` (update history).


1. `overview/` — introductory material grouped under one section
   * `index.md` — listing of overview children (not concept content)
   * `overview.md` — project overview: what it does, who uses it, quick links
   * `architecture.md` — system architecture with Mermaid diagrams
   * `getting-started.md` — prerequisites, install, build, test, run
   * `glossary.md` — project-specific terms and domain vocabulary
2. `by-the-numbers.md` — codebase statistics snapshot (see below)
3. `lore.md` — timeline and history of the codebase (see below)
4. `how-to-contribute/` — how to work in this codebase
   * `index.md` — listing of contribute children
   * `overview.md` — work pickup, PR process, review expectations, definition of done
   * `development-workflow.md` — branch, code, test, PR, merge cycle
   * `testing.md` — frameworks, patterns, how to run, mock, and cover
   * `debugging.md` — logs, common errors, troubleshooting runbook
   * `patterns-and-conventions.md` — error handling, coding style, cross-cutting concerns
   * `tooling.md` — build system, linters, code generators, CI tooling (if the repo's tooling is the product itself, promote this to a top-level section instead)

### By the numbers

A top-level `by-the-numbers.md` page that gives a quantitative snapshot of the codebase. Start the page with a "Data collected on [date]" note so readers know how current the numbers are.

Include these sections:

* **Size** — lines of code by language (with a Mermaid horizontal bar chart), total source files vs test files vs config files, package/module count for monorepos
* **Activity** — commits per week/month (recent trend), most actively changed files/directories in the last 90 days (churn hotspots)
* **Bot-attributed commits** — percentage of commits with bot co-authorship (e.g., `Co-authored-by: factory-droid[bot]`, `dependabot[bot]`, `github-actions[bot]`, `copilot[bot]`). This is a lower bound on AI-assisted work since inline AI tools like Copilot leave no trace in git history. Be transparent about what's counted.
* **Complexity** — average file size by directory, deepest import chains, number of exported symbols per package

Use Mermaid `xychart-beta` (horizontal bar charts) for language breakdown and any other stat where a visual helps. Do NOT use Mermaid `pie` charts — they are not supported by the renderer. Use tables for lists of files/directories.

**Never include individual contributor stats** (top committers, lines per person, leaderboards). The by-the-numbers page is about the codebase, not the people. Per-person metrics create toxic comparisons and don't belong in team documentation. The `maintainers.md` page handles ownership mapping separately.

**Inline stats in other pages:** In addition to this summary page, weave relevant stats into existing pages:

* Language breakdown in `architecture.md`
* Churn hotspots in `cleanup-opportunities/` (if that section exists)
* File counts, bus factor (unique committers), and test-to-code ratio per subsystem on each domain page
* Dependency counts in `reference/dependencies.md`

### Lore

A top-level `lore.md` page that tells the story of how the codebase evolved. This is a narrative history, not a technical reference. It answers "what happened here and when?"

**Boundaries with other sections:**

* `by-the-numbers.md` = current snapshot (what the codebase looks like today)
* `lore.md` = timeline and history (what changed and when)
* `log.md` = chronological wiki update log (what this generator changed)
* `fun-facts.md` = light trivia (easter eggs, amusing discoveries)
* `background/` = technical rationale (why decisions were made)

**Every event, era, and milestone must include a date or month** (e.g., "Mar 2023", "Q4 2024"). Derive dates from git commit timestamps, tag dates, and file creation dates. If an exact date isn't available, use the month of the earliest relevant commit.

Include these sections:

* **Eras** — group the codebase history into 3-8 major phases, each with a short narrative description and key event bullet points. Derive from git history: tag dates, large merge commits, contributor patterns, directory creation dates. Example: "The TypeScript Migration (Mar–Aug 2023): The entire backend was rewritten from JavaScript to TypeScript over 5 months..."
* **Longest-standing features** — code or subsystems that have survived the most refactors and are still actively used. Include when they were first introduced and how many changes they've weathered.
* **Deprecated features** — things that were built, used, and then removed or replaced. Identify from directory names, README mentions, obvious `@deprecated` annotations, and removed routes. What was the feature, when was it introduced, when was it deprecated, and what replaced it.
* **Major rewrites** — large changes that touched many files. What existed before, what replaced it, and when. Derive from git history (large PRs, branch names with "migration" or "rewrite").
* **Growth trajectory** — how the codebase expanded over time: when packages/apps were added, contributor growth signals from git log.

**Speculation:** When the "why" behind a change isn't clear from commits, use natural hedging language ("appears to have been", "likely replaced due to"). No special formatting for speculative content.

### Subsystem coverage tiers (init)

When surveying a repo for **init**, classify each subsystem into a coverage tier. These tiers control **how deep to document on the first run** — not where pages live in the TOC.

| Tier | Meaning | Examples | Init coverage |
|------|---------|----------|---------------|
| **Tier 1** | Core — repo is unusable or misleading without documenting this | Main entrypoints, primary app/service runtime, auth, core data store, build/run path, the product's reason to exist | **Must** get a solid concept page (or sub-pages) on init |
| **Tier 2** | Important — engineers hit this regularly but it is not the whole product | Major packages, shared libraries, cross-cutting features, CI/deploy path, public API surface | Document on init when page budget allows; otherwise stub with `description` + links and expand on **update** |
| **Tier 3** | Peripheral — helpful context, not blocking understanding | Thin utilities, legacy corners, dev-only scripts, rarely touched dirs | Skip on init unless the repo is small; add when a **update** diff touches them |

**How to assign tiers:** Use structural scan signals — README emphasis, entrypoints, traffic/import graphs, directory size and churn, test coverage density, and how often paths appear in AGENTS.md or CONTRIBUTING.md. When unsure between Tier 2 and Tier 3, prefer Tier 3 on first init (expand later).

**Page budget:** Cap at 200 concept pages per run. If Tier 1 + foundation pages exhaust the budget, defer Tier 2 stubs and all Tier 3 to a follow-up **update**. Prefer fewer solid Tier 1 pages over many shallow Tier 2/3 pages.

### Organizational lenses

Five lenses are available for organizing the codebase deep-dives. Use any combination based on what the repo actually contains. At least one lens is required. Most repos use 2-3. The **features** lens is strongly encouraged -- it's the most intuitive entry point for new engineers ("what does this thing do?"). Even small repos typically have user-visible or developer-visible capabilities worth documenting. Only skip it if the repo is a single-purpose library with no distinct features.

| Concept | Default label | Also called | When to use |
|---------|---------------|-------------|-------------|
| Deployable units | `applications/` | `services/`, `apps/` | Repo ships multiple distinct runtimes |
| Internal building blocks | `systems/`    | `services/`, `modules/`, `subsystems/` | Architectural components that don't map to a single app or package |
| Cross-cutting capabilities | `features/`   | `capabilities/`, `workflows/` | User-visible or developer-visible things that span multiple systems |
| Workspace packages | `packages/`   | `libraries/`, `crates/`, `modules/` | Monorepo with shared libraries worth documenting individually |
| Foundational domain objects | `primitives/` | `core-concepts/`, `domain-models/`, `entities/` | Types/concepts that appear across 3+ systems (e.g., session, user, message) |

**Choosing labels:** Mirror the repo's own vocabulary. If the repo has an `apps/` directory, call the section `apps/`, not `applications/`. If the repo calls things "services," use `services/`. The default labels are fallbacks for when the repo has no existing convention.

**Placement rules:**

* Place each concept where the repo's structure suggests it belongs. If agent logic lives in `packages/droid-core`, document it under packages, not systems.
* The systems lens is for things that don't have a natural home in apps or packages -- emergent architectural patterns, cross-package systems, infrastructure that spans multiple directories.
* Do not duplicate content across lenses. If something is documented under packages, the relevant app page should cross-link to it, not repeat it.

**Heuristics for identifying each lens:**

* If it has its own entry point and deployment, it's an **application**
* If it's a workspace package that other packages import, it's a **package**
* If it's a module with internal logic and clear boundaries that doesn't map to a single package, it's a **system**
* If it's a type or concept that appears in 3+ systems, it's a **primitive**
* If understanding it requires tracing through multiple systems or apps, it's a **feature**

### Conditional sections

Include these based on your judgment after surveying the repo. Skip any that don't apply.

* `api/` — if the repo exposes REST, GraphQL, WebSocket, or other APIs
* `deployment/` — if there's a non-trivial deployment process (CI/CD, environments, rollback, infrastructure)
* `security/` — if there are meaningful trust boundaries (auth, authorization, secrets, input validation)
* `background/` — if the repo has meaningful history (design decisions, pitfalls/danger zones, migration context)
* `how-to-monitor/` — if the repo runs as a service with logging, metrics, tracing, or alerting infrastructure
* `cleanup-opportunities/` — if the repo has dead code, accumulated TODOs/FIXMEs, oversized files, or outdated dependencies. Only include if there is actual content to report (see below)
* `fun-facts.md` — easter eggs, origin stories, oldest code, naming origins

### How to monitor

This conditional section documents how to see what the system is doing. Only generate it for repos that run as services with logging, metrics, or tracing infrastructure. Not applicable to libraries, CLI tools, or packages.

Sub-pages:

* `logging.md` — where logs go, how to query them, log levels and conventions, structured logging patterns, how to add new log statements
* `metrics.md` — what metrics are tracked, key SLIs/SLOs, available dashboards, how to add new metrics
* `tracing.md` — distributed tracing setup, how to trace a request end-to-end, span naming conventions, how to instrument new code paths
* `alerting.md` — what alerts exist, alert thresholds and rationale, escalation paths, known noisy alerts, how to add new alerts

Skip any sub-page the repo has no infrastructure for. If only one sub-page has content, collapse `how-to-monitor/` into a single `how-to-monitor.md` file instead of a directory.

### Cleanup opportunities

This conditional section surfaces actionable maintenance work. Only generate it if the scan finds meaningful content. Possible sub-pages:

* `dead-ends.md` — files, exports, or modules that nothing imports. The code equivalent of a ghost town.
* `todos-and-fixmes.md` — accumulated TODO, FIXME, and HACK comments with file locations. Include the oldest ones.
* `complexity-hotspots.md` — the largest source files, deepest nesting, or most complex functions. A gentle nudge toward refactoring.
* `dependency-freshness.md` — outdated or unmaintained dependencies. The oldest dependency still in use.

Skip any sub-page that has no findings. If only one sub-page has content, collapse `cleanup-opportunities/` into a single `cleanup-opportunities.md` file instead of a directory.

### Maintainers

Include a top-level `maintainers.md` page that maps subsystems to the people who know them. This page uses two data sources:

* **CODEOWNERS file** (if it exists) — official ownership assignments
* **Git blame / git log** — the 2-3 most recent or frequent committers per directory or subsystem

Present as a table:

```markdown
| Subsystem      | Official owners (CODEOWNERS) | Recent contributors (git history) | Last activity |
| -------------- | ---------------------------- | --------------------------------- | ------------- |
| Authentication | @alice                       | alice, bob                        | 2 weeks ago   |
| CLI            | @charlie, @dave              | charlie, eve                      | 3 days ago    |
```

If the repo has no CODEOWNERS file, omit that column and derive all data from git history. If the repo has very few contributors (e.g., a solo project), skip this page entirely.

### Per-page active contributors

Each domain page (apps, systems, features, packages, primitives) should include an "Active contributors" byline as the very first line after the page heading (after frontmatter), before the Purpose section:

```markdown
---
type: Feature
title: Authentication
description: How auth works across the stack.
---

# Authentication

Active contributors: alice, bob

## Purpose

...
```

Derive the names from CODEOWNERS (if available) merged with the top 2-3 recent committers from git blame for that subsystem's directory. Use first names or GitHub usernames, no @ symbols.

**Exclude bot accounts** from contributor lists — filter out usernames ending in `[bot]` (e.g., `factory-droid[bot]`, `dependabot[bot]`, `github-actions[bot]`). Bots are not people you'd reach out to with questions. This applies to both the per-page active contributors byline and the maintainers page.

**Use the default branch for contributor data.** When deriving contributors from git blame or git log, always query against the default branch (`main` or `dev`), not the current branch. Feature branches skew contributor data toward whoever is working on that branch. Use `git log origin/main -- <path>` or `git log origin/dev -- <path>` to get accurate contributor history.

### Bottom sections

These appear at the end of every wiki:

* `reference/` — configuration, data models, external dependencies (`index.md` listing + concept pages; use `overview.md` if the section needs an intro concept)
* `maintainers.md` — subsystem ownership table (conditional, skip for solo projects; always the very last entry in root `index.md`)

### Page ordering

Navigation order is encoded in `index.md` listings — root `wiki/index.md` for top-level sequence, each section's `index.md` for sibling order. There is no separate order manifest in `.wiki-meta.json`.

The full ordering in the wiki is:


 1. Root `index.md` (listing) and root `log.md` (not concept pages; still emit `log.md` every run)
 2. overview/ (`overview.md`, architecture, getting-started, glossary — plus directory `index.md`)
 3. by-the-numbers.md (if present)
 4. lore.md (if present)
 5. fun-facts.md (if present)
 6. how-to-contribute/
 7. [organizational lenses, in whatever order makes sense]
 8. [conditional sections: api, deployment, security, how-to-monitor, background, cleanup-opportunities]
 9. reference/
10. maintainers.md (if applicable, always last)

**Ordering rules:**

* Each concept page stays in its defined position regardless of whether it has children. `by-the-numbers.md` appears after `overview/` even though it has no children, not at the top with other childless pages — reflect that in root `index.md` bullet order.
* Refresh `index.md` listings on assembly so link order matches this sequence. Listings (`index.md`) and logs (`log.md`) are not concept pages.
* Within a lens section (e.g., `apps/`), order concept pages from most important to least important in that directory's `index.md`. Put `overview.md` first among concepts.
* Conditional sections appear in the order listed above (api → deployment → security → how-to-monitor → background → cleanup-opportunities), not alphabetically.

### Nesting rules

* Any concept page can expand into a directory with sub-pages, except the overview-section concept files (`overview/overview.md`, `architecture.md`, `getting-started.md`, `glossary.md`) which stay single files under `overview/`
* Maximum depth: 2 levels from any lens root (e.g., `apps/cli.md` or `apps/cli/overview.md` + `apps/cli/tui-rendering.md`). No deeper.
* Every directory must contain a listing `index.md`. Directory intro content lives in `overview.md`, not `index.md`.
* For large repos (50+ source directories or 10+ distinct subsystems), lean toward splitting pages rather than cramming. A 3000-word page covering an entire subsystem is less useful than three focused pages covering its distinct aspects. Critical sub-agents decide whether to create sub-pages based on what they find in the code.
* For small repos, default to single pages and only split when a topic has clearly distinct sub-areas
* Deployment and security start as single pages; expand to directories only if the repo has enough substance

### Naming rules

* Use lowercase filenames with hyphens: `getting-started.md`, not `GettingStarted.md`
* File names use lowercase with hyphens. No spaces, no uppercase.

### Page title rules

Page titles come from frontmatter `title` and the `# Heading` after frontmatter. They should be concise noun phrases that match how the team refers to the thing. The section hierarchy already provides context, so titles should not repeat it.

* **Don't prepend directory paths.** Title is "CLI", not "apps/cli — CLI Architecture".
* **Don't append generic suffixes.** Title is "Apps", not "Apps Overview". Title is "Packages", not "Packages — Overview". The only exception is `overview/overview.md`, which may include the project name (e.g., "Acme platform overview").
* **Don't repeat the parent section name.** A page at `features/sessions.md` is titled "Sessions", not "Features — Sessions".
* **Match the team's vocabulary.** If the team calls it "the daemon", title is "Daemon", not "Background Service Process".
* **Keep it short.** Aim for 1-3 words. If a title needs more, the page probably covers too much and should be split.

### Update mode (no wipe)

When updating an existing wiki, start from the existing TOC (`index.md` listings and on-disk tree). Never discard the tree. Only plan:

* **Pages to update** — made inaccurate or incomplete by the source diff
* **Pages to create** — new subsystems/features that need pages
* **Pages to remove** — deleted subsystems only
* **Pages to leave untouched** — everything else (edit in place; do not regenerate)

Special page rules:

* Prefer **not** refreshing `by-the-numbers.md` / `lore.md` / fun-facts on small diffs; refresh stats only when the user asked for a broader sync or the numbers are clearly misleading
* Root `log.md` gets a new entry only when concept content actually changed
* Parent `index.md` listings refresh when children are added, removed, or retitled

## File structure specification

The generated wiki follows this layout:

```
wiki/
├── .wiki-meta.json                       # Generation tracking (keep)
├── index.md                              # Root listing (reserved)
├── log.md                                # Root update log (reserved, required)

# Always present (in this order)
├── overview/                             # Introductory material
│   ├── index.md                          # Listing of overview children
│   ├── overview.md                       # Project overview (concept)
│   ├── architecture.md                   # System architecture with Mermaid diagrams
│   ├── getting-started.md                # Prerequisites, install, build, test, run
│   └── glossary.md                       # Project-specific terms and vocabulary
├── by-the-numbers.md                     # Codebase statistics snapshot
├── lore.md                               # Timeline, eras, deprecated features, rewrites
├── fun-facts.md                          # Easter eggs, origin stories, oldest code
├── how-to-contribute/                    # How to work in this codebase
│   ├── index.md                          # Listing
│   ├── overview.md                       # Contribute intro (concept)
│   ├── development-workflow.md
│   ├── testing.md
│   ├── debugging.md
│   ├── patterns-and-conventions.md
│   └── tooling.md

# Organizational lenses (use any combination, at least one required)
# Labels mirror the repo's own vocabulary
├── <apps|services|applications>/         # Deployable units
│   ├── index.md                          # Listing
│   ├── overview.md                       # Lens intro (concept)
│   ├── <simple-app>.md                   # Single page for simple apps
│   └── <complex-app>/                    # Directory for complex apps
│       ├── index.md                      # Listing
│       ├── overview.md                   # App intro (concept)
│       └── <sub-topic>.md
├── <systems|modules|subsystems>/         # Internal building blocks
│   ├── index.md
│   ├── overview.md
│   ├── <simple-system>.md
│   └── <complex-system>/
│       ├── index.md
│       ├── overview.md
│       └── <sub-topic>.md
├── <features|capabilities|workflows>/    # Cross-cutting capabilities
│   ├── index.md
│   ├── overview.md
│   ├── <simple-feature>.md
│   └── <complex-feature>/
│       ├── index.md
│       ├── overview.md
│       └── <sub-topic>.md
├── <packages|libraries|crates>/          # Workspace packages
│   ├── index.md
│   ├── overview.md
│   ├── <simple-package>.md
│   └── <complex-package>/
│       ├── index.md
│       ├── overview.md
│       └── <sub-topic>.md
├── <primitives|core-concepts|entities>/  # Foundational domain objects
│   ├── index.md
│   ├── overview.md
│   └── *.md

# Conditional sections (LLM judgment)
├── api/                                  # If the repo exposes APIs
│   ├── index.md
│   ├── overview.md
│   └── *.md
├── deployment.md                         # Single page or directory
├── security.md                           # Single page or directory
├── how-to-monitor/                       # Logging, metrics, tracing, alerting (services only)
│   ├── index.md
│   ├── overview.md
│   ├── logging.md
│   ├── metrics.md
│   ├── tracing.md
│   └── alerting.md
├── background/
│   ├── index.md
│   ├── overview.md
│   └── *.md
├── cleanup-opportunities/
│   ├── index.md
│   ├── overview.md
│   ├── dead-ends.md
│   ├── todos-and-fixmes.md
│   ├── complexity-hotspots.md
│   └── dependency-freshness.md

# Always present (bottom)
├── reference/
│   ├── index.md
│   ├── overview.md
│   ├── configuration.md
│   ├── data-models.md
│   └── dependencies.md
└── maintainers.md                        # Subsystem ownership table (conditional, always last)
```

**Rules:**

* Every **concept** `.md` file MUST have YAML frontmatter with a non-empty `type`, then a level-1 heading matching `title`.
* `index.md` and `log.md` are reserved: listings and update logs only — not concept pages.
* Every directory must contain a listing `index.md`. Former content indexes live at `overview.md`.
* Root `log.md` is required on every generation.
* File names use lowercase with hyphens. No spaces, no uppercase.
* `.wiki-meta.json` is for generation tracking (`commitHash`, `generatedAt`, `branch`) and is not a concept page.
* The overview-section concept files (`overview/overview.md`, `architecture.md`, `getting-started.md`, `glossary.md`) stay single files. Other topics can expand into directories with sub-pages.
* Maximum tree depth: 2 levels from any lens root (e.g., `apps/cli/command-structure.md`). No deeper.
* For large repos, critical sub-agents decide whether to split into sub-pages. A complex subsystem like an editor core or extension host should have its own directory with focused sub-pages, not a single monolithic page.
* Maximum 200 concept pages per wiki run. If a project needs more, prioritize Tier 1 subsystems first.
