# Security Policy

## Supported versions

| Version | Supported |
|---------|-----------|
| `v1.x`  | Yes       |
| `< v1`  | No        |

The `v1` tag is a moving alias that always points at the latest backward-compatible commit. Immutable semver tags (e.g. `v1.0.0`) are kept for supply-chain pinning.

## Reporting a vulnerability

Please report security issues privately to **alex@agentskills.co.il** rather than opening a public GitHub issue.

Include:
- A description of the vulnerability and its impact
- Steps to reproduce
- Affected version (`v1`, `v1.0.0`, or commit SHA)

You can expect an acknowledgement within 72 hours and a fix or mitigation plan within 14 days for confirmed issues.

## Why this matters

This is a reusable GitHub Actions workflow with `id-token: write`, `contents: write`, and `attestations: write` permissions. A compromise here would affect every consuming repo, so we treat reports here as supply-chain critical.
