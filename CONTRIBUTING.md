# Contributing

This file applies org-wide unless a specific repo overrides it.

## Workflow

We use **GitHub Flow**.

1. Open or pick up an issue. Triage labels + priority within 24 h.
2. Branch from `main`. Branch name: `<type>/<issue-number>-<short-slug>` —
   e.g. `feat/12-add-specials-json`, `fix/47-menu-overflow`.
3. Commit using **Conventional Commits**:
   - `feat:` user-visible new capability
   - `fix:` user-visible bug fix
   - `chore:` maintenance (deps, tooling)
   - `docs:` documentation only
   - `refactor:` code change with no behavior change
   - `test:` test-only changes
   - `ci:` CI/CD changes
   - `perf:` performance improvement
4. Open a PR with the template filled out. Reference the issue (`Closes #N`).
5. Wait for at least one approval. CODEOWNERS are auto-requested.
6. Merge — squash by default. Branch is auto-deleted.
7. `release-please` opens or updates a release PR on `main`.
8. Merge the release PR when you want to ship; tag is created automatically.

## Issue priorities

- **P0** — now / blocking. Drop other work.
- **P1** — this iteration. Should ship this week.
- **P2** — next iteration.
- **P3** — later. May never ship; that's fine.

## Iteration field

ISO week buckets: `2026-W18`, `2026-W19`, etc. Set on Project board entries.

## Definition of Done

A change is done when:
- [ ] Code merged to `main`
- [ ] Tests cover the new behavior (or risk explicitly noted)
- [ ] Docs / README / ADR updated if behavior or interface changed
- [ ] Issue closed with a summary comment if non-trivial

## Asking Claude Code to do work

In any repo with the Claude Code Action wired up, comment on an issue or PR:

```
@claude please add a specials.json loader and refactor build_menu.py to read from it. PR back to this issue.
```

Claude will check out the branch, do the work, and open a PR. Review at the PR gate.
