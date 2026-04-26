# Security policy

## Reporting a vulnerability

Email **epedersen1981@gmail.com** with subject `SECURITY: <repo-name>`.
Do not open a public issue or discuss in PR comments.

## Secret handling

This org follows a defense-in-depth posture for secrets:

- **Runtime secrets** (services on R2D2): Docker Secrets via single-node Swarm.
  Files mounted at `/run/secrets/` only. Plaintext on disk is a finding.
- **Operator secrets** (CLI tooling): OS keyring (`secret-tool` / Keychain).
- **CI secrets**: GitHub Actions Secrets, repo-scoped or org-scoped.
- **Network**: Tailscale-only ingress on production hosts.

## Scanning

GitHub secret scanning and Dependabot security updates are expected to be
**on** for every repo in this org. If you find a repo where they are off,
turn them on or open an issue.

## What counts as a vulnerability here

- Plaintext credentials committed to a repo
- Public repo containing data covered by FERPA, HIPAA, or other regulated PII
- Production services reachable outside the tailnet without explicit auth
- Dependencies with known CVEs above CVSS 7 left unpatched > 14 days
