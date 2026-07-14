---
id: 780c9aef-05dc-4e66-834c-3f7cc502ff71
name: engineering-principles
description: >-
  Apply shared engineering principles (YAGNI, deep modules, trusted libraries,
  separation of concerns, leave it better) when writing or changing code, or
  when drafting CONTRIBUTING.md. Use whenever the user asks for design guidance,
  contribution guidelines, how to structure changes, or to follow project
  engineering principles — even if they do not name this skill.
---
# Engineering Principles

These principles apply to contributions — human and AI alike. Prefer them when implementing features, reviewing design choices, or writing `CONTRIBUTING.md`.

Explain the **why** briefly when a principle changes a decision; do not lecture on every edit.

## YAGNI — You Aren't Gonna Need It

Implement only what the current task demands. Build for today's requirements; future requirements arrive with future context.

## Use trusted libraries over reinventing

Reach for a well-maintained dependency before writing your own crypto, HTTP client, or token parser. The goal is a working solution, not a showcase of custom implementations. When a library exists and is maintained, use it.

## Deep modules over shallow ones

Prefer a module with a small surface area and rich internals over a sprawl of thin wrappers. A single cohesive client that handles a concern cleanly beats a dozen one-method classes. More files is not more modular.

## Single responsibility and separation of concerns

Keep boundaries clear — e.g. auth authenticates, storage stores, CLI presents. A flow should not write to storage, and storage should not know about OAuth. If a function is hard to name, it is doing too many things.

## Premature optimization is evil

Do not add caching, batching, or concurrency before there is a measured performance problem. Simple code that is slow is fixable; complex code that is wrong is not.

## Don't do it just because you can

If a feature, abstraction, or refactor is not directly solving a real problem that exists today, skip it.

## Leave it better than you found it

Every change is an opportunity to fix a nearby typo, remove a dead import, or clarify a confusing comment not the whole file, just the immediate vicinity. Small improvements compound.

## Comment the why, not the what

Use clear google-style docstrings for public interfaces . Inline comments should explain non-obvious invariants, workarounds, or hidden constraints.

## Update docs alongside code

If you change behavior, update the relevant docstring, `README.md`, or `docs` in the same change. Documentation debt accumulates faster than technical debt and is harder to pay down later.

## When writing CONTRIBUTING.md

Keep the file short. Summarize how to run, test, lint, and open PRs. Link or briefly list these principles instead of pasting a long essay that will drift out of date.
