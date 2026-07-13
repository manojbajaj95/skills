# Ask

Answer repository questions using `wiki/` as the primary source instead of re-deriving everything from raw source.

## When to use

User asks how/where/why something works, where to change behavior, or to explain a subsystem — and a wiki may exist. Phrases like `/wiki ask`, "how does auth work", "where is billing handled".

## Steps

1. **Check for a wiki.** If `wiki/` is missing or empty, say so and offer **init**. You may still answer from source, but note the wiki is absent.
2. **Start at the entrypoint.** Read `wiki/index.md`, then follow links into `overview/` and relevant sections. Grep across `wiki/` for the question's key terms to find the canonical concept page.
3. **Answer from the wiki.** Cite specific wiki page paths. Surface inline source references (e.g. `apps/api/src/auth.ts`) so the user can jump to code.
4. **Verify only if needed.** If the wiki looks stale, contradicts the question, or lacks coverage, say so, confirm against source, then answer — and suggest **update** if the wiki is out of date.
5. **Stay focused.** Prefer the wiki's canonical explanation over guessing. Do not regenerate or wipe the wiki during ask.

## Output

A direct answer with wiki citations (and source paths when present). Optional one-line suggestion to run update if gaps were found.
