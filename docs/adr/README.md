# Architectural Decision Records (ADRs)

ADRs capture **locked decisions**: choices that would be expensive to
re-litigate, made at a specific point in time, in light of specific
context.

## When to write one

Write an ADR when any of these is true:
- The decision affects more than one file or more than one person.
- The reasoning behind it would be lost if not written down.
- A future contributor (or future you) is likely to ask "why did we do it this way?"
- The decision foreclosed alternatives that someone could otherwise re-open.

If a comment in a PR description would suffice, don't write an ADR.

## How to write one

1. Open an issue using the **ADR draft** template. Use the issue to discuss.
2. When the decision is locked, copy `0000-template.md` from this directory
   in `simply-complex-systems/.github` into the **target repo's** `docs/adr/`
   as `NNNN-short-slug.md` (next sequential number, kebab-case slug).
3. Fill in the template. Active voice in the decision: "We will use ...".
4. Open a PR titled `docs(adr): NNNN <short title>`. Merge after review.
5. Close the original ADR issue with a link to the merged ADR.

## Status field

- **Proposed** — under discussion. Lives only as an issue.
- **Accepted** — the canonical state of a merged ADR.
- **Deprecated** — superseded by another ADR. Add a `Superseded-By: NNNN` link
  at the top of the deprecated ADR. **Never delete an ADR.**
- **Superseded** — synonym for deprecated; pick one and stick with it.

## Format

We use **MADR-lite** (a stripped-down [Markdown Any Decision Record](https://adr.github.io/madr/)).
Sufficient for our scale, easy to read, easy to write.
