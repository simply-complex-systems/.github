# simply-complex-systems / .github

The **org spine**. Shared issue templates, PR templates, reusable workflows,
ADR conventions, and the Claude Code Action wiring used across every
`simply-complex-systems` repo.

This repo is **public** by deliberate exception to the org's default-private
posture. Two reasons it has to be public:
1. GitHub free-tier orgs cannot share private-repo reusable workflows across
   repos. Public is the only way the spine workflows are callable from every
   repo in the org.
2. Default community health files (issue templates, PR template, SECURITY.md,
   CONTRIBUTING.md) inherit cleanly from a public `.github` repo to all repos.

Nothing sensitive lives here — only conventions, templates, and wirings to
public Actions. Project repos remain private.

See [SETUP.md](./SETUP.md) for outstanding manual setup (Actions secrets,
Projects v2 scopes, Dependabot, secret scanning, multi-machine bring-up).

## What lives here

| Path | Purpose | Propagates? |
|------|---------|-------------|
| `.github/ISSUE_TEMPLATE/*` | Bug, feature, ops-task, ADR templates + chooser | Yes — to repos without their own |
| `.github/PULL_REQUEST_TEMPLATE.md` | What / why / test plan / risk | Yes |
| `SECURITY.md`, `CONTRIBUTING.md` | Org-wide policy | Yes |
| `.github/workflows/*-reusable.yml` | Reusable workflows (Claude Code Action, release-please, CI) | Called by `uses:` from other repos |
| `docs/adr/0000-template.md` | Architectural Decision Record template (MADR-lite) | Copy into each repo's `docs/adr/` |
| `docs/templates/CODEOWNERS` | Per-repo `CODEOWNERS` starter | Copy into each repo's `.github/` |
| `profile/README.md` | Public org profile (overridden by each repo's own README) | Org profile only |

## Adopting this in a new repo

1. Create the repo `--private`.
2. Add `.github/CODEOWNERS` at minimum (CODEOWNERS does NOT propagate).
3. Reference the reusable workflows from your repo's `.github/workflows/`:

```yaml
# .github/workflows/claude.yml in any repo
name: Claude Code
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]
  pull_request_review:
    types: [submitted]

jobs:
  claude:
    uses: simply-complex-systems/.github/.github/workflows/claude-code-reusable.yml@main
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

4. Set `ANTHROPIC_API_KEY` as a repo or org Actions secret.
5. Open issues using the templates in this repo (auto-served).

## Conventions enforced by this spine

- **Branching**: GitHub Flow. `main` is shippable. One branch per piece of work.
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/) — `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `ci:`. Powers automated releases.
- **Versioning**: Semver via `release-please`. Tags + auto-generated changelog.
- **Reviews**: At least one approval before merge. CODEOWNERS auto-routes.
- **Priority labels**: `P0` (now / blocking), `P1` (this iteration), `P2` (next iteration), `P3` (later).
- **Iteration field** in Projects: ISO week buckets (`2026-W18`).
- **Triage SLA**: every new issue labeled + prioritized within 24 h.

## ADRs

Architectural Decision Records — one short markdown file per locked decision,
numbered, dated, immutable. Lives in each repo's `docs/adr/`. Use the template
at `docs/adr/0000-template.md` in this repo.

A decision becomes an ADR the moment it would be expensive to re-litigate.
