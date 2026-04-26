# Setup — outstanding manual steps

These steps cannot be done from a shell session and need to be run
once by a human with the right scopes / UI access.

## 1. ANTHROPIC_API_KEY secret (Claude Code Action)

The `claude-code-reusable.yml` workflow needs `ANTHROPIC_API_KEY` to be
available as a GitHub Actions secret in any repo that uses it.

**Recommended**: set it once at the **organization** level, scoped to all
private repos. New repos inherit it automatically.

1. https://github.com/organizations/simply-complex-systems/settings/secrets/actions
2. New organization secret → name `ANTHROPIC_API_KEY` → paste key
3. Repository access: Selected repositories → choose all current private repos.
4. Future repos: when you create one, add it to this secret's repo list.

**Per-repo fallback** (if you don't want it org-wide yet):
```
gh secret set ANTHROPIC_API_KEY --repo simply-complex-systems/<repo>
```
(Reads from stdin; paste the key, hit Ctrl-D.)

## 2. Token scope upgrade for Projects v2

The current `gh` token has `repo`, `read:org`, `gist`, `admin:public_key`.
Projects v2 (the Kanban-style boards) need `read:project` and `project`.

Run once on each machine:
```
gh auth refresh -h github.com -s read:project,project
```

This opens a browser auth flow. After it completes, `gh project list
--owner simply-complex-systems` works, and a Claude Code session can
create / update Project boards programmatically.

## 3. Org-level Project board: "Simply Complex Systems — 2026"

Once Step 2 is done, create the cross-repo Project:

```
gh project create --owner simply-complex-systems --title "Simply Complex Systems — 2026"
```

Recommended fields (added via GraphQL or the Project UI):
- **Status**: Now / Next / Later / In Progress / Blocked / Done
- **Priority**: P0 / P1 / P2 / P3
- **Iteration**: ISO weeks (`2026-W18`, `2026-W19`, …)
- **Repo**: auto-populated from the issue/PR
- **Type**: Bug / Feature / Ops / ADR / Chore

Then add issues from each repo:
```
gh project item-add <project-number> --owner simply-complex-systems --url <issue-url>
```

## 4. Per-repo Actions permissions for release-please

Release-please needs Actions to be able to create PRs. On every repo that
uses the release workflow:

```
gh api -X PUT \
  "repos/simply-complex-systems/<repo>/actions/permissions/workflow" \
  -F can_approve_pull_request_reviews=true \
  -f default_workflow_permissions=write
```

Alternative: set this once at the org level via
https://github.com/organizations/simply-complex-systems/settings/actions
("Workflow permissions" → "Read and write" + "Allow GitHub Actions to create
and approve pull requests").

## 5. Dependabot

Enable org-wide:
https://github.com/organizations/simply-complex-systems/settings/security_analysis

Turn on:
- Dependency graph (already on for private repos by default)
- Dependabot alerts
- Dependabot security updates
- Dependabot version updates (requires per-repo `.github/dependabot.yml`)

A starter `dependabot.yml` lives at `docs/templates/dependabot.yml` once added.

## 6. Secret scanning

Same settings page. Turn on:
- Secret scanning (private repos require GitHub Advanced Security; the free
  tier covers public repos only — for the spine, this is sufficient).
- Push protection.
- Custom patterns (later, as patterns of leaked-thing-shape emerge).

## 7. P99 / cross-machine setup

Each machine that operates on this org runs:

```
# clone the public spine for offline reference
git clone git@github.com:simply-complex-systems/.github.git ~/code/.github

# bring the gh CLI's project scope online
gh auth refresh -h github.com -s read:project,project

# verify
gh project list --owner simply-complex-systems
```

The org spine is **public** specifically so that any machine can clone it
without auth gymnastics, and so that reusable workflows can be called from
private repos in the same org on the GitHub free tier.
